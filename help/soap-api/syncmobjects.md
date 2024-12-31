---
title: syncMObjects
feature: SOAP
description: syncMObjects-SOAP-Aufrufe
exl-id: 68bb69ce-aa8c-40b7-8938-247f4fe97b5d
source-git-commit: 04e6b38a7ee602c38a851f9b99101186e72a8518
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 8%

---

# syncMObjects

Akzeptiert ein Array von [MObjects](marketo-objects.md), die erstellt oder aktualisiert werden sollen, bis zu 100 pro Aufruf, und gibt das Ergebnis (den Status) des Vorgangs (CREATED, UPDATED, FAILED, UNCHANGED, SKIPPED) und die Marketo-IDs der MObject(s) zurück. Die API kann in einem von drei Betriebsmodi aufgerufen werden:

1. EINFÜGEN - Nur neue Objekte einfügen, vorhandene Objekte überspringen
1. UPDATE - Nur vorhandene Objekte aktualisieren, neue Objekte überspringen.
   - Für das Programm-MObject kann die API nur im Aktualisierungsmodus aufgerufen werden, um neue Periodenkosteninformationen hinzuzufügen oder Tags/Kanäle für bestehende Programme hinzuzufügen/zu aktualisieren. (Tags/Kanäle müssen bereits definiert sein: Es können keine neuen Tags/Kanäle über die API erstellt werden)
1. UPSERT - Neue Objekte einfügen und vorhandene Objekte aktualisieren

Die Vorgänge UPDATE und UPSERT verwenden die ID als Schlüssel. In einem einzelnen API-Aufruf können einige Aktualisierungen erfolgreich sein und einige können fehlschlagen. Für jeden Fehler wird eine Fehlermeldung zurückgegeben

## Anfrage

| Feldname | Erforderlich/Optional | Beschreibung |
| --- | --- | --- |
| mObjectList->mObject->type | Erforderlich | Kann eines der folgenden sein: `Program`, `Opportunity`, `OpportunityPersonRole` |
| mObjectList->mObject->id | Erforderlich | ID des MObject. Sie können bis zu 100 MObjects pro Aufruf angeben. |
| mObjectList->mObject->typeAttributeList->typeAttribute->attrType | Erforderlich | Kosten (werden nur bei der Aktualisierung des Programm-MObject verwendet) Kann einer der folgenden sein: `Cost`, `Tag` |
| mObjectList->mObject->typeAttributeList->typeAttribute->attrList->attribute->name | Erforderlich | Für Program MObject können die folgenden Attribute als Name-Wert-Paare übergeben werden. Für Kosten: `Month (Required)`, `Amount (Required)`, `Id (Cost Id - Optional)`, `Note (Optional)`. Für Tag/Kanal: `Type (Required)`, `Value (Required)`. Bei Opportunity MObject können alle Felder aus der Ausgabe des [describeMObject](describemobject.md) als Name-Wert-Paare übergeben werden. In der folgenden Liste sind alle optionalen Felder und der Standardsatz von Attributen aufgeführt. Möglicherweise verfügen Sie über zusätzliche Felder im Opportunity-MO-Objekt, die über eine Support-Anfrage erstellt wurden. |

1. Betrag
1. CloseDate
1. Beschreibung
1. ExpectedRevenue
1. ExternalCreatedDate
1. Fiskalisch
1. Steuerquartal
1. Steuerjahr
1. ForecastCategoryName
1. IsClosed
1. IsWon
1. LastActivityDate
1. LeadSource
1. Name
1. NextStep
1. Wahrscheinlichkeit
1. Menge
1. Phase
1. Typ

Für OpportunityPersonRole MObject können Sie alle Felder aus der Ausgabe von [describeMObject](./describemobject.md) als Name-Wert-Paare bereitstellen. Der Standardsatz von Attributen für das OpportunityPersonRole-Objekt ist hier aufgeführt:

1. OpportunityId (erforderlich)
1. Personen-ID (erforderlich)
   1. Entspricht der Personen-/Kontakt-ID in Marketo.
1. Rolle (optional)
1. ExternalCreatedDate (optional)
1. IsPrimary (optional)
1. Rolle (optional)

| Feldname | Erforderlich/Optional | Beschreibung |
| --- | --- | --- |
| mObjAssociationList->mObjAssociation->mObjType | Optional | Wird verwendet, um Opportunity/OpportunityPersonRole MObjects mithilfe der ID oder des externen Schlüssels eines zugeordneten Objekts zu aktualisieren. Zugeordnete Objekte können sein: Unternehmen (zum Aktualisieren des Opportunity-MObjects), Lead (zum Aktualisieren des OpportunityPersonRole-MObjects), Opportunity (zum Aktualisieren des OpportunityPersonRole-MObjects) |
| mObjAssociationList->mObjAssociation->id | Optional | Die ID des zugehörigen Objekts (Lead/Unternehmen/Opportunity) |
| mObjAssociationList->mObjAssociation->externalKey | Optional | Ein benutzerdefiniertes Attribut des zugehörigen Objekts |

