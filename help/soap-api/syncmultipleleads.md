---
title: Mehrere Leads synchronisieren
feature: SOAP
description: Erfahren Sie, wie Sie mit syncMultipleLeads mehrere Marketo-Leads über SOAP, Schlüssel und Deduplizierungsregeln, Batch-Größenbeschränkungen und Beispiel-XML-, PHP- und Java-Code upsertieren können.
exl-id: 91980b82-dff9-48a7-b03e-20dce9d0d046
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 4%

---

# Mehrere Leads synchronisieren

Diese Funktion fordert einen Einfüge- oder Aktualisierungsvorgang (upsert) für (_)_ Lead-Datensätze an. Beim Aktualisieren eines bestehenden Leads kann der Lead mit einem der folgenden Schlüssel identifiziert werden:

- Marketo-ID
- ID des Fremdsystems
- E-Mail

Wenn mehr als ein Schlüssel vorhanden ist, hat die Marketo-ID Vorrang vor `ForeignSysPersonId`, und letztere wird aktualisiert. Wenn jedoch auch E-Mail als Schlüssel vorhanden ist, wird sie nicht aktualisiert, es sei denn, sie wird in der Liste der Attribute angegeben.

Unsere Empfehlung lautet, dass die Losgrößen nicht höher als 300 sind. Höhere Größen werden nicht unterstützt und können zu Zeitüberschreitungen und in extremen Fällen zu Einschränkungen führen.

Mit diesem Funktionsaufruf können Sie die Deduplizierungsfunktion deaktivieren. Wenn dedupEnabled auf „true“ gesetzt ist und keine andere eindeutige Kennung angegeben wird (`foreignSysPersonId` oder Marketo-Lead-ID), wird der Lead-Datensatz mithilfe der E-Mail-Adresse dedupliziert. Beachten Sie, dass durch die Übergabe von „false“ Duplikate in Marketo erstellt werden.

## Anfrage

| Feldname | Erforderlich/Optional | Beschreibung |
| --- | --- | --- |
| leadRecordList->leadRecord | Erforderlich | Array von LeadRecords, die Sie synchronisieren möchten. LeadRecords muss die Lead-ID, E-Mail-Adresse oder ForeignSysPersonId angeben |
| dedupEnabled | optional | Optionaler Wert, mit dem Sie die Deduplizierungsfunktion deaktivieren können. Die Übergabe des Werts `false` führt zu Duplikaten in Marketo |

## Anfrage-XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>b5e21953ae9f1b263e644da5eccce9ff33802513</requestSignature>
      <requestTimestamp>2013-08-01T18:22:30-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMultipleLeads>
      <leadRecordList>
        <leadRecord>
          <Email>em1000@etestd.marketo.net</Email>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1000</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1000</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
        <leadRecord>
          <Email>em1001@etestd.marketo.net</Email>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1001</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1001</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
      </leadRecordList>
      <dedupEnabled>true</dedupEnabled>
    </ns1:paramsSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Antwort-XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMultipleLeads>
      <result>
        <syncStatusList>
          <syncStatus>
            <leadId>1090240</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
          <syncStatus>
            <leadId>1090239</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
        </syncStatusList>
      </result>
    </ns1:successSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Beispielcode - PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = "";  // CHANGE ME
$marketoSecretKey   = "";  // CHANGE ME
$marketoNameSpace       = "http://www.marketo.com/mktows/";

// Create Signature
$dtzObj = new DateTimeZone("America/Los_Angeles");
$dtObj  = new DateTime('now', $dtzObj);
$timeStamp = $dtObj->format(DATE_W3C);
$encryptString = $timeStamp . $marketoUserId;
$signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);

// Create SOAP Header
$attrs = new stdClass();
$attrs->mktowsUserId = $marketoUserId;
$attrs->requestSignature = $signature;
$attrs->requestTimestamp = $timeStamp;
$authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
$options = array("connection_timeout" => 15, "location" => $marketoSoapEndPoint);
if ($debug) {
  $options["trace"] = 1;
}

