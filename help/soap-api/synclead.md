---
title: Lead synchronisieren
feature: SOAP
description: SyncLead-SOAP-Aufrufe
exl-id: e6cda794-a9d4-4153-a5f3-52e97a506807
source-git-commit: ebe8faf41dff0e0ba5f4323f5909cc3c9813fd10
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 2%

---

# Lead synchronisieren

Diese Funktion fügt einen einzelnen Lead-Datensatz ein oder aktualisiert ihn. Beim Aktualisieren eines bestehenden Leads wird der Lead mit einem der folgenden Schlüssel identifiziert:

- Marketo-ID
- Ausländische System-ID (als `foreignSysPersonId` implementiert)
- Marketo-Cookie (vom Munchkin JS-Skript erstellt)
- E-Mail

Wenn eine vorhandene Übereinstimmung gefunden wird, führt der Aufruf eine Aktualisierung durch. Andernfalls wird ein Lead eingefügt und erstellt. Anonyme Leads können mit der Marketo-Cookie-ID aktualisiert werden und werden bei der Aktualisierung bekannt.

Mit Ausnahme von E-Mail werden alle diese Kennungen als eindeutige Schlüssel behandelt. Die Marketo-ID hat Vorrang vor allen anderen Schlüsseln. Wenn sowohl `foreignSysPersonId` als auch die Marketo-ID im Lead-Datensatz vorhanden sind, hat die Marketo-ID Vorrang und die `foreignSysPersonId` wird für diesen Lead aktualisiert. Wenn der einzige `foreignSysPersonId` angegeben ist, wird er als eindeutiger Bezeichner verwendet. Wenn sowohl `foreignSysPersonId` als auch E-Mail vorhanden sind, aber die Marketo-ID nicht vorhanden ist, hat die `foreignSysPersonId` Priorität und die E-Mail wird für diesen Lead aktualisiert.

Optional kann eine Kontextkopfzeile angegeben werden, um den Zielarbeitsbereich zu benennen.

Wenn Marketo-Arbeitsbereiche aktiviert sind und die Kopfzeile verwendet wird, werden die folgenden Regeln angewendet:

- Wenn Zuweisungsregeln festgelegt sind und sich ein neuer Lead für eine der konfigurierten Regeln qualifiziert, werden neue Leads in der durch die Zuweisungsregel definierten Partition erstellt. Andernfalls werden neue Leads in der primären Partition des benannten Arbeitsbereichs erstellt.
- Leads, die mit einer Marketo-Lead-ID, einer fremden System-ID oder einem Marketo-Cookie abgeglichen wurden, müssen in der Primärpartition des benannten Arbeitsbereichs vorhanden sein. Andernfalls wird ein Fehler zurückgegeben
- Wenn ein vorhandener Lead per E-Mail zugeordnet wird, wird der benannte Arbeitsbereich ignoriert und der Lead in der aktuellen Partition aktualisiert

Wenn Marketo-Arbeitsbereiche aktiviert sind und die Kopfzeile NICHT verwendet wird, werden die folgenden Regeln angewendet:

- Wenn Zuweisungsregeln festgelegt sind und sich ein neuer Lead für eine der konfigurierten Regeln qualifiziert, werden neue Leads in der durch die Zuweisungsregel definierten Partition erstellt. Andernfalls werden neue Leads in der primären Partition des Arbeitsbereichs „Standard“ erstellt.
- Vorhandene Leads werden in ihrer aktuellen Partition aktualisiert

Wenn Marketo-Arbeitsbereiche NICHT aktiviert sind, MUSS der Zielarbeitsbereich der „Standard“-Arbeitsbereich sein. Es ist nicht erforderlich, den -Header zu übergeben.

## Anfrage