## Anfrage-XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>40e6d89bd2f7f0daaddaf150ce7a7ca49788ffe7</requestSignature>
      <requestTimestamp>2013-08-05T14:22:45-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMObjects>
      <mObjectList>
        <mObject>
          <type>Program</type>
          <id>1970</id>
          <typeAttribList>
            <typeAttrib>
              <attrType>Cost</attrType>
              <attrList>
                <attrib>
                  <name>Month</name>
                  <value>2013-06</value>
                </attrib>
                <attrib>
                  <name>Amount</name>
                  <value>2000</value>
                </attrib>
                <attrib>
                  <name>Id</name>
                  <value>153</value>
                </attrib>
              </attrList>
            </typeAttrib>
          </typeAttribList>
        </mObject>
      </mObjectList>
      <operation>UPDATE</operation>
    </ns1:paramsSyncMObjects>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Antwort-XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMObjects>
      <result>
        <mObjStatusList>
          <mObjStatus>
            <id>1970</id>
            <status>UPDATED</status>
          </mObjStatus>
        </mObjStatusList>
      </result>
    </ns1:successSyncMObjects>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Beispielcode - PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = ""; // CHANGE ME
$marketoSecretKey   = ""; // CHANGE ME
$marketoNameSpace   = "http://www.marketo.com/mktows/";
 
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
////////////////////////////////
$params = prepareUpdateProgramRequest();
// -or- 
//$params = prepareCreateOpptyRequest();
// -or-
//$params= prepareCreateOpptyPersonRoleRequest();
////////////////////////////////
            
$soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
try {
  $leads = $soapClient->__soapCall('syncMObjects', array($params), $options, $authHdr);
}
catch(Exception $ex) {
  var_dump($ex);
}

if ($debug) {
  print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
  print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
}

function prepareUpdateProgramRequest() {
  $params = new stdClass();

  $mObj = new stdClass();
  $mObj->type = 'Program';
  $mObj->id="1970";

  $attrib1 = new stdClass();
  $attrib1->name="Month";
  $attrib1->value="2013-06";
        
  $attrib2 = new stdClass();
  $attrib2->name="Amount";
  $attrib2->value="2000";
        
  $attrib3 = new stdClass();
  $attrib3->name="Id";
  $attrib3->value="153";
  $attribList = array ($attrib1, $attrib2, $attrib3);
        
  $costAttrib = new stdClass();
  $costAttrib->attrType="Cost";
  $costAttrib->attrList = $attribList;

  $mObj->typeAttribList= array($costAttrib);
  $params->mObjectList = array($mObj);

  $params->operation="UPDATE";

  return $params;
}

function prepareCreateOpptyRequest() {
  $params = new stdClass();

  $mObj = new stdClass();
  $mObj->type = 'Opportunity';

  $attrib1 = new stdClass();
  $attrib1->name="Name";
  $attrib1->value="Q1 2014";
        
  $attrib2 = new stdClass();
  $attrib2->name="Amount";
  $attrib2->value="2000";
        
  $attrib3 = new stdClass();
  $attrib3->name="Probability";
  $attrib3->value="80%";
  $attribs = array ($attrib1, $attrib2, $attrib3);

  $attribList = new stdClass();
  $attribList->attrib = $attribs;

  $mObj->attribList = $attribList;
  $params->mObjectList = array($mObj);

  $params->operation="INSERT";
  
  return $params;
}