// Create Request
for ($i=1000; $i < 1002; ++$i)
{
  $uid = $i;
  $attrs = array();
  $attr1 = new stdClass();
  $attr1->attrName = "Company";
  $attr1->attrValue = "Marketo$uid";
  $attr2 = new stdClass();
  $attr2->attrName = "Phone";
  $attr2->attrValue = "650-555-$uid";
  $attrs[] = $attr1;
  $attrs[] = $attr2;

  $attrList = new stdClass();
  $attrList->attribute = $attrs;

  $leadRec = new stdClass();
  $leadRec->Email = 'em' . $uid . '@etestd.marketo.net';
  $leadRec->leadAttributeList = $attrList;
  $leads[] = $leadRec;
}
$leadRecordList = new stdClass();
$leadRecordList->leadRecord = $leads;
$params = new stdClass();
$params->leadRecordList = $leadRecordList;
$params->dedupEnabled = true;
$soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
try {
  $leads = $soapClient->__soapCall('syncMultipleLeads', array($params), $options, $authHdr);
  //      print_r($leads);
}
catch(Exception $ex) {
  var_dump($ex);
}
if ($debug) {
  print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
  print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
}
?>
```

## Beispielcode - Java

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;

public class SyncMultipleLeads {

    public static void main(String[] args) {
        System.out.println("Executing syncMultipleLeads");
        try {
            URL marketoSoapEndPoint = new URL("https://100-AEK-913.mktoapi.com/soap/mktows/2_1" + "?WSDL");
            String marketoUserId = "demo17_1_809934544BFABAE58E5D27";
            String marketoSecretKey = "27272727aa";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

            ObjectFactory objectFactory = new ObjectFactory();

            JAXBElement<Boolean> dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
            request.setDedupEnabled(dedup);

            ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

            // Create First Lead Record
            LeadRecord rec1 = new LeadRecord();

            JAXBElement<String> email = objectFactory.createLeadRecordEmail("t@t.com");
            rec1.setEmail(email);

            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George");

            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");

            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);

            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttribute> attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

            rec1.setLeadAttributeList(attrList);
            arrayOfLeadRecords.getLeadRecords().add(rec1);

            // Create Second Lead Record
            LeadRecord rec2 = new LeadRecord();

            JAXBElement<String> email2 = objectFactory.createLeadRecordEmail("myemail@test.com");
            rec2.setEmail(email2);

            Attribute attr11 = new Attribute();
            attr11.setAttrName("FirstName");
            attr11.setAttrValue("Nancy");

            Attribute attr21 = new Attribute();
            attr21.setAttrName("LastName");
            attr21.setAttrValue("Lady");

            ArrayOfAttribute aoa2 = new ArrayOfAttribute();
            aoa2.getAttributes().add(attr11);
            aoa2.getAttributes().add(attr21);

            qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttribute> attrList2 = new JAXBElement(qname, ArrayOfAttribute.class, aoa2);

            rec2.setLeadAttributeList(attrList);
            arrayOfLeadRecords.getLeadRecords().add(rec2);

            request.setLeadRecordList(arrayOfLeadRecords);

            SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Beispielcode - Ruby

```ruby
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "http://www.marketo.com/mktows/"

#Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

#Create SOAP Header
headers = {
    'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
    "requestTimestamp"  => requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record_list => {
           :lead_record => {
               :email => "em1000@etestd.marketo.net",
            :lead_attribute_list => {
                :attribute => {
                    :attr_name => "Company",
                    :attr_value => "Marketo1000" },
                :attribute! => {
                    :attr_name => "Phone",
                    :attr_value => "650-555-1000" }
            }
        },
        :lead_record! => {
               :email => "em1001@etestd.marketo.net",
            :lead_attribute_list => {
                :attribute => {
                    :attr_name => "Company",
                    :attr_value => "Marketo1001" },
                :attribute! => {
                    :attr_name => "Phone",
                    :attr_value => "650-555-1001" }
            }
        }
    },
    :dedup_enabled => "True"
}

response = client.call(:sync_multiple_leads, message: request)

puts response
```