| Feldname | Erforderlich/Optional | Beschreibung |
| --- | --- | --- |
| leadRecord->ID | Erforderlich - Nur wenn keine E-Mail oder `foreignSysPersonId` vorhanden ist | Die Marketo-ID des Lead-Datensatzes |
| leadRecord->Email | Erforderlich - Nur wenn keine ID oder `foreignSysPersonId` vorhanden ist | Die mit dem Lead-Datensatz verknüpfte E-Mail-Adresse |
| leadRecord->`foreignSysPersonId` | Erforderlich - Nur wenn keine ID oder E-Mail vorhanden ist | Die dem Lead-Datensatz zugeordnete ausländische System-ID |
| leadRecord->ForeignSysType | Optional - Nur erforderlich, wenn `foreignSysPersonId` vorhanden ist | Die Art des Fremdsystems. Mögliche Werte: CUSTOM, SFDC, NETSUITE |
| leadRecord->leadAttributeList->attribute->attrName | Erforderlich | Der Name des Lead-Attributs, dessen Wert Sie aktualisieren möchten. |
| leadRecord->leadAttributeList->attribute->attrValue | Erforderlich | Der Wert, den Sie auf das in attrName angegebene Attribut für den Lead festlegen möchten. |
| returnLead | Erforderlich | Wenn „true“, wird der vollständige aktualisierte Lead-Datensatz bei der Aktualisierung zurückgegeben. |
| marketoCookie | Optional | Das [Munchkin-JavaScript](../javascript-api/lead-tracking.md)-Cookie |

## Anfrage-XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncLead>
      <leadRecord>
        <Email>t@t.com</Email>
        <leadAttributeList>
          <attribute>
            <attrName>FirstName</attrName>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Antwort-XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1089965</leadId>
        <syncStatus>
          <leadId>1089965</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Beispielcode - PHP

```php
 <?php
 
  $debug = true;
 
  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";
 
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
  $options = array("connection_timeout" =20, "location" =$marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }
 
  // Create Request
  $leadKey = new stdClass();
  $leadKey->Email = "george@jungle.com";
 
  // Lead attributes to update
  $attr1 = new stdClass();
  $attr1->attrName  = "FirstName";
  $attr1->attrValue = "George";
 
  $attr2= new stdClass();
  $attr2->attrName  = "LastName";
  $attr2->attrValue = "of the Jungle";
 
  $attrArray = array($attr1, $attr2);
  $attrList = new stdClass();
  $attrList->attribute = $attrArray;
  $leadKey->leadAttributeList = $attrList;
 
  $leadRecord = new stdClass();
  $leadRecord->leadRecord = $leadKey;
  $leadRecord->returnLead = false;
  $params = array("paramsSyncLead" =$leadRecord);
 
  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $result = $soapClient->__soapCall('syncLead', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }
 
  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }
  print_r($result);
 
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
 
 
public class SyncLead {
 
    public static void main(String[] args) {
        System.out.println("Executing syncLead");
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
            ParamsSyncLead request = new ParamsSyncLead();
            LeadRecord key = new LeadRecord();
             
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Stringemail = objectFactory.createLeadRecordEmail("george@jungle.com");
            key.setEmail(email);
            request.setLeadRecord(key);
             
            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George2");
             
            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");
             
            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);
             
            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttributeattrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);         
            key.setLeadAttributeList(attrList);
             
            MktowsContextHeader headerContext = new MktowsContextHeader();
            headerContext.setTargetWorkspace("default");
             
            SuccessSyncLead result = port.syncLead(request, header, headerContext);
 
            JAXBContext context = JAXBContext.newInstance(SuccessSyncLead.class);
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
    'ns1:AuthenticationHeader' ={ "mktowsUserId" =mktowsUserId, "requestSignature" =requestSignature, 
    "requestTimestamp"  =requestTimestamp 
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record ={
        :Email ="t@t.com",
          :lead_attribute_list ={
              :attribute ={
                :attr_name ="FirstName",
                :attr_value ="George" },
              :attribute! ={
                :attr_name ="LastName",
                :attr_value ="of the Jungle" }
        }
    },
    :return_lead ="false"
}

response = client.call(:sync_lead, message: request)

puts response
```