function prepareCreateOpptyPersonRoleRequest() {
  $params = new stdClass();

  $mObj = new stdClass();
  $mObj->type = 'OpportunityPersonRole';

  $attrib1 = new stdClass();
  $attrib1->name="OpportunityId";
  $attrib1->value="64";
        
  $attrib2 = new stdClass();
  $attrib2->name="PersonId";
  $attrib2->value="19";
        
  $attrib3 = new stdClass();
  $attrib3->name="Role";
  $attrib3->value="Influencer/Champion";
  $attribs = array ($attrib1, $attrib2, $attrib3);

  $attribList = new stdClass();
  $attribList->attrib = $attribs;

  $mObj->attribList = $attribList;
  $params->mObjectList = array($mObj);

  $params->operation="INSERT";
  return $params;
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
import javax.xml.bind.Marshaller;
 
 
public class SyncMObjects {
    public static void main(String[] args) {
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";
             
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
            ////////////////////////////////
            ParamsSyncMObjects request = prepareUpdateProgramRequest();
            // -or- 
            //ParamsSyncMObjects request = prepareCreateOpptyRequest();
            // -or-
            //ParamsSyncMObjects request = prepareCreateOpptyPersonRoleRequest();
            ////////////////////////////////
             
            SuccessSyncMObjects result = port.syncMObjects(request, header);
            
            JAXBContext context = JAXBContext.newInstance(SuccessSyncMObjects.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);
            
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private static ParamsSyncMObjects prepareUpdateProgramRequest() {
        ParamsSyncMObjects request = new ParamsSyncMObjects();
        request.setOperation(SyncOperationEnum.UPDATE);

        MObject mobj = new MObject();
        mobj.setType("Program");
        mobj.setId(1970);
        
        TypeAttrib typeAttrib = new TypeAttrib();
        typeAttrib.setAttrType("Cost");
        
        Attrib attrib = new Attrib();
        attrib.setName("Month");
        attrib.setValue("2013-06");
        
        Attrib attrib2 = new Attrib();
        attrib1.setName("Amount");
        attrib1.setValue("2000");
        
        Attrib attrib3 = new Attrib();
        attrib1.setName("Id");
        attrib1.setValue("153");
        
        ArrayOfAttrib attribList = new ArrayOfAttrib();
        attribList.getAttribs().add(attrib);
        attribList.getAttribs().add(attrib2);
        attribList.getAttribs().add(attrib3);
        typeAttrib.setAttrList(attribList);
        
        ArrayOfTypeAttrib typeAttribList = new ArrayOfTypeAttrib();
        typeAttribList.getTypeAttribs().add(typeAttrib);
        mobj.setTypeAttribList(typeAttribList);
        
        ArrayOfMObject objList = new ArrayOfMObject();
        objList.getMObjects().add(mobj);
        request.setMObjectList(objList);
        
        return request;
    }

    private static ParamsSyncMObjects prepareCreateOpptyRequest() {
        ParamsSyncMObjects request = new ParamsSyncMObjects();
        request.setOperation(SyncOperationEnum.INSERT);
        
        MObject mobj = new MObject();
        mobj.setType("Opportunity");
        
        Attrib attrib = new Attrib();
        attrib.setName("Name");
        attrib.setValue("Q1 2014");

        Attrib attrib2 = new Attrib();        
        attrib1.setName("Amount");
        attrib1.setValue("2000");
        
        Attrib attrib3 = new Attrib();
        attrib1.setName("Probability");
        attrib1.setValue("80%");
        
        ArrayOfAttrib attribList = new ArrayOfAttrib();
        attribList.getAttribs().add(attrib);
        attribList.getAttribs().add(attrib2);
        attribList.getAttribs().add(attrib3);
        mobj.setAttribList(attribList);
        
        ArrayOfMObject objList = new ArrayOfMObject();
        objList.getMObjects().add(mobj);
        request.setMObjectList(objList);
        
        return request;
    }
    private static ParamsSyncMObjects prepareCreateOpptyPersonRoleRequest() {
        ParamsSyncMObjects request = new ParamsSyncMObjects();
        request.setOperation(SyncOperationEnum.INSERT);
        
        MObject mobj = new MObject();
        mobj.setType("OpportunityPersonRole");
        
        Attrib attrib = new Attrib();
        attrib.setName("OpportunityId");
        attrib.setValue("64"); // Id of the opportunity created earlier
        
        Attrib attrib2 = new Attrib();
        attrib1.setName("PersonId");
        attrib1.setValue("19");
        
        Attrib attrib3 = new Attrib();
        attrib1.setName("Role");
        attrib1.setValue("Influencer/Champion");
        
        ArrayOfAttrib attribList = new ArrayOfAttrib();
        attribList.getAttribs().add(attrib);
        attribList.getAttribs().add(attrib2);
        attribList.getAttribs().add(attrib3);
        mobj.setAttribList(attribList);
        
        ArrayOfMObject objList = new ArrayOfMObject();
        objList.getMObjects().add(mobj);
        request.setMObjectList(objList);
        
        return request;
    }
            
}
```

## Beispielcode - Ruby

```ruby
require 'savon' # Use version 1.0 Savon gem
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
    :m_object_list => {
          :m_object => {
              :type => "Program",
              :id => "1970",
              :type_attrib_list => {
                  :type_attrib => {
                      :attr_type => "Cost",
                      :attr_list => {
                          :attrib => {
                              :name => "Month",
                              :value => "2013-06" },
                        :attrib! => {
                              :name => "Amount",
                              :value =>  "2000" },
                        :attrib! => {
                              :name => "Id",
                              :value => "153" }
                    }
                }
            }
        }
    },
      :operation => "UPDATE"
}

response = client.call(:sync_m_objects, message: request)

puts response
```
