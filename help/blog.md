---
title: Blog-Archiv
description: Marketo Developer Blog Archive 2014-2023 mit historischen Beiträgen zu Forms 2.0, Zapier, API-Updates, Einstellung von SOAP und Migration zu REST.
exl-id: d7ae88dd-9938-4957-9798-db43090dab4e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '61723'
ht-degree: 0%

---

# Blog-Archiv

>[!INFO]
>
>Dies ist ein Archiv des Marketo-Blogs von 2014 bis 2023. Sie ist hier nur als historische Referenz angegeben.
>&#x200B;>Einige Informationen sind möglicherweise veraltet.  In der aktuellen Dokumentation finden Sie immer die neuesten Funktionen.
>

>[!IMPORTANT]
>Die SOAP-API wird nicht mehr unterstützt und ist nach dem 31. Januar 2026 nicht mehr verfügbar. Alle neuen Entwicklungen sollten mit der Marketo REST-API durchgeführt werden, und die vorhandenen Services sollten bis zu diesem Datum migriert werden, um Unterbrechungen des Services zu vermeiden. Wenn Sie über einen Dienst verfügen, der die SOAP-API verwendet, finden Sie im [SOAP-API-Migrationshandbuch](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/migration) Informationen zur Migration.
>

>[!IMPORTANT]
>Die Unterstützung für die Authentifizierung mit dem `access_token` Abfrageparameter wird am 31. Januar 2026 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um den [Autorisierungs](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/authentication#using-an-access-token)Header zu verwenden. Für neue Entwicklungen sollte ausschließlich der Autorisierungs-Header verwendet werden.
>

## Willkommen beim Marketo Developer Blog

Willkommen bei Marketo Developers! Wir waren hier bei Marketo recht beschäftigt und haben neue Funktionen veröffentlicht, mit denen Marketing-Experten erfolgreicher sein können als je zuvor. Marketer werden heutzutage von einer phänomenalen Entwickler-Community unterstützt. Dieser Blog ist für die Web-Entwickler und Software-Ingenieure gewidmet, die die sich schnell entwickelnden Bedürfnisse des modernen Marketing-Experten unterstützen. Im Zuge der Weiterentwicklung der Marketo-Plattform kündigen wir neue Integrationsoptionen und API-Versionsaktualisierungen an, sobald diese freigesetzt werden. Außerdem werden wir eine neue Reihe von Anleitungsartikeln einführen, in denen wir Codebeispiele und Best Practices zur Integration mit der Marketo-Plattform austauschen. Im ersten Artikel dieser Reihe erfahren Sie, wie Sie mithilfe der -API effizient Informationen zu den Personen (Kunden/Kontakte/Leads) abrufen können, die in Marketo gespeichert sind. Bitte melden Sie sich über das obige Formular an, um auf dem Laufenden zu bleiben. Aktualisierungen werden per E-Mail gesendet, sobald neue Artikel veröffentlicht werden.

Veröffentlicht am _2014-03-06_ von _David_

## Updates vom Januar 2014

### Formulare 2.0

Forms 2.0 ermöglicht es Marketing-Experten, ansprechende, stabile und flexible Web-Formulare zu erstellen, ohne Programmierkenntnisse zu benötigen. Neben dem stark verbesserten Editor, der Bedingungslogik und dem robusten Design ist es jetzt einfacher denn je, Marketo-Formulare auf jeder Seite Ihrer Website einzubetten. Entwicklerinnen und Entwickler können JavaScript verwenden, um die Kernfunktionalität zu erweitern. Anwendungsfälle sind:

* Ausblenden eines Formulars nach erfolgreicher Übermittlung, damit der Besucher auf der Seite bleibt, nachdem er das Formular ausgefüllt hat
* Anzeigen einer benutzerdefinierten Fehlermeldung beim Senden basierend auf benutzerdefinierter Geschäftslogik
* Anzeigen des Formulars in einem Dialogfeld im Lightbox-Stil

Die Entwicklerdokumentation finden Sie [hier](/help/javascript-api/forms-api-reference.md).

### SOAP API-Version 2_3 jetzt verfügbar

* [getLeadChanges:](/help/soap-api/getleadchanges.md) Einführung in das Anfragefeld `activityNameFilter`
* [ListOperation:](/help/soap-api/listoperation.md) Feld „Entfernte Anfrage“ `skipActivityLog`

**Hinweis** SOAP API-Revisionen sind abwärtskompatibel

Veröffentlicht am _2014-01-26_ von _Travis Kaufman_

## Zapier Part II: Marketo-Integrationsankündigung

In einem früheren Beitrag haben wir besprochen, wie Sie Zapier verwenden können, um externe Datenquellen mit Marketo zu integrieren. Der Beitrag präsentierte einen praxisnahen Ansatz zum Erstellen eines eigenen Zapier-Workflows (oder „Zap„) von Grund auf, um Marketo und andere Apps zu integrieren. Ihnen gefällt die Idee, Zapier mit Marketo zu verwenden, aber Sie benötigen Hilfe beim Einstieg? Gute Nachrichten! Zapier hat gerade eine Reihe von Beispiel-Zaps für Marketo veröffentlicht, die Ihnen einen schnellen Einstieg ermöglichen:

* Neue Leads erfassen
* Kundschaft fördern
* Kontaktinformationen verteilen
* Auf neue potenzielle Kunden reagieren

Zusätzlich zu diesen Beispielen können Sie die Seite [Marketo-Integrationen](https://zapier.com/apps/marketo/integrations) mit Hunderten anderer Apps auf Zapier durchsuchen und in wenigen Minuten Ihre eigenen automatisierten Workflows erstellen. Es ist keine Codierung erforderlich. Sparen Sie Zeit und lassen Sie die Automatisierung Ihre manuelle Arbeit erledigen. Werde kreativ! Der Himmel ist die Grenze!

Veröffentlicht am _2016-06-01_ von _David_

## Abrufen von Kunden- und Interessenteninformationen aus Marketo mithilfe der -API

Sie können Informationen zu Kunden und potenziellen Kunden abrufen, die in Marketo gespeichert sind, indem Sie die `getLead` und [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) SOAP-API verwenden. Oft ist es erwünscht, diese Informationen regelmäßig zu extrahieren, um ein anderes System auf dem neuesten Stand zu halten, wenn Kunden und Interessenten in Marketo aktualisiert oder neue Datensätze erstellt werden. Wir zeigen Ihnen das Code-Beispiel, das wiederkehrend ausgeführt wird, um Marketo nach Updates zu fragen. Das folgende Diagramm zeigt die API-Aufrufe, die an einem festgelegten periodischen Zeitgeber durchgeführt werden. Je nach Anwendungsfall kann der periodische Timer so eingestellt werden, dass der folgende Code alle 10 Minuten ausgeführt wird.

Beim ersten Aufruf von [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) werden der Zeitbereich, die Batch-Größe und die zurückzugebenden Felder festgelegt. Alle Leads innerhalb von Marketo, die im angegebenen Zeitraum aktualisiert wurden, werden zusammen mit einer streamPosition zurückgegeben, wenn mehr Datensätze verfügbar sind als die angegebene batchSize.

**SOAP-Anfrage für ersten Aufruf an getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector>
  <batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

**SOAP-Antwort vom ersten Aufruf an getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
 <returnCount>2</returnCount><remainingCount>24</remainingCount><newStreamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</newStreamPosition><leadRecordList>
      <leadRecord>
        <Id>84105</Id>
        <Email>eimang@marketo.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>French</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>Lead</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <leadRecord>
        <Id>1089965</Id>
        <Email>t@t.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
    </leadRecordList>
  </result>
</ns2:successGetMultipleLeads>
```

Solange der `<remainingCount/>` größer als 0 ist, führen Sie nachfolgende Aufrufe an `getMultipleLeads` durch, um den Rest zu paginieren, indem Sie den im vorherigen Aufruf zurückgegebenen `<newStreamPosition/>`-Wert an den `<streamPosition/>` übergeben. **SOAP-Anfrage für nachfolgenden Aufruf an getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector><streamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</streamPosition><batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

Diese Logik gilt so lange, wie `<remainingCount/>` größer als null ist. **Unten finden Sie ein Java-Beispielprogramm, das das oben beschriebene Szenario ausführt.**

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.GregorianCalendar;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import javax.xml.datatype.DatatypeFactory;
import javax.xml.datatype.XMLGregorianCalendar;

public class GetMultipleLeads {
  public static void main(String[] args) {
    System.out.println("Executing GetMultipleLeads");
        try {
        URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
        String marketoUserId = "CHANGE ME";
        String marketoSecretKey = "CHANGE ME";
        QName serviceName = new QName("<http://www.marketo.com/mktows/>",
        "MktMktowsApiService");
        MktMktowsApiService service = new
        MktMktowsApiService(marketoSoapEndPoint, serviceName);
        MktowsPort port = service.getMktowsApiSoapPort();

        // Create Signature
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String text = df.format(new Date());
        String requestTimestamp = text.substring(0, 22) + ":" +         text.substring(22);
        String encryptString = requestTimestamp + marketoUserId ;

        SecretKeySpec secretKey = new         SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");

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
        ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();

        // Build Request Using LastUpdateAtSelector
        LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
        GregorianCalendar gc = new GregorianCalendar();
        gc.setTimeInMillis(new Date().getTime());
        gc.add( GregorianCalendar.DAY_OF_YEAR, -20);
        DatatypeFactory factory = DatatypeFactory.newInstance();
        ObjectFactory objectFactory = new ObjectFactory();
        JAXBElement<XMLGregorianCalendar> until =         objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

        GregorianCalendar since = new GregorianCalendar();
        since.setTimeInMillis(new Date().getTime());
        since.add( GregorianCalendar.DAY_OF_YEAR, -21);
        leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
        leadSelector.setLatestUpdatedAt(until);
        request.setLeadSelector(leadSelector);

        ArrayOfString attributes = new ArrayOfString();
        attributes.getStringItems().add("FirstName");
        attributes.getStringItems().add("LastName");
        attributes.getStringItems().add("Email");
        request.setIncludeAttributes(attributes);

        JAXBElement<Integer> batchSize = new
        ObjectFactory().createParamsGetMultipleLeadsBatchSize(2);
        request.setBatchSize(batchSize);

        SuccessGetMultipleLeads result = null;

        int count = 0;

        do {
        if (count > 0) {
        // Set the streamPosition on subsequent calls to paginate
        through large result sets
        String pos = result.getResult().getNewStreamPosition();
        JAXBElement<String> streamPos = new
        ObjectFactory().createParamsGetMultipleLeadsStreamPosition(pos);
        request.setStreamPosition(streamPos);
        }

        JAXBContext context =
        JAXBContext.newInstance(ParamsGetMultipleLeads.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        System.out.println("REQUEST:");
        m.marshal(request, System.out);
        result = port.getMultipleLeads(request, header);
        System.out.println("RESPONSE:");
        m.marshal(result, System.out);
        count = result.getResult().getReturnCount();
        } while (result.getResult().getRemainingCount() > 0);
        }

        catch(Exception e) {
        // Update to include appropriate retry/error handling
        e.printStackTrace();
        }

  }

}
```

### Tipps und Tricks

Wenn Sie große Mengen an Kontakten aus Marketo extrahieren, wird empfohlen, die API-Anfrage anhand der folgenden Parameter anzupassen:

* `<includeAttributes/>`: Es wird empfohlen, nur die Felder anzufordern, die mit Ihrem System synchronisiert werden sollen. Dies reduziert die Payload der Antwort und erhöht die Abfrageleistung.
* `<batchSize/>`: Die API unterstützt die Rückgabe von bis zu 1.000 Datensätzen in einem einzigen Aufruf. Wird dieser Wert auf 500 Datensätze reduziert, wird auch die Payload der Antwort reduziert.
* `<LastUpdatedAtSelector/>`: Es wird empfohlen, sowohl die `<oldestUpdatedAt/>` als auch den Parameter `<latestUpdatedAt/>` festzulegen, um den Datumsbereich zu begrenzen. Zum Beispiel, anstatt eine einzige Anfrage für die Daten eines Jahres zu stellen. Teilen Sie die API-Aufrufe auf, um kleinere Datumsbereiche anzufordern.

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-03-05_ von _Travis Kaufman_

## Versionsaktualisierungen Februar 2014

### SOAP API-Aktualisierung

* [syncMObjects](/help/soap-api/syncmobjects.md): Sie können jetzt Tags und Kanäle für vorhandene Programme hinzufügen und aktualisieren.

Aktualisierungen werden in die [2_3 WSDL ](http://app.marketo.com/soap/mktows/2_3?WSDL).

Veröffentlicht am _2014-02-26_ von _Travis Kaufman_

## Updates vom März 2014

### SOAP API-Aktualisierung

* Leistungsverbesserungen an [syncLead](/help/soap-api/synclead.md) und [syncMultipleLeads](/help/soap-api/syncmultipleleads.md)

Aktualisierungen werden in die [2_3 WSDL ](http://app.marketo.com/soap/mktows/2_3?WSDL).

Veröffentlicht am _2014-03-20_ von _Travis Kaufman_

## Aktivität „Anonymer Besucher“ beim Ausfüllen des Formulars durch den Besucher zusammenführen

In dem Blogpost mit dem Titel „Anonyme Besucheraktivität basierend auf Geschäftslogik erfassen“ haben wir besprochen, wie anonyme Lead-Datensätze in Marketo auf der Grundlage benutzerdefinierter Ereignisse erstellt werden. In diesem Blogpost werden wir auf diesem Beitrag aufbauen und einen anonymen Lead-Eintrag mit einem bekannten Benutzer verknüpfen, nachdem Sie die Kontaktinformationen des Benutzers erhalten haben. Mit dem [Munchkin-Trackingcode](/help/javascript-api/lead-tracking.md) von Marketo können Sie Besuche auf Ihrer Website verfolgen. Wenn jemand zum ersten Mal eine Seite Ihrer Website besucht, die den Munchkin-Trackingcode enthält, erstellt Marketo einen anonymen Lead und verwendet ein Browser-Cookie, um diesen zu verfolgen. Sobald sie sich identifizieren, werden sie zu einem bekannten Lead und der mit ihrem Browser-Cookie verknüpfte Verlauf wird in ihrem Marketo-Lead-Datensatz zusammengeführt. **Anonyme Leads werden erstellt, wenn jemand:**

1. Besucht Ihre Marketo-Landingpage zum ersten Mal
1. Besuche einer Seite mit Munchkin-Trackingcode
1. Klicken Sie in einer Marketo-E-Mail auf den Link Als Webseite anzeigen .

**Ein anonymer Lead wird zu einem neuen oder vorhandenen bekannten Lead zusammengeführt, wenn jemand:**

1. Auf einen Link zu einer Marketo-E-Mail klicken
1. Ausfüllen eines Marketo-Formulars
1. Mit einer der folgenden Methoden kann ein anonymer Lead mit einem bekannten Datensatz verknüpft werden.

Verwenden Sie eine der folgenden Methoden, um Personendaten zu übermitteln oder ein Browser-Webtracking-Cookie mit dem entsprechenden Personendatensatz in Marketo zu verknüpfen: **Browser-seitige Übermittlung** Wenn Ihr Anwendungsfall die Übermittlung von Personendaten vom Browser erfordert, sollten Sie die Übermittlung von Hintergrundformularen verwenden. **Server-seitige Übermittlung** Wenn Sie keine Browser-seitige Übermittlung benötigen, bietet die REST-API viele Methoden für die Übermittlung von Personendaten und eine speziell entwickelte Methode für die Zuordnung von Cookies zu Personendatensätzen.

Veröffentlicht am _2014-04-22_ von _Murta_

## Versionsaktualisierungen April 2014

### Marketo Forms-Sicherheitsupdate

Wir haben ein Limit für die Anzahl und Häufigkeit der Formularübermittlungen von einer einzelnen IP-Adresse aus eingeführt. Diese Beschränkung wird jetzt auf 30 Beiträge pro Minute durchgesetzt, um unsere Kunden vor böswilliger Verwendung von programmgesteuerten Formularübermittlungen zu schützen. Die [syncLead API](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/leads/synclead) ist das empfohlene Integrationsvehikel für die programmgesteuerte Übermittlung neuer Kontakte in Marketo.

Veröffentlicht am _2014-04-29_ von _Travis Kaufman_

## Ersetzen Sie die Formularnachbearbeitungsintegration durch die Marketo-API.

Es ist üblich, dass Marketing-Fachleute Marketo verwenden, um den Erfolg einer bestimmten Marketing-Kampagne anhand der Anzahl der Kontakte/Leads zu verknüpfen, die ein bestimmtes Web-Formular ausgefüllt haben. Der Marketing-Experte erstellt einfach ein neues Marketo-Formular, platziert es auf einer Landingpage und richtet dann in Marketo eine Kontaktkampagne ein, um alle Trigger, die dieses Formular ausgefüllt haben, mit diesem Marketing-Programm zu verknüpfen. (siehe Screenshot unten). Es ist naheliegend, denselben Ansatz bei der Aufzeichnung des Programmerfolgs zu verwenden, auch wenn sich der Kampagnenerfolg außerhalb des Formulars befindet. Wenn beispielsweise ein Marketer ein Werbeangebot ausführt und die Konversion darin besteht, einen Termin zu vereinbaren. Der potenzielle Kunde füllt zu keinem Zeitpunkt des Prozesses ein Formular aus. Im folgenden Artikel zeigen wir Ihnen, wie Sie mit der Marketo syncLead-API einen neuen Kontakt erstellen, wenn es sich um neue Kontakte handelt, und dann die RequestCampaign-API verwenden können, um den Erfolg für ein bestimmtes Marketing-Programm aufzuzeichnen.

Veröffentlicht am _1970-01-01_ von _Travis Kaufman_

## Löschen von Leads mit der Marketo-API

Angenommen, Sie möchten einen Lead über die Marketo-API löschen. Dazu können Sie die requestCampaign-API für eine Kampagne mit einer vordefinierten Flussaktion zum Löschen eines Leads aufrufen. Hier erfahren Sie zunächst, wie Sie eine Smart Campaign erstellen, wie Sie einen Trigger einrichten, um eine Kampagne über die API auszuführen, sowie drittens, wie Sie das Löschen eines Leads im Rahmen einer Flussaktion definieren und viertens, welche Codebeispiele zum Ausführen dieser Kampagne verwendet werden. **Erstellen einer neuen Smart-Kampagne in Marketo** Smart-Kampagnen in Marketo führen alle Ihre Marketing-Aktivitäten aus. In einer intelligenten Kampagne können Sie eine Reihe automatisierter Aktionen einrichten, um eine intelligente Kontaktliste zu erstellen. Beim Löschen eines Leads richten Sie in der Kampagne einen Trigger ein, wie unten dargestellt. Richten wir zunächst die Smart Campaign ein.

1. Wählen Sie unter „Marketing-Aktivitäten“ ein Programm aus und klicken Sie dann in der Dropdown-Liste „Neu“ auf „Neues lokales Asset 1“. Klicken Sie auf Smart Campaign
1. Geben Sie den Namen der Smart-Kampagne ein und klicken Sie auf Erstellen Trigger zu einer Smart-Kampagne hinzufügen** Durch das Hinzufügen von Triggern zu einer Smart-Kampagne kann eine Smart-Kampagne auf der Grundlage eines Live-Ereignisses einzeln ausgeführt werden. In diesem Fall handelt es sich um eine Anfrage über die requestCampaign-API.
1. Suchen Sie nach dem Trigger „Kampagne ist angefordert“ und ziehen Sie ihn dann per Drag-and-Drop auf die Arbeitsfläche.
1. Wählen Sie im Trigger „is“ und „Web Service API“ aus.

**Erstellen einer Aktion „Lead-Fluss löschen“ in einer Kampagne** Klicken Sie oben im Menü auf Fluss . Suchen Sie im Menü auf der rechten Seite nach Lead löschen und ziehen Sie ihn dann in die Mitte, um ihn als Trigger zur Kampagne hinzuzufügen. Hinweis: Wenn Sie einen Lead nur aus Marketo löschen und in Ihrem CRM belassen, wird der Lead bei der Aktualisierung seiner Daten in Marketo neu erstellt.  **Codebeispiel zum Aufrufen der requestCampaign-API** Nachdem Sie die Kampagne und die Trigger in der Marketo-Benutzeroberfläche eingerichtet haben, zeigen wir Ihnen, wie Sie die API zum Ausführen dieser Kampagne verwenden. Das erste Beispiel ist eine XML-Anfrage, das zweite eine XML-Antwort und das letzte Beispiel ist ein Java-Codebeispiel, das zum Generieren der XML-Anfrage verwendet werden kann. Außerdem erfahren Sie, wie Sie die Kampagnen-ID finden, die beim Aufruf der requestCampaign-API verwendet wird. Für den API-Aufruf müssen Sie außerdem die ID der Marketo-Kampagne im Voraus kennen. Sie können die Kampagnen-ID mit einer der folgenden Methoden ermitteln:

1. Verwenden der API [getCampaignForSource](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET)
1. Öffnen Sie die Marketo-Kampagne in einem Browser und sehen Sie sich die URL-Adressleiste an. Die Kampagnen-ID (dargestellt als 4-stellige Ganzzahl) ist unmittelbar nach „SC“ zu finden. Beispiel: `https://app-stage.marketo.com/#SC**1025**A1`. Der fett gedruckte Teil ist die Kampagnen-ID „1025“. SOAP-Anfrage für requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-Antwort für requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Unten finden Sie ein Java-Beispielprogramm, das das oben beschriebene Szenario ausführt.

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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-05-16_ von _Murta_

## Benutzerdefiniertes Aktivitäts-Tracking mit der Munchkin-API

Im Folgenden möchten Sie die benutzerdefinierte Aktivität in Marketo verfolgen. Sie haben beispielsweise ein Video auf einer Web-Seite und möchten Besucher verfolgen, die mehr als 50 % eines Videos ansehen. Dies ist mit der benutzerdefinierten Aktivitäts-Tracking-Funktion von Munchkin möglich. Dies würde implementiert, indem nach einem Ereignis auf der Seite gesucht wird, d. h. wenn das Video 50 % erreicht, und dann die Munchkin-API aufgerufen wird. Dazu müssen wir eine benutzerdefinierte Aktivität in Marketo einrichten, die basierend auf diesem Ereignis auf der Seite aufgerufen wird. Wir verwenden YouTube für den Videoplayer und dessen [YouTube Iframe-API](https://developers.google.com/youtube/iframe_api_reference) um die Methode mit der Munchkin-API aufzurufen.

Wir zeigen Ihnen zunächst, wie Sie Munchkin-Trackingcode in Marketo generieren, zweitens, wie Sie Ihren Munchkin-Beispielcode basierend auf Seitenereignissen in Trigger ändern, drittens, wie Sie eine Kampagne mit einer Smart-Liste einrichten, die durch Aktionen auf der Seite mit Flussschritten definiert wird, und fünftens, wie Sie überprüfen können, ob ein Seitenbesuch eines anonymen Benutzers in Marketo aufgezeichnet wurde. ==== Dieser Blogpost ist ein Live-Beispiel für den Code, der erklärt wird. Bitte füllen Sie dieses Formular aus, damit Sie ein bekannter Nutzer von Marketo sind. Auf diese Weise, wenn Sie 50 % des Videos ansehen, sendet es Ihnen den Rest des Videos, und wenn Sie 100 % des Videos ansehen, sendet es Ihnen einen Link zu einem anderen Blogpost. === <https://developers.google.com/youtube/iframe_api_reference>

**Generieren von Munchkin-Trackingcode** Mit dem Munchkin-Trackingcode können Sie Besuche auf Ihrer Website verfolgen. Im Folgenden werden drei Typen von Munchkin-Code beschrieben. In diesem Beispiel verwenden wir jedoch den asynchronen Munchkin-Trackingcode. A) Einfach: Verfügt über die wenigsten Codezeilen, wird aber nicht für die Ladezeit der Web-Seite optimiert. Dieser Code lädt die jQuery-Bibliothek jedes Mal, wenn eine Web-Seite geladen wird. B) Asynchron: Verringert die Ladezeit der Web-Seite. Dieser Code prüft, ob die jQuery-Bibliothek bereits vorhanden ist, lädt sie, falls sie fehlt, und verwendet sie für die Ausführung von Trackingcode, sobald der Rest der Web-Seite geladen wurde. C) Asynchrone jQuery: verringert die Ladezeit der Web-Seite und verbessert auch die Systemleistung. In diesem Code wird davon ausgegangen, dass Sie bereits über jQuery verfügen, und es wird nicht geprüft, ob es geladen wird.

1. Klicken Sie oben rechts in der App auf Admin .
1. Klicken Sie in der Baumstruktur links auf Munchkin .
1. Wählen Sie Asynchron für den Trackingcode-Typ aus.
1. Klicken Sie auf und kopieren Sie den JavaScript-Trackingcode, den Sie auf Ihrer Website einfügen möchten. **YouTube-Code** <https://developers.google.com/youtube/js_api_reference#EventHandlers> <https://developers.google.com/youtube/iframe_api_reference> Player.

`getCurrentTime()` Gibt die verstrichene Zeit in Sekunden seit Beginn der Videowiedergabe zurück. `player.getDuration()` Gibt die Dauer in Sekunden des aktuell wiedergegebenen Videos zurück. Beachten Sie, dass `getDuration()` 0 zurückgibt, bis die Metadaten des Videos geladen werden, was normalerweise unmittelbar nach dem Beginn der Videowiedergabe geschieht. Wenn ein nicht-Cookie-Benutzer auf eine Seite mit dem Munchkin-Trackingcode wechselt, wird ein neues Cookie im Browser des Benutzers erstellt und in Marketo wird ein neuer anonymer Lead erstellt. Wenn der Benutzer bereits Cookies hat und er bereits ein Lead in Marketo ist, wird der Besuch der Seite im Aktivitätsprotokoll des Benutzers in Marketo aufgezeichnet. **Code-Beispiel für Cookie-Benutzer und Tracking-**: Platzieren Sie den Tracking-Code auf Ihren Web-Seiten direkt vor dem `</body>`-Tag. In Marketo erstellte Landingpages enthalten automatisch Trackingcode, sodass Sie diesen Code nicht auf sie anwenden müssen. Dieses Codebeispiel ruft die Munchkin-API auf, nachdem das Skript geladen wurde:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Dieses Codebeispiel ruft die Munchkin-API auf, nachdem der/die Benutzende 5 Sekunden lang auf der Seite war und auch 500 Pixel nach unten gescrollt hat:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Überprüfen, ob der Seitenbesuch eines anonymen Benutzers in Marketo aufgezeichnet wurde**

1. Klicken Sie im oberen Menü auf Analytics und dann auf Neuer Bericht . Wählen Sie Webseitenaktivität als Berichtstyp aus und benennen Sie dann Ihren Bericht.
1. Nachdem Sie einen Bericht erstellt haben, klicken Sie auf Smart-Liste. Wählen Sie dann im Feld rechts den Filter Besuchte Web-Seite aus. Geben Sie die Web-Seite ein, auf der Sie den Munchkin-Trackingcode ablegen.
1. Klicken Sie auf Setup. Wählen Sie anonyme Besucher von ISPs aus und ändern Sie die Option in Angezeigt.
1. Klicken Sie auf Bericht. Die Aktivität wird nun auf der ausgewählten Web-Seite verfolgt.
1. Doppelklicken Sie auf den Lead-Eintrag, der dann das Aktivitätsprotokoll anzeigt, in dem die spezifische Seite angezeigt wird, die der anonyme Benutzer besucht hat.

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Bei Seiten mit Multimedia-Inhalten können Sie beispielsweise ein benutzerdefiniertes Tracking durchführen. Ein gängiges Beispiel ist das Hinzufügen von Munchkin-Trackingcode zur Seite sowie die Verwendung der Munchkin-API , um Ereignisse in Ihrer Marketo-Instanz für Aktivitäten wie die Wiedergabe eines Videos oder das Hören eines Audioclips zu generieren. Es wird empfohlen, Munchkin-Trackingcode auf den meisten oder allen Web-Seiten zu platzieren. Munchkin-Trackingcode wird automatisch in die Landingpages eingefügt, die Sie mit Marketo erstellen. Verwenden Sie diesen Aufruf, um aufzuzeichnen, dass der Benutzer etwas getan hat, z. B. eine Seite in Ajax, Flash oder einer anderen RIA-Umgebung besucht hat. Die URL darf weder &quot;&quot; noch eine Domain enthalten, kann aber auf eine beliebige Seite verweisen - sogar auf Seiten, die nicht vorhanden sind. Wenn Sie URL-Parameter hinzufügen möchten, verwenden Sie das Argument params .
Das Ereignis wird als Web-Seiten-Besuchsereignis im Aktivitätsprotokoll des Benutzers unter der Domain der aufrufenden Web-Seite angezeigt. Beachten Sie: Beim ersten Aufruf von `mktoMunchkin()` wird immer ein Besuchs-Web-Seiten-Ereignis für die aktuelle Seite erstellt. Sie müssen `visitWebPage` nur dann aufrufen, wenn Sie einen zusätzlichen Web-Seitenbesuch verfolgen möchten. `mktoMunchkinFunction('visitWebPage', { url: '/MyFlashMovie/Story1', params: 'x=y&2=3' });` Hinweis Bitte stellen Sie sicher, dass Sie Zugriff auf einen erfahrenen JavaScript-Entwickler haben. Der technische Support von Marketo unterstützt nicht bei der Fehlerbehebung bei benutzerdefiniertem JavaScript. Mit der Munchkin JavaScript-API können Sie ein Web-System von Drittanbietern in Ihr Marketo-Konto integrieren. Bei einigen Web-Entwicklungen können Sie neue Leads erfassen oder aktuelle Leads mit vorhandenen Programmen auf Ihrer Website aktualisieren. Angenommen, Sie verfügen über eine Web-Anwendung für die Kundenregistrierung, in der neue Kundeninformationen erfasst werden. Mit nur wenig Programmierung können Sie auch Lead-Informationen für die in Marketo erfassten Benutzer sowie ein Marketo-Cookie für das zukünftige Webtracking bereitstellen.

Darüber hinaus können Web-Entwickler mit einer anderen Funktion Web-Aktivitätsinformationen aus komplexen Web-Umgebungen wie Flash oder Ajax erfassen und verfolgen. Hinweis: Wenn Sie über die entsprechenden Entwicklungsressourcen verfügen, sollten Sie anstatt dieser API unsere SOAP-API für die Integration verwenden. Die SOAP-API ist robuster und verfügt über mehr Funktionen als die Munchkin-API. Marketo SOAP-API-Anforderungen Sie müssen den Munchkin-JavaScript-Code auf Ihrer Web-Seite einfügen, damit dies funktioniert. Die erforderlichen Skript-Tags finden Sie im Munchkin-Tutorial. Aktivieren Sie auch die Munchkin-API , die ebenfalls im Tutorial beschrieben wird.
Unter der Haube wird der Benutzer nach einem Munchkin-API-Aufruf automatisch Cookies erhalten, wenn er kein Cookie hat. In Marketo wird das Ereignis (Klick auf einen Link, Besuch einer Web-Seite oder neuer Lead) im Aktivitätsprotokoll der Person protokolliert. Wenn Sie den Klick-Link verwenden oder einen Web-Seiten-Aufruf besuchen, wird das Ereignis zum Aktivitätsprotokoll dieses Leads hinzugefügt (bekannt oder anonym). Wenn es sich um einen neuen Lead handelt und Sie den Lead-Aufruf „Verknüpfen“ verwenden, wird dieser Lead zu einem bekannten Lead und der Aktivitätsverlauf wird beibehalten. Wenn es sich um einen bestehenden Lead handelt (basierend auf der Übereinstimmung der E-Mail-Adressen), werden alle geänderten oder neuen Werte im Datensatz dieses Leads aktualisiert.

Hier ist die allgemeine Form eines `munchkinFunction`. Binden Sie sie als Tags an beliebiger Stelle in Ihre Web-Seite ein. Sie können dies wie jede andere JavaScript-Funktion aufrufen. Sie müssen jedoch die Munchkin-Tracking-Funktion `mktoMunchkin()` aufrufen, bevor Sie `mktoMunchkinFunction()` Aufrufe ausführen:

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"> mktoMunchkin("###-###-###"); mktoMunchkinFunction('function', { key: 'value', key2: 'value'}, 'hash');
```

Wobei: `###-###-###` die Munchkin-Konto-ID für Ihre Kontofunktion ist der Aufruf, bei dem Sie Parameter zu einem Array von Parametern machen möchten, die für den Aufruf-Hash benötigt werden, nur für `associateLead`

Veröffentlicht am _1970-01-01_ von _Murta_

## Daten in Marketo einbringen

Die folgende Präsentation zeigt Ihnen die verschiedenen Möglichkeiten, Daten in Marketo zu importieren. Der Fokus liegt auf Formularen, benutzerdefinierten Objekten und Integrationen.

[Daten in Marketo einbringen](https://www.slideshare.net/MurtzaManzur/getting-data-into-marketo-35662408) von [Murtza Manzur](https://www.slideshare.net/MurtzaManzur)

Veröffentlicht am _2014-06-06_ von _Murta_

## Erstellen von Leads in einer Workspace

Nehmen wir an, Ihr Unternehmen hat zwei Geschäftsbereiche: Nordamerika und Europa. Sie möchten Ihre Leads nach Unternehmensbereichen in Marketo segmentieren. Dies können Sie mit Arbeitsbereichen erreichen, eine Funktion in Marketo, mit der Sie den Zugriff auf Leads einschränken können. Dazu würden Sie einen Arbeitsbereich für Nordamerika und einen weiteren für Europa erstellen. Anschließend können Sie mit der „syncLead-API“ einen Lead in [ bestimmten Arbeitsbereich ](/help/soap-api/synclead.md). Sie sollten die Verwendung von Arbeitsbereichen und Lead-Partitionen in Betracht ziehen, wenn Ihr Unternehmen über Folgendes verfügt:

1. Separate Marketing-Teams für mehrere Produktlinien
1. Separate Marketing-Teams für verschiedene Gebiete oder Länder
1. Muttergesellschaft und Tochtergesellschaften
1. Muttergesellschaft und Händler

Wenn Sie Lead-Partitionen und Arbeitsbereiche verwenden, können Sie:

1. Beschränken des Zugriffs auf Leads in Ihrer Organisation
1. Beschränken des Zugriffs auf Assets in Ihrer Organisation
1. Freigeben von Assets über Marketing-Teams hinweg

Wir zeigen Ihnen zunächst, wie Sie einen Arbeitsbereich in Marketo über die Benutzeroberfläche erstellen, und dann, wie Sie mithilfe der „syncLead[API einen Lead in diesen Arbeitsbereich schreiben](/help/soap-api/synclead.md). **Erstellen einer Workspace** Ein Arbeitsbereich ist ein Satz von Leads und Marketo-Assets. In einem Arbeitsbereich können Sie nur Leads aus diesem Arbeitsbereich und Assets (E-Mails, Kampagnen, Listen usw.) in diesem Arbeitsbereich sehen. Intelligente Kampagnen in diesem Arbeitsbereich wirken sich nur auf Leads in diesem Arbeitsbereich aus. So zeigen Sie die Arbeitsbereiche in Ihrem Konto an:

1. Navigieren Sie zur Seite Arbeitsbereiche und Lead-Partitionen des Abschnitts Admin . Ihre Arbeitsbereiche werden auf der Registerkarte Arbeitsbereiche angezeigt. 1. Um einen neuen Arbeitsbereich zu erstellen, klicken Sie in der Menüleiste der Registerkarte Arbeitsbereiche auf die Schaltfläche Neue Workspace .
1. Im Dialogfeld müssen Sie einige Informationen zum neuen Arbeitsbereich hinzufügen:

* **Workspace-**: Der Name dieses Arbeitsbereichs, wie er in der Benutzeroberfläche angezeigt wird
* **Beschreibung** - eine optionale Textbeschreibung des Arbeitsbereichs
* **Lead-Partitionen** - welche Leads in dieser Partition sichtbar sind.
* **Alle Lead-Partitionen** - sieht Leads aus allen aktuellen und zukünftigen Partitionen
* **Einzelne Partitionen auswählen** - nur Leads von diesen Partitionen sind sichtbar (und keine zukünftigen Partitionen)
* **Primäre Lead-**: Leads, die Ihre Landingpages besuchen, werden dieser Partition standardmäßig hinzugefügt
* **Language** - die Geschäftssprache des Arbeitsbereichs

Wir zeigen Ihnen, wie Sie die API-Schreibzugriffe verwenden, um zu einem bestimmten Arbeitsbereich zu gelangen. Das erste Beispiel ist eine XML-Anfrage, das zweite eine XML-Antwort und das letzte Beispiel ist ein Ruby-Code-Beispiel, das zum Generieren der XML-Anfrage verwendet werden kann. 1. Nach dem Erstellen des Leads ist die Lead-Partition ein Feld unter Lead-Informationen. SOAP-Anfrage für `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-Antwort für requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Unten finden Sie ein Java-Beispielprogramm, das das oben beschriebene Szenario ausführt.

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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

**Wie melde ich mich für Arbeitsbereiche und Lead-Partitionen an?** Für Marketo Enterprise-Kunden haben Sie freien Zugriff auf Arbeitsbereiche und Lead-Partitionen. Wenden Sie sich an Ihren Customer Enablement Manager, um weitere Informationen zur Aktivierung und Implementierung zu erhalten. Für andere Kunden wenden Sie sich bitte an Ihren Marketo-Vertriebsmitarbeiter oder wenden Sie sich per E-Mail an unser Vertriebsteam, um weitere Informationen zu diesem Produkt zu erhalten.

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _1970-01-01_ von _Murta_

## Updates vom Juni 2014

### Marketo Real-Time Personalization-API

Die JavaScript-API von Marketo Real-Time Personalization (RTP) erweitert die automatisierte Personalisierungsfunktion der Plattform. Sie ermöglicht die Ereignisverfolgung und dynamische Anpassung einer Web-Seite. Die vollständige Dokumentation finden [hier](/help/javascript-api/web-personalization.md).

Veröffentlicht am _2014-06-20_ von _Travis Kaufman_

## Speichern eines Fremdschlüssels in Marketo

Bei der Synchronisierung von Kontakt- und Lead-Datensätzen zwischen Systemen wie einem proprietären CRM-System oder Data Warehouse ist es gängige Voraussetzung, einen Lead-Datensatz mit einer eindeutigen Systemkennung zu verknüpfen. In Marketo können Sie einen Lead-Datensatz über einen Aufruf [syncMultipleLeads API](/help/soap-api/syncmultipleleads.md) mit Ihrer eindeutigen Systemkennung erstellen oder aktualisieren. Dazu würden Sie Ihre eindeutige Systemkennung (Primärschlüssel) als Fremdschlüssel in Marketo speichern. Der Name dieses Felds in Marketo zum Speichern eines Fremdschlüssels lautet „ForeignSysPersonId“. Im Folgenden sind drei wichtige Dinge zu beachten:

1. ForeignSysPersonId ist in der Benutzeroberfläche von Marketo nicht sichtbar. Es empfiehlt sich daher, auch ein benutzerdefiniertes Attributfeld mit diesem Wert auszufüllen.
1. ForeignSysPersonId ist für einen Lead eindeutig, aber ein Lead kann mehr als eine ForeignSysPersonId haben.
1. ForeignSysPersonId kann nicht aktualisiert oder gelöscht werden, sondern kann einem anderen Datensatz neu zugewiesen werden.

Wir zeigen, wie die „syncMultipleLeads[API“ aufgerufen wird](/help/soap-api/syncmultipleleads.md) um einen „ForeignSysPersonId“-Wert in zwei bestehende Lead-Datensätze in Marketo zu schreiben. **So schreiben Sie eine ForeignSysPersonId mithilfe der syncMultipleLeads** API: Sie können einen neuen Lead-Datensatz einfügen und die ForeignSysPersonId angeben. Sie können sie auch zu einem bestehenden Lead hinzufügen, indem Sie sowohl die Marketo-ID als auch die ForeignSysPersonId angeben. Wir führen Sie durch den letzteren Fall. **Anfrage-XML für syncMultipleLeads SOAP-API-Aufruf**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
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
          <leadId>1090240</leadId>
          <foreignSysPersonId>1224191</foreignSysPersonId>
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
          <leadId>1090239</leadId>
          <foreignSysPersonId>1224192</foreignSysPersonId>
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

**Antwort-XML für syncMultipleLeads SOAP-API-Aufruf**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
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

**Unten finden Sie ein Beispiel für ein Ruby-Programm, das die obige Anfrage-XML ausgibt.**

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
        :lead_record_list => {
                :lead_record => {
                        :lead_id => "1090240",
                        :foreign_sys_person_id => "1224191",
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
                        :lead_id => "1090239",
                        :foreign_sys_person_id => "1224192",
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

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-06-27_ von _Murta_

## Aktualisieren der E-Mail-Adresse eines Leads

Angenommen, ein Benutzer füllt ein Marketo-Formular auf Ihrer Site aus. Was passiert? Marketo verwendet Cookies für den Benutzer und verknüpft ihn mit der von ihm angegebenen E-Mail. Was passiert, wenn der Benutzer das nächste Mal Ihre Website besucht und dasselbe Formular mit einer anderen E-Mail erneut ausfüllt? Was wird passieren? Marketo erstellt einen neuen Lead-Eintrag und überschreibt das erste Cookie im Browser des Benutzers. Der/die Benutzende ist jetzt ein neuer/ein anderer Lead in Marketo. Wir zeigen Ihnen vier Möglichkeiten, die E-Mail-Adresse eines Leads in Marketo zu aktualisieren, einschließlich der [syncLead-API](/help/soap-api/synclead.md)-Methode, des benutzerdefinierten Felds in einer Formularmethode, der Marketo-Benutzeroberfläche und durch Importieren einer Liste. **Über die syncLead** API können Sie die [syncLead-API](/help/soap-api/synclead.md) verwenden, um einen Lead-Eintrag mit seiner Marketo-ID und der neuen E-Mail-Adresse zu aktualisieren. Anfordern von XML für `syncMultipleLeads` SOAP-API-Aufruf

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
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
        <leadId>1090240</leadId>
        <Email>t@t.com</Email>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Antwort-XML für syncMultipleLeads SOAP-API-Aufruf

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1090240</leadId>
        <syncStatus>
          <leadId>1090240</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Unten finden Sie ein Beispiel für ein Ruby-Programm, das die obige Anfrage-XML ausgibt.

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
 :lead_record => {
  :Email => "<t@t.com>",
  :lead_id => "1090240",
 :return_lead => "false"
}

response = client.call(:sync_lead, message: request)

puts response
```

**Über ein benutzerdefiniertes Feld in einem Formular** Sie in Marketo ein benutzerdefiniertes Feld für die „neue E-Mail-Adresse“ erstellen. Bitten Sie dann den Benutzer, ein Formular auszufüllen, das dieses neue Feld enthält. Erstellen Sie dann in Marketo ein Programm, das den Datenwert des Systemfelds E-Mail-Adresse mit dem Token `{{lead.newEmailAddress}}` ändert, wenn es eine Änderung im neuen benutzerdefinierten Feld „Neue E-Mail-Adresse“ gibt. **Über die Marketo-** können Sie die E-Mail-Adresse eines Leads über die Marketo-Benutzeroberfläche manuell aktualisieren. Im Folgenden finden Sie einen [Hilfeartikel](https://nation.marketo.com/) in dem beschrieben wird, wie Sie dies tun können (Anmeldung bei Marketo erforderlich, um den Artikel zu sehen). **Durch Importieren einer Liste** Sie die E-Mail-Adresse eines Leads mithilfe der hier beschriebenen Methode zum Importieren einer Liste in [Marketo) aktualisieren ](https://nation.marketo.com/)Marketo-Anmeldung erforderlich, um den Artikel zu sehen).  

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _1970-01-01_ von _Murta_

## Speichern einer zweiten E-Mail-Adresse für einen Lead

Angenommen, Sie möchten die Punktzahl eines Leads in Marketo mithilfe der -APIs ändern. Dies ist in Verbindung mit der REST-API mithilfe des Lead-Endpunkts „Erstellen/Aktualisieren“ möglich. Wenn Sie mehr als eine E-Mail in einem Lead-Datensatz speichern möchten, müssen Sie ein benutzerdefiniertes Feld erstellen und die zweite E-Mail dort speichern. Im Folgenden finden Sie einen Hilfeartikel mit weiteren Informationen: Im Folgenden finden Sie ein Codebeispiel in Ruby, das zeigt, wie dieser Aufruf erfolgt.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Veröffentlicht am _2015-02-20_ von _Murta_

## Erstellen Sie ein benutzerdefiniertes Feld in Marketo und aktualisieren Sie dieses Feld über die API

Angenommen, Sie verfügen über zusätzliche Daten zu Ihren Leads, die nicht in die standardmäßigen Marketo-Felder passen. Dieses benutzerdefinierte Feld könnte beispielsweise ein Drittanbieterwert sein. Sie können in Marketo ein benutzerdefiniertes Feld für die Bewertung von Drittanbietern erstellen und dann den Wert dieses Felds entweder über die Marketo-(REST[APIs) ](https://developer.adobe.com/marketo-apis/) [SOAP-APIs ](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/activity-type-filters). Wir zeigen Ihnen zunächst, wie Sie ein benutzerdefiniertes Feld in Marketo erstellen und wie Sie dieses Feld mithilfe der REST-API aktualisieren können.

### Erstellen eines benutzerdefinierten Felds in Marketo

1. Klicken Sie unter Admin auf Feldverwaltung.
1. Klicken Sie auf die Schaltfläche Neues benutzerdefiniertes Feld .
1. Wählen Sie den Feldtyp aus. Dies ändert die Darstellung in Smart Lists und Forms in Marketo.
1. Geben Sie den Namen ein, wie er in Marketo angezeigt werden soll. Wählen Sie den Namen und den API-Namen sorgfältig aus, da das Umbenennen von Feldern schwierig und in einigen Situationen nicht möglich sein kann.
1. Auf das von Ihnen erstellte benutzerdefinierte Feld kann jetzt über die APIs zugegriffen werden.

### Aktualisieren eines benutzerdefinierten Felds über die REST-API

Im vorherigen Abschnitt haben wir ein benutzerdefiniertes Feld namens `myCustomField` mit dem Datentyp „String“ erstellt. Um den Wert dieses Felds zu aktualisieren, verwenden wir den REST-API-Endpunkt namens Leads erstellen/aktualisieren . Bevor Sie eine Anfrage an die REST-API stellen können, müssen Sie sich authentifizieren. Dies würde den Rahmen dieses Artikels sprengen, aber detaillierte Informationen [auf der Marketo Developers&#39; Site](/help/rest-api/authentication.md).

**Endpunkt**

`/rest/v1/leads.json`

**Anfrageinhalt**

```json
{
   "action":"createOrUpdate",
   "input":[
      {
         "email":"<example@example.com>",
         "myCustomField":"examplestring"
      }
   ]
}
```

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-08-19_ von _Murta_

## Integrieren von Unbounce und Marketo

**HINWEIS: Dies ist ein Gastbeitrag von Fab Capodicasa. Er ist Marketo-zertifizierter Consultant bei [Hoosh Marketing](http://hooshmarketing.com.au/), einem Marketo LaunchPoint-Agenturpartner, der sich auf B2C spezialisiert hat. Seit 13 Jahren ist er sowohl im SaaS- als auch im Marketing-Bereich tätig. Sein Hintergrund ist eine Mischung aus Hardcore-IT, Direkt-Marketing und Enterprise Sales. Fab ist auch ein ehemaliger Marketo-Mitarbeiter.**

**Überblick** In diesem Artikel zeigen wir, wie Sie Unbounce, ein beliebtes Landingpage-Tool, mit Marketo integrieren. Wir zeigen Ihnen zuerst, wie Sie das Marketo-Tracking in Unbounce einfügen können, und dann, wie Sie Unbounce-Formulare ändern können, um Daten direkt in Marketo einzufügen. Die Herausforderung bei der Integration von Unbounce mit Marketo besteht darin, dass Unbounce das Umbenennen von Standardfeldern nicht zulässt (zum Beispiel kann first_name nicht in firstName geändert werden). Außerdem dürfen Feldbezeichnungen nicht von Feldnamen abweichen. Diese Integration umfasst JavaScript, das die vorhandenen Formulare optimiert, um sie mit Marketo kompatibel zu machen. Ich empfehle Ihnen, mindestens über Anfängerkenntnisse in JavaScript und fortgeschrittene Marketo-Kenntnisse zu verfügen, um die Aufgaben in diesem Artikel zu erledigen.
**Teil 1: Marketo-Trackingcode zu Unbounce hinzufügen** Das Hinzufügen des Munchkin-Trackingskripts von Marketo zu Unbounce-Seiten ist sowohl für Analytics als auch für die Formularintegration erforderlich, um zu funktionieren. Führen Sie die folgenden Schritte aus: Kopieren Sie Ihren Munchkin-Code aus Marketo: Navigieren Sie zu Admin > Munchkin und kopieren Sie die „einfache“ Version von JavaScript. Öffnen Sie die Landingpage Unbounce und klicken Sie auf JavaScript > Neue JavaScript hinzufügen .  Klicken Sie auf „Hinzufügen“, rufen Sie das Skript &quot;Munchkin&quot; auf, wählen Sie „Vor dem Body-End-Tag“ aus und fügen Sie dann den Munchkin-Code ein. Klicken Sie auf die Schaltfläche Fertig . Für zukünftige Unbounce-Seiten gehen Sie zu JavaScript und aktivieren Sie das von uns erstellte Munchkin-Skript. Sie muss nicht neu erstellt werden.
**Teil 2: Konvertieren Sie das Unbounce-Formular in ein Marketo-Formular** Jetzt müssen wir das Unbounce-Formular ändern, indem wir einige neue ausgeblendete Felder und JavaScript hinzufügen, damit Ihre Unbounce-Landingpages Lead-Informationen direkt an Marketo senden können. Zunächst erstellen wir ein Marketo-Platzhalterformular. Erstellen Sie in Marketo ein leeres Formular und genehmigen Sie es.

Dies ist das Proxy-Formular in Marketo, das das Formular „Unbounce“ darstellt. Fügen Sie dem Unbounce-Formular ausgeblendete Felder hinzu. Diese ausgeblendeten Felder sind von Marketo erforderlich, um zu bestimmen, für welche Marketo-Instanz, welches Formular und welche Benutzersitzung diese Formularübermittlung gelten soll. Öffnen Sie das Formular unter Unbounce per Doppelklick. Fügen Sie ein ausgeblendetes Feld namens `_mkt_trk` hinzu. Fügen Sie ein zweites ausgeblendetes Feld namens `formid` hinzu.  233 muss durch die ID Ihres Formulars ersetzt werden, die Sie im Einbettungs-Code des Marketo-Formulars in Marketo finden. Öffnen Sie in Marketo Ihr Formular und wählen Sie Formularaktionen > Einbettungs-Code aus. Fügen Sie ein ausgeblendetes Feld namens `returnurl` hinzu. `http://hooshmarketing.com.au/thank-you` muss durch eine Follow-up-URL ersetzt werden. Dies ist die URL, zu der Benutzer nach dem Absenden des Formulars weitergeleitet werden sollen. Dies könnte beispielsweise Ihre Dankeseite sein.
**Teil 3: Direktes Unbounce-Formular an Marketo** Die Folge-URL ist die Seite, auf die Ihr Lead weitergeleitet wird, nachdem sein Lead an Marketo gesendet wurde. Führen Sie im Abschnitt Unbounce die folgenden Schritte aus: Klicken Sie auf Ihr Formular. Ändern Sie den Abschnitt Formularbestätigung . Ändern Sie die Bestätigung so, dass Formulardaten an eine URL gesendet werden. Legen Sie die URL auf die gewünschte Folgeseite fest. `fpmarkets` muss durch Ihre Marketo-Kontozeichenfolge ersetzt werden, die in Marketo unter „Admin->Landingpages“ zu finden ist.
**Teil 4: JavaScript zur Unbounce-Seite hinzufügen** Diese JavaScript konvertiert das Formular, damit es mit Marketo kompatibel ist, und sendet es an Marketo. Führen Sie bei Unbounce die folgenden Schritte aus: Öffnen Sie die Landingpage Unbounce und klicken Sie auf JavaScript > Neue JavaScript hinzufügen . Klicken Sie auf „Hinzufügen“, rufen Sie das Skript &quot;Marketo Form Convert“ auf und wählen Sie „Vor dem Body-End-Tag“ aus. Fügen Sie den folgenden JavaScript-Code ein:

```javascript
var MARKETO_MUNCHKIN_ID='614-CGT-700';
var MARKETO_ACCOUNT_STRING = 'fpmarkets';

var UNBOUNCE_MARKETO_FIELD_MAP = new Object();

//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
UNBOUNCE_MARKETO_FIELD_MAP['last_name'] = 'LastName';
UNBOUNCE_MARKETO_FIELD_MAP['phone_number'] = 'Phone';

function getMarketoField(k) {
    return UNBOUNCE_MARKETO_FIELD_MAP[k];
}


var formFields = [];
var hiddenClonedFields = [];
var firstForm = document.forms[0];

//Convert Unbounce form names to Marketo field names
for(i=0; i<firstForm.elements.length; i++){
 var formField = firstForm.elements[i];
 var newFieldName = getMarketoField(formField.name);

  if(newFieldName != undefined) {


    //save original field as hidden field
    var hiddenField = document.createElement("input");
    hiddenField.setAttribute("type", "hidden");
    hiddenField.setAttribute("name", formField.name);
    hiddenField.setAttribute("id", formField.id);
    hiddenClonedFields.push(hiddenField);

    //change original field
    console.log ( 'Changed form field name: ' + formField.name + '=>' + newFieldName );
    formField.name = newFieldName;
    formField.id = newFieldName;
    formFields.push(formField);


  } else {
    console.log ( 'Couldn't map:' + formField.name );
  }
}

//Add hidden cloned Unbounce fields to form
//for Unbounce validation
for(i=0; i<hiddenClonedFields.length; i++){
    firstForm.appendChild(hiddenClonedFields[i]);
    formFields[i].onchange = (function(hf) {
        return function(event) {
            hf.value = event.target.value;
          console.log('Changed hidden cloned field:' + hf.name + '=>' + hf.value);
        };
    }(hiddenClonedFields[i]));
    console.log ( 'Added cloned field: ' + hiddenClonedFields[i].name );
}


//Add MunchkinId to form
var input = document.createElement("input");
input.setAttribute("type", "hidden");
input.setAttribute("name", "munchkinId");
input.setAttribute("value", MARKETO_MUNCHKIN_ID);
firstForm.appendChild(input);
console.log('Added hidden field:' + input.name + '=' + input.value);
```

Wenn Sie benutzerdefinierte Felder mit API-Namen haben, die nicht mit Unbounce kompatibel sind, können Sie diese zur Zuordnung in der JavaScript hinzufügen. Wenn beispielsweise eines Ihrer benutzerdefinierten Felder in Marketo `Comments_c` wurde, die Feldbezeichnung jedoch als Kommentare angezeigt werden soll, lässt sich der Feldname bei Unbounce nicht in `Comments_c` ändern.

```
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['comments'] = 'Comments_c';
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
```

_comments sind der Name des Felds in Unbounce. _Comments_c_ ist der Name des Felds in Marketo. Für zukünftige Unbounce-Seiten gehen Sie einfach zu JavaScript und aktivieren Sie das von uns erstellte Munchkin-Skript. Sie muss nicht neu erstellt werden.
**Teil 5: Testen** Der letzte Schritt besteht darin, zu testen, ob diese Formularintegration funktioniert. Erstellen Sie einen Trigger in Marketo, der beim Ausfüllen des Marketo-Formulars aktiviert wird, und stellen Sie sicher, dass Leads korrekt in Marketo eingefügt werden. Sobald das Formular übermittelt wurde, sollte die Seite Sie zur Folge-URL weiterleiten.

Veröffentlicht am _2014-08-04_ von _

## Versionsaktualisierungen Juli 2014

### Munchkin-Updates

Die neue Version von Munchkin ist 141. Version 142 wird nicht unterstützt und wird Anfang September 2011 entfernt. In Version 142 gab es öffentlich zugängliche Funktionen, die auf der Entwickler-Site nicht dokumentiert waren. Diese nicht dokumentierten Funktionen sind nicht mehr öffentlich zugänglich. Dokumentierte Funktionen auf der Entwickler-Site werden weiterhin langfristig unterstützt.

### RTP-Aktualisierungen

Die RTP-API verfügt über eine neue Funktion namens Besucherdaten abrufen . Mit diesem RTP-API-Aufruf können Sie Echtzeit-Besucherdaten wie Organisation, Branche, Standort und Segment-Code-Übereinstimmung abrufen.

Veröffentlicht am _2014-07-30_ von _Murta_

## Verwenden mehrerer Munchkin-Trackingcodes auf einer Seite

Angenommen, Sie verfügen über mehrere Marketo-Instanzen und möchten Webtracking-Ereignisse wie Seitenbesuche oder angeklickte Links zu diesen mehreren Instanzen senden, können Sie dies mit Munchkin tun. Marketo verfolgt Besucher Ihrer Website nach Domain (z. B. „marketo.com„). Wenn Sie dieses Munchkin-Skript auf einer Domain hosten, die sich von Ihrer primären Domain unterscheidet (z. B. „company.com„), werden diese Besucher als anonyme Leads angezeigt, bis sie ein Formular auf dieser anderen Domain ausfüllen. Fügen Sie dazu dem `altIds`-Aufruf den `Munchkin.init` hinzu. Der altIds-Parameter enthält ein Array der zusätzlichen Munchkin-IDs, an die Web-Ereignisse gesendet werden sollen. Ersetzen Sie mithilfe des folgenden Beispiels die hervorgehobenen Munchkin-IDs (XXX-XXX-XXX, YYYY-YYYY und ZZZ-ZZZ-ZZZ) durch die Munchkin-IDs aus jeder Marketo-Instanz, an die die Tracking-Informationen gesendet werden sollen.

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"></script>
<script>Munchkin.init('XXX-XXX-XXX', { altIds:['YYY-YYY-YYY', 'ZZZ-ZZZ-ZZZ'] });</script>
```

Weitere Informationen zu Munchkin-Initialisierungsparametern finden Sie [diesem Dokument](/help/javascript-api/configuration.md).

Veröffentlicht am _2014-08-08_ von _Murta_

## Integrieren von Munchkin mit Google Tag Manager

Mit Google Tag Manger können Sie Ihrer Website Tags hinzufügen. Anstatt jedes Tracking-Skript wie Munchkin manuell zum Quell-Code Ihrer Website hinzuzufügen, können Sie [Google Tag Manager](https://marketingplatform.google.com/about/tag-manager/) auf Ihrer Site platzieren und dann Tags wie [Munchkin](/help/javascript-api/lead-tracking.md) über die Benutzeroberfläche von Google Tag Manager hinzufügen. In diesem Beitrag zeigen wir Ihnen zunächst, wie Sie Munchkin-Trackingcode in Marketo generieren, und dann, wie Sie diesen Munchkin-Trackingcode zu Google Tag Manager hinzufügen.

### Generieren eines Munchkin-Trackingcodes

1. Klicken **oben rechts** der App auf „Admin“.
1. Klicken Sie in der **auf der linken Seite auf** Munchkin.
1. Wählen Sie **asynchron** für den Trackingcode-Typ aus.
1. Klicken Sie auf und kopieren Sie den JavaScript-Trackingcode.

**Hinzufügen von Munchkin-Trackingcode zum Google Tag-Manager**

1. Melden Sie sich bei Ihrem Google Tag Manager-Konto an und **fügen Sie ein neues Tag hinzu.**
1. Erstellen Sie ein neues **Benutzerdefiniertes HTML-Tag**.
1. Kopieren Sie Ihren Munchkin-Code, fügen Sie ihn in das Feld **HTML** ein und klicken Sie auf **Weiter**.
1. Wählen Sie **Auf allen Seiten auslösen** und klicken Sie auf **Tag erstellen.** Hinweis: Wenn Sie über eine Website mit extrem hohem Traffic verfügen, können Sie mit &quot;**auf einigen Seiten auslösen** Bereiche Ihrer Website ausschließen.
1. Klicken Sie auf Speichern und überprüfen Sie, ob der Munchkin-Trackingcode nun auf Ihrer Website geladen wird.

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-08-05_ von _Murta_

## Lightbox nach Formularübermittlung ausblenden

### Überblick

Der aktuelle, von Marketo generierte Lightbox-Einbettungs-Code verschwindet beim Übermitteln des Formulars nicht. Die Seite wird nach der Formularübermittlung neu geladen und das Formular wird erneut angezeigt. Wenn Sie eine Lightbox erstellen möchten, die nach der Formularübermittlung ausgeblendet wird, führen Sie die folgenden Schritte aus.

### Anleitung

1. Generieren Sie nach dem Erstellen des Formulars in Marketo den Einbettungs-Code. Klicken Sie dazu auf Formularaktionen und anschließend als Code-Typ auf Lightbox . Kopieren Sie diesen Code und fügen Sie ihn in einen Texteditor ein, damit Sie ihn im nächsten Schritt bearbeiten können.
1. Ersetzen Sie den gesamten Code nach „(form)“ in Ihrem Einbettungs-Code durch den folgenden Code:

```javascript
{
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Nach Schritt 2 sollte die fertige Version wie der unten stehende Code aussehen. Der Code kann jetzt auf Ihrer Website verwendet werden.

```javascript
<script src="http://app-sj04.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_0000"></form>
<script>MktoForms2.loadForm("http://app-sj04.marketo.com", "AAA-BBB-CCC", 0000, function (form){
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _21.08.2014_ von _Murta_

## Schnellstartanleitung für die Marketo-REST-API

In diesem Handbuch erfahren Sie, wie Sie Ihren ersten Aufruf an die Marketo-REST-API in zehn Minuten ausführen. Wir zeigen Ihnen, wie Sie einen einzelnen Lead mithilfe des REST-API[Endpunkts Lead nach ID abrufen ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Dazu führen wir Sie durch den Authentifizierungsprozess, um ein Zugriffstoken zu generieren, mit dem Sie eine HTTP-GET-Anfrage an [Lead nach ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) stellen. Dann stellen wir Ihnen den Code zur Verfügung, um die Anfrage zu stellen, die Lead-Informationen zurückgibt, die als JSON formatiert sind.

### Generieren eines Authentifizierungs-Tokens

>[!NOTE]
> Seit Juni 2025 wird das Authentifizierungstoken nicht mehr unterstützt. Sie müssen den Authentifizierungs-Header verwenden.
>

Mit einem benutzerdefinierten Service in Marketo können Sie beschreiben und definieren, auf welche Daten Ihre Anwendung Zugriff haben soll. Sie müssen als Marketo-Administrator angemeldet sein, um einen benutzerdefinierten Service zu erstellen und diesen Service mit einem einzigen Benutzer zu verknüpfen, der nur über eine API verfügt.

1. Navigieren Sie zum Admin-Bereich des Marketo-Programms.
1. Klicken Sie im linken Bedienfeld auf den Knoten Benutzer und Rollen .
1. Erstellen Sie eine neue Rolle. Anzeigen der Liste der Rollenberechtigungen durch Klicken auf „API-Zugriff“. Scrollen Sie jetzt nach unten und wählen Sie nur die benötigten Berechtigungen aus. In diesem Fall wählen wir einfach Schreibgeschützte Lead-Berechtigung aus.
1. Der nächste Schritt besteht darin, einen Benutzer zu erstellen, der nur über eine API verfügt, und ihn mit der API-Rolle zu verknüpfen, die Sie im vorherigen Schritt erstellt haben. Aktivieren Sie dazu bei der Benutzererstellung das Kontrollkästchen Benutzer nur API aktivieren .
1. Ein benutzerdefinierter Service ist erforderlich, um Ihre Client-Anwendung eindeutig zu identifizieren. Um ein benutzerdefiniertes Programm zu erstellen, gehen Sie zum Bildschirm Admin > LaunchPoint und erstellen Sie einen neuen Service.
1. Geben Sie den Anzeigenamen ein, wählen Sie den Servicetyp „Benutzerdefiniert“, geben Sie die Beschreibung und die in Schritt 1 erstellte Benutzer-E-Mail-Adresse an. Es wird empfohlen, einen beschreibenden Anzeigenamen zu verwenden, der entweder das Unternehmen oder den Zweck dieses benutzerdefinierten REST-API-Service darstellt.
1. Klicken Sie im Raster auf den Link „Details anzeigen“, um die Client-ID und das Client-Geheimnis abzurufen. Ihre Client-Anwendung kann die Client-ID und das Client-Geheimnis verwenden, um ein Zugriffstoken zu generieren.
1. Kopieren Sie Ihr Authentifizierungstoken und fügen Sie es in einen Texteditor ein. Ihr Authentifizierungs-Token sieht ähnlich wie im folgenden Beispiel aus:

`cdf01657-110d-4155-99a7-f986b2ff13a0:int`

### Ermitteln der Endpunkt-URL

Wenn Sie eine Anfrage an die Marketo-API stellen, müssen Sie Ihre Marketo-Instanz in der Endpunkt-URL angeben. Alle Nicht-Massen-API-Anfragen an die Marketo-REST-API folgen dem folgenden Format:

`<REST API Endpoint URL>/rest/`

Die REST-API-Endpunkt-URL finden Sie im Bedienfeld Marketo Admin > Web-Services . Ihre Marketo-Endpunkt-URL-Struktur sollte dem folgenden Beispiel ähneln:

`https://100-AEK-913.mktorest.com/rest/v1/lead/{id}.json`

**Hinweis: Massen-API-URLs haben nicht das Präfix &quot;/rest/&quot;. Für die Bulk-API sollten Sie nur den Host verwenden und den entsprechenden Pfad wie folgt anhängen:**

`https://100-AEK-913.mktorest.com/bulk/v1/leads/export/create.json`

### Verwenden des Authentifizierungs-Tokens zum Aufrufen von Get Lead by API

In den vorherigen Abschnitten haben wir ein Authentifizierungstoken generiert und die Endpunkt-URL gefunden. Wir stellen jetzt eine Anfrage an einen REST-API-Endpunkt mit dem Namen [Lead abrufen nach ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Die einfachste Möglichkeit, Ihre Anfrage an die Marketo-REST-API zu stellen, besteht darin, die URL in die Adressleiste Ihres Webbrowsers einzufügen. Im folgenden Format:

`https://<REST API Endpoint URL for your Marketo instance>/rest/v1/<API that you are calling>?access_token=<access_token>`

### Beispiel

`https://100-AEK-913.mktorest.com/rest/v1/lead/318581.json?access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

Wenn Ihr Aufruf erfolgreich ist, wird JSON mit dem folgenden Format zurückgegeben:

```json
{
    "requestId": "d82a#14e26755a9c",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2015-06-11T23:15:23Z",
            "lastName": "Doe",
            "email": "<jdoe@marketo.com>",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "John"
        }
    ],
    "success": true
}
```

Wenn Sie mehr über die Marketo-REST-APIs erfahren möchten, ist dies ein [guter Ausgangspunkt](https://developer.adobe.com/marketo-apis/).

Veröffentlicht am _2015-09-04_ von _David_

## Löschen des Marketo-Tracking-Cookies

Frage: „Wir haben auf unserer Website ein Formular eingerichtet, über das das Vertriebsteam Personen abmelden kann, die mündlich gebeten haben, aus E-Mail-Nachrichten entfernt zu werden. Wir stellen jedoch fest, dass sie, wenn sie in die E-Mail eintreten und übermitteln, dass sie mit dieser E-Mail-Adresse gekocht werden und beginnen, Warnhinweise für ihre Aktivität auf unserer Website zu erhalten. Das Formular, das wir haben, ist derzeit ein eingebettetes Formular. Hat jemand eine Möglichkeit gefunden, das Cook-Tracking für ein eingebettetes Formular zu deaktivieren, ich weiß, wie man das mit einer Marketo-Landingpage macht.“ Wir können das tun. Um dies zu implementieren, beschleunigen Sie den Ablauf des Cookies. Nachdem das Cookie vom Formular erstellt wurde, können Sie sein sofortiges Ablaufen erzwingen. Da das Cookie abgelaufen ist, löscht der Browser es automatisch. Zu Beginn dieses Prozesses verwenden wir die native Formularfunktion, um unter die Funktion namens `deleteCookie` aufzurufen.

Veröffentlicht am _2014-08-26_ von _Travis Kaufman_

## Integrieren einer Marketo-Landingpage mit Wordpress

Angenommen, Ihre Website wird mit Wordpress erstellt und Sie möchten eine Marketo-Landingpage auf einer der Seiten einbetten. Dies ist mit einem iframe möglich. Mit einem iFrame können Sie eine Seite in eine andere Seite einbetten. In diesem Beitrag zeigen Sie, wie Sie eine Marketo-Landingpage in eine Wordpress-Seite einbetten. **Auswahl eines Wordpress-Plugins** Wordpress Es gibt eine Reihe von hier ist ein Wordpress-Plugin, das Sie ermöglicht : `http://wordpress.org/plugins/iframe/` Hier sind einige Profis und CON&#39;s auf diesem Ansatz zu verwenden: `http://www.elixiter.com/marketo-landing-page-and-form-hosting-options/` Great post Murtza! Colby behält der iframe den Teil bei, bei dem Sie nach dem Absenden des Formulars zu einem PDF weitergeleitet werden. Wir haben auf unserer Seite seit langem iFrames verwendet. Großartiger Beitrag Murtza! Colby behält der iframe den Teil bei, bei dem Sie nach dem Absenden des Formulars zu einem PDF weitergeleitet werden. Wir haben auf unserer Seite seit langem iFrames verwendet. Hinweis: Wenn Sie URL-Parameter von der Haupt-URL an den iframe übergeben möchten, müssen Sie etwas codieren. Stellen Sie außerdem sicher, dass Ihre Google Analytics ordnungsgemäß eingerichtet ist. Sie möchten nicht, dass Seitenansichten für jeden Besuch auf der Seite mit dem iframe zweimal gezählt werden.

Veröffentlicht am _1970-01-01_ von _Murta_

## Integrieren von Optimizely mit einer Marketo-Landingpage

[Optimizely](https://www.optimizely.com/de) bietet die Möglichkeit, A/B-Tests, mehrseitige und multivariate Tests auf Ihrer Site durchzuführen. Sie können Optimizely mit einer Marketo-Landingpage verwenden. Gehen Sie wie folgt vor:

1. Suchen und kopieren Sie Ihren Optimizely-Code-Ausschnitt.** Navigieren Sie zu Ihrem Dashboard in Optimizely und klicken Sie auf den Link Projekt-Code . Kopieren Sie die Codezeile, die im Popup-Fenster angezeigt wird.
1. Melden Sie sich bei Marketo an und wählen Sie Ihre Landingpage-Vorlage aus. Klicken Sie dann auf „Entwurf bearbeiten“.**
1. Klicken Sie auf Landingpage-Aktionen. Klicken Sie dann auf Seite bearbeiten Meta-Tags **
1. Fügen Sie Ihr Optimizely-Codefragment in den Abschnitt Benutzerdefiniertes HEAD HTML ein und klicken Sie auf Speichern .
1. Testen Sie die Landingpage, um zu bestätigen, dass das Snippet Optimizely funktioniert

Veröffentlicht am _2014-09-18_ von _Murta_

## Suche nach vollständigem Namen über Marketo REST-API

**Frage:** Gibt es eine Möglichkeit, einen Lead mithilfe von Marketo-APIs mit nur dem vollständigen Namen eines Leads abzufragen?
**Antwort:** Es ist nicht direkt möglich. Mit der unten beschriebenen Problemumgehung können Sie dies jedoch tun.

1. Erstellen Sie in Marketo ein benutzerdefiniertes Feld namens „FullName“.
1. Verwenden Sie entweder [getMultipleLeads](/help/soap-api/getmultipleleads.md) SOAP-API oder [Abrufen mehrerer Leads nach Filtertyp](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), um Ihre Lead-Datenbank abzufragen. Fügen Sie Ihren Vor- und Nachnamen als Attribute in Ihre Anfrage an REST- oder SOAP-APIs ein.
1. Nachdem Sie Ihre Lead-Datenbank abgefragt haben, verketten Sie „Vorname“ und „Nachname“ für jeden Lead und speichern Sie diese Daten in einer Spalte „Vollständiger Name“. 1. Verwenden Sie [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) SOAP-API, um diese Daten in das benutzerdefinierte Feld „Vollständiger Name“ zu übertragen. Alternativ können Sie die API [Lead importieren](/help/rest-api/leads.md) verwenden oder eine CSV- oder XLS-Datei über die Marketo-Benutzeroberfläche importieren.
1. Jetzt können Sie mit der API Mehrere Leads nach Filtertyp abfragen [um nach diesem benutzerdefinierten Feld ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) suchen). Geben Sie „FullName“ als „filterType“ an und „filterValue“ wäre „Joe Johnson“ mit einem REST-API-Aufruf zum Abrufen mehrerer Leads nach Filtertyp.

Veröffentlicht am _2014-09-09_ von _Murta_

## Marketo-Tracking-Cookie löschen

Diese Methode sollte nur verwendet werden, wenn die gewünschte Wirkung darin besteht, das Cookie vollständig zu entfernen.

Dieses Codebeispiel kann verwendet werden, um ein Marketo-Tracking-Cookie nach der Übermittlung eines Marketo-Formulars aus dem Browser eines Benutzers zu löschen. Es funktioniert durch Aufrufen `deleteMarketoCookie` Methode, nachdem ein Benutzer ein Formular gesendet hat. Diese Methode läuft das Cookie ab, indem sie das Ablaufdatum auf ein Datum in der Vergangenheit festlegt. Das Standardverhalten des Browsers besteht dann darin, dieses Cookie zu löschen, da es abgelaufen ist.

Veröffentlicht am _2014-09-09_ von _Murta_

## Kostenlose E-Mail-Domains beim Ausfüllen des Formulars einschränken

Angenommen, Sie möchten verhindern, dass Besuchende einer Website ein Formular mit einer kostenlosen E-Mail-Domain wie Gmail oder Yahoo senden. Fügen Sie dazu das folgende Skript in den Quellcode der Seite mit dem Marketo-Formular ein. Dabei wird geprüft, ob Benutzende eine geschäftliche E-Mail-Adresse eingegeben haben (Gmail, Hotmail usw.), und die Übermittlung von Marketo-Formularen wird verhindert, wenn Benutzende eine eingeben. Sie können die Liste der blockierten E-Mail-Domains erweitern, indem Sie Zeile 9 so ändern, dass auch Domains einbezogen werden, die beschränkt werden sollen.

Veröffentlicht am _2014-09-09_ von _Murta_

## Debuggen eines Felds, auf das nicht über die API zugegriffen werden kann

Wenn Sie zu diesem Feld kommen <https://nation.marketo.com/> Wenn wir versuchen, das Feld AnnualRevenue mithilfe der SOAP-API zu aktualisieren, lautet die Antwort, dass der Datensatz aktualisiert wird. Das Feld AnnualRevenue behält die Änderung jedoch nicht bei. Wenn wir versuchen, das Feld manuell mithilfe der Lead-Datenbank zu aktualisieren, werden die Änderungen an diesem Feld ebenfalls nicht beibehalten. Überprüfen Sie, ob das Feld in Admin blockiert ist. Die andere Sache, die manchmal passieren kann, ist, dass es sich um ein Kontofeld handeln kann, das von SFDC synchronisiert wird. Wir blockieren Änderungen an diesen Feldern standardmäßig, da SFDC in vielen Fällen das Aufzeichnungssystem für sie ist. 1) Erstellt am 2) Marketo SFDC ID 3) Marketo Unique Code 4) SFDC Typ 5) aktualisiert am 6) Dringlichkeit 7) Priorität 8) Erstellungsdatum des Verkaufs

Veröffentlicht am _1970-01-01_ von _Murta_

## Hinzufügen von benutzerdefiniertem Code zu einer Marketo-Landingpage

Angenommen, Sie möchten Ihrer Marketo-Landingpage ein Tracking-Skript eines Drittanbieters hinzufügen, z. B. Google Analytics. Dies ist über die Benutzeroberfläche von Marketo möglich. Im Allgemeinen können Sie der Marketo-Landingpage beliebigen benutzerdefinierten Code (HTML, CSS, JavaScript) hinzufügen, indem Sie die folgenden Anweisungen befolgen.

1. Wählen Sie Ihre Landingpage aus und klicken Sie auf Entwurf bearbeiten .
1. Ziehen Sie das Element HTML .
1. Geben Sie Ihren benutzerdefinierten Code ein und klicken Sie auf Speichern .

Veröffentlicht am _2014-09-10_ von _Murta_

## Server-seitige Formularbereitstellung

`https://community.marketo.com/MarketoResource?id=kA650000000GsXXCA0`

Veröffentlicht am _2014-09-11_ von _Murta_

## Löschen des Marketo-Tracking-Cookies aus den Forms 2.0-Übermittlungen

### Überblick

Forms 1.0 enthielt den Wert für das Munchkin-Tracking-Cookie als Feld im DOM. Dieser wurde zusammen mit allen anderen Vorleistungen übermittelt. [Forms 2.0](/help/javascript-api/forms-api-reference.md) lässt dieses Feld weg und füllt den Wert dynamisch beim Senden und nicht beim Laden des Formulars. Dies ist zwar im Allgemeinen akzeptabel, es erstellt jedoch eine Klasse von Anwendungsfällen, für die das Tracking-Cookie gelöscht werden muss, um unbeabsichtigtes Tracking und Vorbefüllen zu vermeiden. Dies kann beispielsweise auf einer Fachmesse geschehen, auf der ein Marketo-Kunde dasselbe Formular auf demselben Gerät verwendet und Kontaktinformationen von mehreren Personen erhält. Der folgende Ausschnitt ermöglicht es Ihnen, den Cookie-Wert bei der Übermittlung des Formulars zu löschen, ohne das Cookie selbst aus dem Browser des Benutzers löschen zu müssen.

### Codebeispiel 

Dieser Ausschnitt erwartet ein einzelnes Formular, das auf der Seite geladen wird. Wenn mehrere Formulare vorhanden sind, sollten Sie die Methoden [loadForm oder getForm](/help/javascript-api/forms-api-reference.md) verwenden, um Ihre Callbacks zu implementieren.

```javascript
<script>
//add a callback to the first ready form on the page
MktoForms2.whenReady( function(form){
 //add the tracking field to be submitted
        form.addHiddenFields({"_mkt_trk":""});
        //clear the value during the onSubmit event to prevent tracking association
 form.onSubmit( function(form){
  form.vals({"_mkt_trk":""});
 })
})
</script>
```

Veröffentlicht am _2014-09-11_ von _Kenny_

## Updates vom September 2014

### Aktualisierungen der REST-API

Der API [Abrufen mehrerer Leads“ wurde ein neuer optionaler Feldwert hinzugefügt](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) der die mit einem Lead-Datensatz verknüpften Munchkin-Cookie-Werte zurückgibt. Fügen Sie einfach &quot;?fields=cookies“ zur Anfrage hinzu.

Veröffentlicht am _2014-09-16_ von _Murta_

## Hinzufügen von Standortdaten von der RTP-API zum Ausfüllen des Marketo-Formulars

**Sie benötigen aktive RTP- und MLM-Abonnements, um den in diesem Blogpost beschriebenen Anwendungsfall zu implementieren.**
Mit den [RTP JavaScript APIs](/help/javascript-api/web-personalization.md) und [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md) können Sie die abgeleiteten Standortdaten aus RTP abrufen und über ein ausgefülltes Formular in Marketo übertragen. Auf diese Weise können Sie den abgeleiteten Speicherort des Benutzers (basierend auf der IP-Adresse) während der letzten Formularaktivität anzeigen. Zunächst müssen Sie drei benutzerdefinierte Zeichenfolgenfelder in Marketo erstellen. Sie können dies entweder über Ihr CRM-System vornehmen, wenn es über eine native Integration mit Marketo verfügt, oder über das Menü „Feldverwaltung“ im Admin-Bereich von Marketo. Ich empfehle, diese Felder als „Zuletzt verwendet“, „Zuletzt verwendet“ und „Zuletzt verwendet“ zu benennen. Wir setzen diesen Blog mit dieser Namenskonvention fort. Die API-Namen für diese Felder lauten „mostRecentCountry“, „mostRecentState“ und „mostRecentCity“. Um die Standortdaten abzurufen, verwenden wir die [RTP-Methode, um die Standortdaten des Besuchers abzurufen](/help/javascript-api/web-personalization.md) und sie dann mithilfe der [addHiddenFields- und vals-Methoden](/help/javascript-api/forms-api-reference.md) aus Marketo Forms 2.0 in das Formular zu übergeben. Fügen Sie auf Ihrer Seite Ihr RTP-JS-Tag und ein Marketo-Formular hinzu. Fügen Sie dann das unten stehende Skript ein. Sie müssen die Namen der Zielformularfelder im Beispielcode ändern, wenn Sie eine andere Namenskonvention als die oben beschriebene verwenden.

```javascript
<script>
//modify the form and grab the user
MktoForms2.whenReady( function(form) {
        //add the hidden fields to the form
 form.addHiddenFields({
  "mostRecentCountry":"",
  "mostRecentState":"",
  "mostRecentCity":""});
 //Grab the visitor data, a JS object with it is passed in the callback function of the third argument
 rtp('get','visitor',function(visitor){
  //add the desired info from the visitor object to the form fields
  form.vals({
   "mostRecentCountry":visitor.results.location.country,
   "mostRecentCity":visitor.results.location.city,
   "mostRecentState":visitor.results.location.state});
  }
  );
 });
</script>
```

Veröffentlicht am _2014-09-17_ von _Kenny_

## Blockieren des Crawlings und der Suchindizierung einer Marketo-Landingpage

Angenommen, Sie möchten verhindern, dass eine Marketo-Landingpage von Suchmaschinen wie Google durchsucht und in Ergebnissen angezeigt wird. Gehen Sie wie folgt vor:

1. Melden Sie sich bei Marketo an und wählen Sie Ihre Landingpage aus. Klicken Sie dann auf „Entwurf bearbeiten“.
1. Klicken Sie auf Landingpage-Aktionen. Klicken Sie dann auf Seite bearbeiten Meta-Tags .
1. Ändern Sie das Feld Robots in No Index, No Follow (Kein Index). Klicken Sie dann auf Speichern.

Veröffentlicht am _2014-09-19_ von _Murta_

## Häufig gestellte Fragen zu Marketo REST und SOAP-APIs

**Aktualisiert: März 2016** Hier finden Sie Antworten auf die am häufigsten gestellten Fragen zu Marketo [REST](/help/rest-api/rest-api.md) und [SOAP](/help/soap-api/soap-api.md) APIs. **F: Was sind die Hauptunterschiede zwischen den Marketo REST- und SOAP-APIs?** A: Während sich die Möglichkeit, bestimmte Daten über REST- und SOAP-APIs per Push/Pull abzurufen, meistens überschneidet, gibt es bestimmte Funktionen, die nur in REST- oder SOAP-APIs vorhanden sind. In Bezug auf die Leistung hat die REST-API einen besseren [Durchsatz](https://en.wikipedia.org/wiki/Throughput) als die SOAP-API. Was das Authentifizierungsmodell betrifft, verfügt die REST-API über ein Authentifizierungsmodell, das ein ablaufendes Token verwendet. Unsere REST-API bietet auch Zugriff auf Marketo [Assets](https://developer.adobe.com/marketo-apis/api/asset/).   **F: Welche Funktionen sind in der REST-API verfügbar, die nicht in der SOAP-API verfügbar sind?** A: [List of lists API](/help/rest-api/list-of-standard-fields.md), [remove a lead from a list API](/help/rest-api/lead-database.md), [Usage API](/help/rest-api/rest-api.md), and [Error API](/help/rest-api/rest-api.md) sind nur mit der REST API verfügbar. **F: Gibt es Pläne, die Anzahl der für die SOAP-API verfügbaren APIs zu erhöhen?** A: Nein. **F: Gibt es Pläne, die Anzahl der für die REST-API verfügbaren APIs zu erhöhen?** A: Ja. REST steht derzeit im Mittelpunkt der API-Entwicklung in Marketo.

Veröffentlicht am _2014-09-_) von _Murta_

## Server-seitige Formularbereitstellung

**Hinweis: Dies ist eine nicht öffentliche und nicht unterstützte API. Sie wird nicht unterstützt, und ihr Verhalten kann sich jederzeit ändern.** Sie Ihre eigenen Formulare auf Ihrer Website verwenden, können Sie diese Daten mithilfe eines serverseitigen Postings an Marketo senden. Der Vorteil dieses Ansatzes besteht darin, dass Sie Ihre bestehende Formular- und Programmlogik beibehalten können, aber weiterhin einen tatsächlichen Formularpost in Marketo verwenden können. Dadurch erhalten Marketo-Benutzende ein Ereignis „Formular ausfüllen“, das zum Trigger automatisierter Prozesse oder zur Segmentierung verwendet werden kann. **HINWEIS: Es gibt eine Ratenbeschränkung von 30 Server-seitigen Formular-Beiträgen pro Minute von einer einzigen IP-Adresse.** In jeder Skript- oder Programmiersprache gibt es verschiedene Optionen für eine Server-seitige Formularübermittlung, und sie können unterschiedliche Objekte/Methoden für den POST-Aufruf verwenden. In PHP verwenden zum Beispiel viele Leute die cURL-Bibliothek. In allen Fällen senden Sie Name-Wert-Paare an eine angegebene URL. Die Namen müssen mit den API-Namen der Marketo-Felder übereinstimmen. Darüber hinaus gibt es eine Reihe von Systemfeldern, die einbezogen werden müssen, um die Formularübermittlung korrekt zu erfassen.

1. Erstellen eines Formulars. Der erste Schritt besteht darin, ein Formular in Marketo zu erstellen oder ein bestehendes Formular zu verwenden, das Sie senden möchten. Der Name des Formulars muss beschreibend sein, erfordert jedoch keine Formularfelder. Wenn Sie ein neues Formular erstellen, geben Sie einfach einen Namen ein, deaktivieren Sie das Kontrollkästchen „Formular-Editor öffnen“ und Sie sind fertig.
1. Formular-ID suchen. Wählen Sie in der Benutzeroberfläche von Marketo das Formular aus und sehen Sie sich die URL an: Sie sollte im Format `https://app-x.marketo.com/#FO8B2ZN12` sein. Hinter dem #-Zeichen suchen Sie die Zahl, die unmittelbar auf „FO“ folgt, um die Formular-ID zu finden. In diesem Fall ist die Formular-ID 8. In einigen Fällen ist Ihr erstes Formular möglicherweise mit 1001 nummeriert und zählt von dort aus. Die Formular-ID ist eine Variable, mit der Sie die Übermittlung verschiedener Formulare als Trigger festlegen können.
1. Rufen Sie Ihre Marketo-Konto-ID ab. Wechseln Sie zu Admin > Munchkin und kopieren Sie die Munchkin-Konto-ID im Format 000-AAA-000 . Sie benötigen diese, damit das Formular an die richtige Marketo-Instanz gesendet wird.
1. Bestimmen Sie die POST-URL. Notieren Sie sich in der Marketo-Benutzeroberfläche die Domain in der Speicherortleiste, normalerweise im `<http://app-x.marketo.com/>`. Verwerfen Sie alles nach dem Schrägstrich und hängen Sie dann „index.php/leadCapture/save“ an, um die vollständige Formular-POST-URL zu erhalten. Hinweis 1: Hierbei wird zwischen Groß- und Kleinschreibung unterschieden. Hinweis 2: Marketo-Sandboxes können eine andere Domain als Ihr Marketo-Produktionssystem haben. Eine Beispiel-URL wäre also: `http://app-x.marketo.com/index.php/leadCapture/save` Sie können auch HTTPS anstelle von HTTP verwenden (verwenden Sie nicht Ihren CNAME, da er eine Sicherheitsausnahme verursacht).
1. Suchen Sie die Formularfeldnamen** Gehen Sie zu Admin > Feldverwaltung und klicken Sie auf die Schaltfläche „Feldnamen exportieren“, um eine Tabelle mit den API-Feldnamen herunterzuladen. Verwenden Sie den API-Namen als Namen in Ihren Name-Wert-Paaren.
1. Festlegen, welche Felder gepostet werden sollen. Sie können bei der Formularübermittlung jedes beliebige Marketo-Lead-Feld einbeziehen. Beachten Sie, dass bei Feldnamen zwischen Groß- und Kleinschreibung unterschieden wird. Zusätzlich zu den Feldern, die Sie senden möchten, gibt es zwei Pflichtfelder und zwei Empfehlungsfelder: Pflichtfelder im Formular: (1) `munchkinId` - Dieses Feld wird für Ihre Munchkin-Konto-ID verwendet (2) `formid` - Dieses Feld gibt an, welches Formular in Marketo übermittelt wurde Empfohlene Felder im Formular: (1) E-Mail - Dieses Feld wird als Primärschlüssel für die Deduplizierung verwendet. Wenn Marketo eine entsprechende E-Mail-Adresse in der Marketo-Datenbank findet, wird der vorhandene Datensatz aktualisiert, andernfalls wird ein neuer Datensatz erstellt. Wenn mehrere Übereinstimmungen vorliegen, wird der zuletzt aktualisierte Datensatz (2) `_mkt_trk` aktualisiert. Dieses Feld enthält die Cookie-Informationen, sodass Sie die Web-Seitenbesuche des Kontakts verfolgen können. Wenn sich Munchkin auf der Formularseite befindet, gibt Munchkin automatisch einen Wert in dieses ausgeblendete Formularfeld ein. Andernfalls lesen Sie es aus dem Cookie mit demselben Namen und übergeben Sie es in diesem Feld an Marketo. Hinweis: Der Text des Formulars POST an Marketo muss URL-kodiert sein.
1. Siehe Antwort** Die Antwort auf den Formularpost ist ein HTTP-302-Umleitungs-Code. In einigen Systemen wird dies als Fehler angezeigt. In diesem Fall bedeutet dies jedoch, dass der Lead erfolgreich erstellt oder aktualisiert wurde. Wenn ein Fehler auftritt, erhalten Sie einen Fehlercode 4xx oder 5xx.

Hier ist ein [Beitrag](https://nation.marketo.com:443/t5/product-blogs/how-to-build-an-external-subscription-center/ba-p/242185) über die Verwendung dieser Technik für Abmeldeszenarien [von Justin Cooperman, Sr. Product Manager]

Veröffentlicht am _2014-11-07_ von _Murta_

## Aktualisierte Leads für bestimmten Datumsbereich suchen

Angenommen, Sie möchten Leads finden, die an bestimmten Daten über die [Marketo-API aktualisiert ](/help/soap-api/soap-api.md). Dies ist mit der SOAP-API [getMultipleLeads“ ](/help/soap-api/getmultipleleads.md). Diese Methode gibt alle Leads mit einer Datenwertänderung oder einer neuen Aktivität in Marketo für den von Ihnen angeforderten Datumsbereich zurück. Für die `leadSelector` geben Sie `LastUpdateAtSelector` an. Anschließend würden Sie die Datumsbereiche mit `oldestUpdatedAt` und `latestUpdatedAt` Zeitgrenzen definieren. Unten finden Sie eine Beispiel-Anfrage-XML, die Ihnen zeigt, wie Sie Leads finden, die zwischen 12 Uhr PST am 6. Juni 2014 und 12 Uhr PST am 7. Juni 2011 aktualisiert wurden. Hinweis: Der Datumsbereich darf 30 Tage nicht überschreiten.

**Beispiel-Anfrage-XML zur Suche nach Leads, die nach Datum aktualisiert wurden**

```xml
<soapenv:Envelope xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
<soapenv:Header>
<mkt:AuthenticationHeader>
<mktowsUserId>REPLACE</mktowsUserId>
<requestSignature>REPLACE</requestSignature>
<requestTimestamp>2014-10-23T12:19:51-07:00</requestTimestamp>
</mkt:AuthenticationHeader>
</soapenv:Header>
<soapenv:Body>
<mkt:paramsGetMultipleLeads>
<leadSelector xsi:type="mkt:LastUpdateAtSelector">
<oldestUpdatedAt>2014-06-06T00:00:00-08:00</oldestUpdatedAt>
<latestUpdatedAt>2014-06-07T00:00:00-08:00</latestUpdatedAt>
</leadSelector>
</mkt:paramsGetMultipleLeads>
</soapenv:Body>
</soapenv:Envelope>
```

Veröffentlicht am _2014-09-24_ von _Murta_

## Hinzufügen dynamischer Inhalte zu einer E-Mail

Angenommen, Sie senden eine tägliche E-Mail und möchten das Datum dieses Tages automatisch in die E-Mail-Vorlage aufnehmen. Verwenden Sie dazu Token und E-Mail-Scripting in Marketo.

1. Erstellen Sie ein Token.** Navigieren Sie zu dem Programm, in dem Sie das Token verwenden möchten. Klicken Sie auf Meine Token.
1. Doppelklicken Sie auf E-Mail-Script. Geben Sie diesem Token einen Namen. Klicken Sie dann auf Bearbeiten.
1. Fügen Sie das unten stehende E-Mail-Skript in dieses Fenster ein. Klicken Sie dann auf Speichern.

## Zugriff auf das Velocity-Kalenderobjekt

`set($x = $date.calendar)`

## Datum formatieren

`set($current_date = $date.format('dd-MM-yyyy', $x.getTime()))`

## Gibt das heutige Datum zurück.

`$current_date`

1. Referenzieren Sie das Token in der E-Mail-Vorlage.** Notieren Sie den Namen des Tokens. Navigieren Sie zu Ihrem E-Mail-Entwurf. Token einschließen.  Wenn die E-Mail gesendet wird, wird der Wert des Tokens ausgefüllt. Weitere Informationen finden Sie in der [Entwicklerdokumentation für E-Mail-Skripterstellung](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/email-scripting).

Veröffentlicht am _2014-11-22_ von _Murta_

## Bash-Sicherheitsberatung

Marketo hat die Bash-Schwachstelle, auch bekannt als [Shellshock (CVE-2014-6271)](https://nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-6271), eingehend untersucht und ist zu dem Schluss gekommen, dass wir für diese Angriffe nicht anfällig sind. Darüber hinaus haben wir vorbeugende Maßnahmen ergriffen, indem wir die Software auf die neueste Version aktualisiert haben, um die Einhaltung der [CERT-Empfehlung](https://www.cisa.gov/news-events/alerts/2014/09/25/gnu-bourne-again-shell-bash-shellshock-vulnerability-cve-2014-6271) sicherzustellen.

Veröffentlicht am _2014-09-26_ von _Murta_

## Aktualisieren der SOAP-API-Anmeldeinformationen

Es empfiehlt sich, die Anmeldeinformationen für die [SOAP-API](/help/soap-api/soap-api.md) regelmäßig zu aktualisieren. Derzeit gibt es keine Möglichkeit, dies programmgesteuert über die Marketo-API zu tun. Die folgenden Anweisungen zeigen Ihnen, wie Sie Ihre SOAP-API-Anmeldeinformationen über die Marketo-Benutzeroberfläche aktualisieren.

1. Gehen Sie zum Abschnitt Admin und klicken Sie auf Web-Services.
1. Legen Sie einen Verschlüsselungsschlüssel mit mindestens 10 Zeichen fest und klicken Sie auf Änderungen speichern.

Veröffentlicht am _2014-09-29_ von _Murta_

## Bereinigen der Marketo-Datenbank

**HINWEIS: Dies ist ein Gast-Blogpost. Josh Hill ist Marketo Practice Lead bei Perkuto, einer Agentur für Marketing-Automatisierung. Josh arbeitet an der Schnittstelle von Marketing, Vertrieb und Technologie, um umsatzgenerierende Systeme bereitzustellen. Er schreibt über Marketing-Automatisierung und Nachfragegenerierung unter [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).** Die Bereinigung Ihrer Marketo-Datenbank ist ein wichtiger Bestandteil der Verwaltung dieses leistungsstarken Systems. Wenn Ihre Marketo-Daten unsauber sind oder Ihre CRM-Daten unsauber sind, wird Ihr Job als Marketing-Manager um einiges schwieriger, da Sie erklären, warum Kampagnen an falsche Personen gesendet wurden oder Ihre Berichte „gerichtet“ sind. [In einer Gartner-Studie aus dem Jahr 2011 wurde festgestellt](https://www.data.com/export/sites/data/common/assets/pdf/DS_Gartner.pdf) dass eine schlechte Datenqualität die Arbeitsproduktivität um 20 % senkte. Das sind 20 % Ihrer Zeit (8 Stunden/Woche oder 1 Tag pro Woche), die verschwendet wird, weil Sie Daten korrigieren mussten. Dieser Verlust verblasst jedoch im Vergleich zu den Umsatzeinbußen, die durch das falsche Targeting verursacht wurden. Im Folgenden finden Sie die fünf wichtigsten Gründe, warum Sie in die Sauberhaltung Ihrer Daten investieren sollten: - Es kann eine bessere Segmentierung von Leads und Kunden vorgenommen werden, sodass Sie Ihre Botschaft zur richtigen Zeit auf die richtigen Personen konzentrieren können. Dieser 20%-Rabatt sollte nur an neue Interessenten gehen, nicht an Ihre besten Kunden, oder? - Doppelten Versand von E-Mails vermeiden. Trotz aller Vorsichtsmaßnahmen ist es möglich, mehrere E-Mails an denselben Lead zu senden, normalerweise weil doppelte Einträge nicht zusammengeführt werden. - Genaue Berichterstattung an Ihren Chef. Da der CMO an der Umsatztabelle mitwirkt, benötigen Sie genaue, konsistente und wiederholbare Berichte … andernfalls können Sie kein Umsatzmarketer mehr sein. - Schadet ausstehenden Angeboten, wenn bei Verkaufsverhandlungen falsches Marketing-Material an wichtige Interessenten gesendet wird.

Wenn Ihre Datenbank diese Details nicht in der Lebenszyklusphase bereitstellt, kann dies zu einer oder zwei Einbußen führen. - Entfernen Sie fehlerhafte oder alte Leads, um unter den Preisschwellen zu bleiben. Es ist viel über die Kosten schlechter Daten geschrieben worden. Ihre größte Sorge ist, dass zu viele fehlerhafte, doppelte oder alte Leads Sie über Ihre Marketo- oder Salesforce-Preisstufe treiben, was zu Tausenden von Gebühren pro Jahr führen kann. Wie also halten Sie Ihre Datenbank sauber? Sie können die folgenden Prinzipien der Datenbereinigung befolgen, aber wie kommen Sie in Marketo an? Ich gehe von einigen Annahmen über Ihr System aus: Sie haben ein standardmäßiges Marketo-System. - Sie haben eine typische Einrichtung für Lead-Kontakt-Konto-Salesforce. - Sie synchronisieren alle Datensätze zwischen Systemen. - Sie verwenden keine zielgerichteten Duplikate.

Finden Sie die richtigen Leads zur **. Zunächst finden wir die Leads, die potenzielle Probleme darstellen. Bei jedem Client gehe ich eine Reihe von Smart-Listen durch, um den Zustand seiner Datenbank zu ermitteln. Ich schlage vor, dass Sie das auch monatlich tun. Sobald die Smart-Listen funktionieren, dauert dies nicht mehr als 15 Minuten pro Monat. Auf diese Weise lässt sich der ROI Ihrer Arbeit und von Marketo hervorragend demonstrieren. Erstellen Sie eine Tabelle, um die Gesamtauswirkungen auf Ihre Datenbank zu verstehen. Das folgende Beispiel enthält die Kriterien, nach denen ich suche. Achten Sie daher darauf, andere Felder einzubeziehen, die für Ihr Unternehmen wichtig sind. Schlüsseln Sie die einzelnen Gruppen nach „Nur Marketo&quot;, &quot;SFDC Leads“ und &quot;SFDC-Kontakte“ auf.

Automatisierung zur Korrektur von Datenwerten verwenden: Die Verwendung von Automatisierungsregeln zur Korrektur gängiger Rechtschreibfehler oder fehlender Daten geht mehrere Jahrzehnte zurück. In den 1960er Jahren konnte eine schlechte Datenqualität beim Versand von Briefpost dazu führen, dass Tausende von Dollar verschwendet wurden. Heute ruinieren Fehler bei der Datenqualität den Ruf schneller als eine fehlgeleitete Post. E-Mail-Reputation, Sprachauswahl und Kundenerlebnis sind von großer Bedeutung, und sie sind von größerer Bedeutung, da Fehler in wenigen Minuten öffentlich bekannt gemacht werden können. Mit automatisierter Datenbereinigung können Sie die Reputation Ihrer Firma retten. Diese Datenverwaltungsflüsse gehören zu den ersten Dingen, die ich in Marketo einrichte. Die meisten Firmen richten ähnliche Flüsse ein: - Länderkorrektor (obwohl Sie Prinzip 1 hätten befolgen sollen, um dies nicht zu benötigen) - Landeskorrektor oder Mapper - oft hilfreich, wenn Sie Land und abgeleiteten Staat haben. - Anzahl der Mitarbeiter im Mitarbeiterbereich. - Fehlerhafte Lead-Source zu guter Lead-Source - E-Mail ungültig an E-Mail ist gut, wenn die E-Mail geändert wurde. : Übersetzen Sie alte Feldwerte in ein neues Feld (Full to Empty). Beispiel: Dieser Fluss passt den Mitarbeiterbereich basierend auf der Mitarbeiternummer an.

Das Ausfüllen leerer Felder mit Tools zum Anhängen von Daten ist für eine bessere Segmentierung Ihrer Datenbank von entscheidender Bedeutung. Leads geben nicht immer die richtige Branche oder Funktion oder sogar den richtigen Titel für ein Formular aus. Manchmal befinden sich ältere Daten aus einem älteren System. Dieser Datenmangel bedeutet, dass Sie diese E-Mail entweder an weniger oder möglicherweise an falsche Personen senden müssen. Um dies zu beheben, benötigen Sie Tools zum Anhängen von Daten.

Option 1: Füllen Sie es selbst aus. Möglicherweise können Sie Daten interpolieren, um leere Felder aufzustocken. Möglicherweise haben Sie einen SIC-Code anstelle des Branchennamens oder des „Jahresumsatzes vs. Jahresumsatzbereich“. Marketo kann diese Fehlerbehebungen einfach automatisieren.

Option 2: Finden Sie über LaunchPoint einen Anbieter für das Anhängen/Anreichern von Daten. Es [ mehrere Anbieter in Launchpoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) wie NetProspex und ReachForce, die Ihnen bei der Anreicherung Ihrer Lead-Daten helfen können. Einige bitten Sie um ein Blatt mit Ihren Daten, die sie dann bereinigen und zurücksenden. Die bessere Option ist ein automatisiertes Tool in Marketo oder Salesforce, das die gewünschten Felder prüft und dann die richtigen Daten zurückgibt. Die meisten Anbieter verwenden dazu die [Marketo-API oder ](/help/home.md)-Hooks.

Option 3: Verwenden von Marketo-APIs zum Aktualisieren von Leads Sie können die Marketo-APIs verwenden, um Leads zu identifizieren, die bereinigt werden müssen, und sie dann über die API aktualisieren. Die [Abrufen mehrerer Leads nach Filtertyp - REST-](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) ist ein guter Ausgangspunkt für das Abrufen von Daten aus Marketo, die bestimmten Kriterien entsprechen. Informationen zum Aktualisieren von Leads finden Sie in der [Erstellen/Aktualisieren von Leads-REST-API](/help/rest-api/leads.md).

Alternativ können Sie einen [Marketo-Webhook](/help/webhooks/webhooks.md) einrichten, um ein externes System über ein bestimmtes Ereignis zu informieren, z. B. über das Ausfüllen eines Formulars. Sie können dann mit Werten antworten, um den Lead zu aktualisieren.

Was sollte automatisiert werden? In Schritt 1 habe ich einige Vorschläge gemacht. Wenn wir jedoch einen größeren Teil der Datenbank bereinigen möchten, müssen wir über das Festlegen von Datenwerten hinausgehen. - Blacklisting für Konkurrenten: Stellen Sie sicher, dass Sie die Erfassung und das Blacklisting für Ihre Konkurrenten automatisieren. Wenn Sie Mitbewerber-Partner haben, ist es immer noch gut, sie richtig zu markieren und sie in eine Liste zu platzieren, um sie möglicherweise zu unterdrücken.  - Mehrere Hardbounces: Automatisieren Sie diesen Fluss auf jeden Fall. Wenn ein Lead innerhalb eines Zeitraums von 30 Tagen mehr als zweimal Hardbounces aufweist, setzen Sie ihn auf „Ausgesetzt“ oder „Ungültig“. Gehen Sie dann einmal im Monat vorbei, um zu überprüfen, ob das Problem ein Tippfehler oder etwas Anderes war.  - Mehrere Softbounces in 30 Tagen: Setzen Sie diese auf „Marketing ausgesetzt=TRUE“ für 30 Tage.  - Sperrung/Löschung von Spam-Fallen: Seien Sie vorsichtig, wenn Ihr Produkt bedeutet, dass Personen mögliche Spam-Fallen-Adressen verwenden. Siehe eine Liste von Spam-Fallen. Die Smart-Liste.

**Was soll nicht automatisiert werden** Automatisieren Sie niemals das Löschen von Leads, da das Risiko des versehentlichen Massenlöschens falscher Datensätze zu hoch ist. Stattdessen werden Leads einmal monatlich als Papierkorb markiert. Wenn Sie jedoch beschädigte Leads löschen möchten, führen Sie einen Fluss wie diesen mit einer Wartezeit von 30 Tagen aus.

Einschränkungen: Die Verwendung der Automatisierung ist eine hervorragende Möglichkeit, Zeit zu sparen und Ihre Datenbank sauber zu halten. Automatisierung ist ein zweischneidiges Schwert, denn wenn man es falsch einrichtet, kann es in wenigen Minuten Chaos verursachen. Seien Sie vorsichtig, während Sie diese Prozesse durchlaufen, und halten Sie alle auf dem Laufenden. Im Allgemeinen rate ich davon ab, SFDC-Kontakte zu löschen, da es sich mit höherer Wahrscheinlichkeit um aktive Opportunities, Kunden oder Ex-Kunden handelt. Von Finance oder Legal werden Sie möglicherweise aufgefordert, bestimmte Datensätze aufzubewahren, und diesen Datensätzen sind Kontakte beigefügt. Konzentrieren Sie sich stattdessen auf Kontakte, die Hardbounce verursachen, ungültig werden oder nicht mehr vorhanden sind. Jetzt kennen Sie ein paar Methoden, um Ihre Marketo-Datenbank sauber zu halten. Lassen Sie es uns wissen, wenn Sie andere Möglichkeiten gefunden haben, diese Prozesse mithilfe der API oder Webhooks zu automatisieren!

Veröffentlicht am _2014-10-08_ von _Josh_

## Updates vom Oktober 2014

### Vorbefüllen externer Seiten

Marketo Forms bietet keine native Vorbefüllungsfunktion, wenn es außerhalb einer Marketo-Landingpage geladen wird. Wir können dies jedoch weiterhin mit den [Marketo-APIs](/help/rest-api/rest-api.md) und der [Forms 2.0 JavaScript-API](/help/javascript-api/forms-api-reference.md/) implementieren. Der erste Schritt besteht darin, die Lead-Daten über einen REST-Aufruf von Ihrem Marketo-Server abzurufen. Wenn wir keine sofortige Möglichkeit haben, Lead-IDs oder eine andere eindeutige Kennung vom Server zu vergleichen, müssen wir das Munchkin-Cookie &#39;_mkto_trk&#39; verwenden, um Daten vom Marketo-Server abzurufen, indem wir die Methode [Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) verwenden.
Für diesen Aufruf benötigen wir Ihre Authentifizierungs- und REST-Endpunkte aus Ihrer -Instanz. Nachdem Sie sich bei Ihrer Marketo-Instanz authentifiziert haben, müssen wir die Leads-API unter `https://<host>/rest/v1/leads.json` aufrufen. Anschließend müssen wir eine Abfragezeichenfolge erstellen, um nach dem Marketo-Cookie wie `?filterType=cookie&filterValues=` zu filtern. Sie müssen den spezifischen Wert aus dem Schlüssel &#39;_mkto_trk&#39; abrufen, der vom Client an Ihren Server gesendet wurde. HINWEIS: Der Cookie-Wert _mkto_trk enthält ein kaufmännisches Und-Zeichen und muss als URL-codiert `%26`, damit er vom Marketo-Endpunkt ordnungsgemäß akzeptiert wird. Standardmäßig gibt die Leads-API vier Felder zurück: `id`, `email`, `firstName` und `updatedAt`. Um einen bestimmten Satz von Feldern festzulegen, müssen Sie einen `fields` Abfrageparameter mit Feldnamen einschließen, die durch Kommas wie diese getrennt sind: `&fields=email,firstName,lastName,company`. Am Ende wird unser Aufruf wie folgt aussehen:

`https://<host>/rest/v1/leads.json?filterType=cookie&filterValues=<cookie>&fields=email,firstName,lastName,company&access_token=<token>`

Wenn wir diesen Aufruf ausführen, wird ein JSON-Objekt zurückgegeben, das wie folgt aussieht:

```json
{
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
 "lastName":"Elkington",
        "email":"<mkto@example.com>",
 "company":"Marketo Inc."
        }]
};
```

Nachdem wir nun unsere Lead-Details haben, können wir diese mithilfe der Methoden whenReady und vals in JavaScript einem Marketo-Formular zuordnen. Zunächst müssen wir die Lead-Ergebnisse als Variable auf unserer Seite festlegen:

```javascript
<script>
//print your JSON object dynamically as the mktoLead variable
var mktoLead = {
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
  "lastName":"Elkington",
        "email":"mkto@example.com",
  "company":"Marketo Inc."
        }]
}
</script>
```

Nachdem wir nun unsere Ergebnisse auf der Seite haben, können wir sie unseren Formularfeldern zuordnen:

```javascript
<script>
MktoForms2.whenReady( function(form) {
 //set the first result as local variable
 var mktoLeadFields = mktoLead.result[0];
    //map your results from REST call to the corresponding field name on the form
 var prefillFields = {
   "Email" : mktoLeadFields.email,
   "FirstName" : mktoLeadFields.firstName,
   "LastName" : mktoLeadFields.lastName,
   "Company" : mktoLeadFields.company
   };
 //pass our prefillFields objects into the form.vals method to fill our fields
 form.vals(prefillFields);
 }
 );
</script>
```

Veröffentlicht am _2014-10-24_ von _Kenny_

## E-Mail-HTML ersetzen

In diesem Beitrag erfahren Sie, wie Sie die von Marketo generierte HTML für eine E-Mail entfernen können. Sie können dann Ihre eigene HTML verwenden, ohne sie von Marketo neu formatieren zu müssen.

1. Navigieren Sie zur E-Mail und klicken Sie auf Entwurf bearbeiten .
1. Klicken Sie unter E-Mail-Aktionen auf HTML-Tools und dann auf HTML ersetzen.
1. Ersetzen Sie den Code in diesem Feld durch Ihren HTML. Klicken Sie dann auf Speichern.

Veröffentlicht am _2014-10-28_ von _Murta_

## Abrufen der Cookie-ID eines Besuchers und anschließende Abfrage der zugehörigen Lead-Daten

Mithilfe des REST[Endpunkts „Mehrere Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) können Sie Lead-Daten basierend auf der Cookie-ID eines Benutzers abfragen. Beispielsweise können Sie diesen Ansatz zum Vorausfüllen eines Formulars auf einer Landingpage verwenden, die nicht von Marketo stammt. Dieser Beitrag zeigt Ihnen, wie Sie den Cookie-Wert des Benutzers während eines Web-Seitenbesuchs erfassen, [mehrere Leads abrufen REST-API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) mit dieser Cookie-ID abfragen und dann die Lead-Daten des Benutzers zurückgeben. Zunächst benötigen wir den Wert des Munchkin-Cookies &#39;_mkto_trk&#39; des Benutzers. Im Folgenden finden Sie ein Beispiel für eine JavaScript-Funktion, mit der Sie den Cookie-Wert abrufen können. Weitere Informationen [ diesem Ansatz finden Sie ](https://stackoverflow.com/questions/10730362/get-cookie-by-name) dieser StackOverflow-Seite. Es wird empfohlen, nach dem Seitenladeereignis eine Verzögerung von 500 ms festzulegen, bevor diese Funktion aufgerufen wird. Dadurch hat Munchkin Zeit zum Laden und Cookie für den Benutzer.

```javascript
//Function to read value of a cookie
function readCookie(name) {
    var cookiename = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(cookiename) == 0) return c.substring(cookiename.length,c.length);
    }
    return null;
}

//Call readCookie function to get value of user's Marketo cookie
var value = readCookie('_mkto_trk');
```

Übergeben Sie als Nächstes den Wert des Cookies &#39;_mkto_trk&#39; an Ihren Server. Um Lead-Daten von Ihrem Server abzurufen, rufen Sie die REST-API [ mehrere Leads ab ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) verwenden diesen Cookie-Wert. Sie benötigen Ihre Authentifizierungs- und REST-Endpunkte von Ihrer -Instanz. Ihr -Aufruf sollte wie folgt strukturiert sein:

HINWEIS: Der `_mkto_trk`-Cookie-Wert enthält ein kaufmännisches Und-Zeichen und muss URL-kodiert sein, damit er `%26` ordnungsgemäß vom Marketo-Endpunkt akzeptiert wird.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "cde42eff-aca0-48cf-a1ac-576ffec65a84:ab"
# Replace with filter type and values
filter_type_and_values = "&filterType=cookies&filterValues=id:AAA-BBB-CCC%26token:_mch-marketo.com-1418418733122-51548&fields=cookies,email"
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

Im obigen Beispiel werden die E-Mail und alle mit dem Benutzer verknüpften Cookies zurückgegeben. Anschließend können Sie diese Daten verwenden, um die nachfolgende Seite, die der Benutzer besucht, zu personalisieren.

`{"requestId":"aa00#14a405aa786","result":[{"id":583,"email":"<testaccount@gmail.com>","cookies":"_mch-marketo.com-1418418733122-51548"}],"success":true}`

Veröffentlicht am _2014-10-30_ von _Murta_

## Wann benötigen Sie einen Entwickler, der Ihnen bei der Marketing-Automatisierung hilft?

HINWEIS: Dies ist ein Gast-Blogpost. Josh Hill ist Marketo Practice Lead bei Perkuto, einer Agentur für Marketing-Automatisierung. Josh arbeitet an der Schnittstelle von Marketing, Vertrieb und Technologie, um umsatzgenerierende Systeme bereitzustellen. Er schreibt über Marketing-Automatisierung und Nachfragegenerierung unter [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).

Marketing-Automatisierungsplattformen sind unglaublich leistungsstark, sowohl vorkonfiguriert als auch in den Händen eines erfahrenen Benutzers. Plattformen ermöglichen definitionsgemäß die Verwendung von Erweiterungsanwendungen, damit das System noch erstaunlichere Dinge für Ihr Team erledigen kann. Sie denken vielleicht, dass Marketos Logikmaschine so viel kann (und das ist sie), aber es gibt Einschränkungen. Marketo kann nicht alles für Sie tun und sollte es auch nicht.

Es gibt andere Tools, die ihre Funktion besser erfüllen, als Marketo sie erstellen könnte. Die Marketo-Plattform ist sehr offen, sodass das [LaunchPoint-Ökosystem von Programmen](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) existieren kann. Sie können diese Offenheit auch nutzen, um die Funktionen Ihrer Site und von Marketo entsprechend Ihren Geschäftsanforderungen zu erweitern. Das Tolle an einer Plattform wie Marketo ist, dass sie es typischen Marketern ermöglicht, Seiten, E-Mails und Routing-Logik zu erstellen, ohne ein vollwertiger Programmierer zu sein.
Ein Marketer muss heutzutage die Logik verstehen, aber die eigentliche Programmierung sollte am besten den Experten überlassen werden. Woher wissen Sie also, wann Sie einen Entwickler bzw. eine Entwicklerin aufrufen müssen? Ich habe ein paar Grundregeln oder Heuristiken, um zu entscheiden, wann ein Programmierer beteiligt werden sollte: - Wenn Marketo keinen offensichtlichen Filter, Trigger oder eine Funktion für die Notwendigkeit hat, kann dies oft mit JavaScript oder jQuery getan werden. - Wird dies für Marketo allein zu kompliziert? - Kann Marketo das überhaupt? - Ist diese Anpassung einer Website nicht einfach zu unterstützen? - Muss Marketo mit einer Website oder einer anderen Datenbank kommunizieren? - Klingt es nach etwas, das ein Computer tun kann, aber Marketo hat dafür keine Funktion? Marketo bietet zwar keine vordefinierte Funktion, stellt jedoch eine Verbindung zu vielen Drittanbieterintegrationen sowie zu benutzerdefinierten Verbindungen her.

Sehen Sie sich einige dieser Kategorien auf dem [LaunchPoint Marketplace](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) an: - [Analytics-Tools](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Datenanhängen](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Content-Management-Systeme](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Einige Drittanbieteranwendungen bieten intuitive Bedienfelder und Einrichtungstools direkt auf der Plattform (GoToWebinar). Hierbei handelt es sich um „native“ Integrationen, bei denen die meiste Arbeit, die Sie tun müssen, darin besteht, die Anmeldung einzurichten und sie dann in Marketo zu verwenden. Andere Erweiterungen erfordern jedoch die Verwendung der komplexeren API, die direkter programmiert werden muss.

**Marketos Integrationsoptionen** - LaunchPoint-Integration - normalerweise eine Anmeldung oder einfache Einstellungen. - API-Integration: erfordert die Einrichtung der API und die Programmierung: (1) [REST-API](/help/rest-api/rest-api.md) (2) [SOAP-API](/help/soap-api/soap-api.md) (3) [Webhook-Integration](/help/webhooks/webhooks.md) - erfordert die Einrichtung von speziellem Code, ist jedoch relativ einfach. (4) [E-Mail-Skripterstellung](./email-scripting.md) (Velocity) - JavaScript und jQuery: (1) [Forms 2.0](/help/javascript-api/forms-api-reference.md) (2) [Lead-Tracking (Munchkin)](/help/javascript-api/lead-tracking.md) (3) [RTP JS](/help/javascript-api/web-personalization.md) Im Folgenden finden Sie einige Anwendungsfälle für die Verwendung eines Entwicklers zur Erweiterung der Funktionen der Marketo-Plattform. Haben Sie einen dieser Anwendungsfälle? Wenn ja, könnte es Zeit sein, mit einem Entwickler zu sprechen. [Besuchen Sie den Abschnitt „Service-Partner“ auf LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO).

Veröffentlicht am _2014-11-06_ von _Josh_

## Integrieren von Slack mit Marketo

[Slack ist eine Enterprise Collaboration-Plattform](https://slack.com/). Wenn sich Ihr Team in Slack befindet, können Sie ganz einfach Marketo-Benachrichtigungen in Ihren Workflow integrieren. Dieser Beitrag zeigt Ihnen, wie Sie Ihrem Chat-Protokoll eine Benachrichtigung hinzufügen können, wenn eine bestimmte Lead-Aktivität in Marketo stattfindet. Zu den möglichen Anwendungsfällen gehören die Benachrichtigung des gesamten Teams über ein ausgefülltes Formular, der Besuch einer Preisseite oder ein Lead, der innerhalb von 30 Tagen nicht kontaktiert wurde. Der folgende Screenshot zeigt, wie die Marketo-Benachrichtigung in Slack nach Befolgung der Schritte in diesem Hilfeartikel aussieht.

1. Melden Sie sich bei Slack an. Klicken Sie auf Integrationen in Slack
1. Klicken Sie auf die Schaltfläche Hinzufügen für eingehende Webhooks.
1. Wählen Sie den Kanal für die Marketo-Benachrichtigung aus. Klicken Sie dann auf Eingehende Webhook-Integration hinzufügen .
1. Webhook-URL kopieren und einfügen (was für Schritt 8 erforderlich ist)
1. Namen für die Benachrichtigung auswählen
1. Melden Sie sich bei Marketo an. Zu Admin gehen. Klicken Sie auf Webhooks.
1. Klicken Sie auf Neuer Webhook
1. Geben Sie einen Namen für den Webhook ein. Geben Sie die Webhook-URL aus Schritt 4 ein. Geben Sie als Anforderungstyp „POST“ ein. Payload-Vorlage eingeben.

Im Folgenden finden Sie die Payload-Vorlage aus dem Screenshot. Er verwendet Token für den Vornamen, das Unternehmen und die E-Mail-Adresse auf Lead-Ebene.

`payload={"text": "DEVELOPER SITE ALERT: {{lead.First Name:default=edit me}} {{lead.Company=edit me}}, {{lead.Email Address:default=no email address}}" }`

1. Einrichten von Trigger Campaign in Marketo. Der Flussschritt besteht darin, Webhook an Slack aufzurufen. Smart List ist ein Web-Seitenbesuch.
1. Überprüfen Sie, ob es funktioniert.

Marketo Weitere Informationen zu Webhooks in [ finden ](./webhooks/webhooks.md) in der Entwicklerdokumentation.

Veröffentlicht am _2014-11-10_ von _Murta_

## Integrieren von Litmus mit Marketo

[Litmus ist ein Service](https://www.litmus.com/) zum Testen von E-Mails über Browser und E-Mail-Clients hinweg. Litmus bietet auch Analysen rund um E-Mails, einschließlich Klicks, Öffnungen und Löschungen. Dieser Beitrag zeigt Ihnen, wie Sie Marketo mit Litmus integrieren.

1. Klicken Sie beim Einrichten Ihres E-Mail-Programms in Marketo im Programm-Dashboard auf „Meine Token“
1. Ziehen Sie das Token „E-Mail-Skript“ in den mittleren Bereich, um es hinzuzufügen.
1. Benennen Sie das Token und klicken Sie auf „Zum Bearbeiten klicken“
1. Erweitern Sie rechts unter „Standardobjekte“ die Kategorie „Lead“. Suchen Sie das Feld „E-Mail-Adresse“ und aktivieren Sie das Kontrollkästchen. Fügen Sie im leeren Skriptbereich links auf derselben Seite den von Litmus bereitgestellten Trackingcode ein. Ersetzen Sie im Litmus-Code jede Instanz von `{{lead.Email Address}}` durch `${lead.Email}`. Klicken Sie auf Speichern , um das Lightbox-Fenster zu schließen, und klicken Sie erneut auf der Seite „Token“ auf „Speichern“.
1. Notieren Sie sich den Namen des Token-`{{my.LitmusToken}}`. Öffnen Sie die E-Mail, die Sie verfolgen möchten. Platzieren Sie Ihr neues Skript-Token ganz unten in Ihrer E-Mail. Sie können auch Standardinformationen hinzufügen, die mit der Litmus-Version `{{my.LitmusToken:default=editme}}` übereinstimmen.

Wenn die E-Mail gesendet wird, wird das Skript von Marketo in die E-Mail eingefügt.

Veröffentlicht am _2014-11-18_ von _Murta_

## Bild angeben, wenn eine Marketo-Landingpage in Facebook geteilt wird

Angenommen, Sie möchten, dass ein Bild automatisch angezeigt wird, wenn Sie eine Marketo-Landingpage auf Facebook freigeben. Vielleicht möchten Sie auch, dass dieses Bild nicht auf der Marketo-Landingpage selbst ist, sondern nur auf der Facebook-Freigabe. Sie können dies tun, indem Sie Ihrer Marketo-Landingpage ein Open-Graph-Meta-Tag hinzufügen. Gehen Sie dazu wie folgt vor.

1. Landingpage auswählen. Klicken Sie dann auf Entwurf bearbeiten .
1. Klicken Sie auf Seiten-Meta-Tags bearbeiten .
1. Fügen Sie Open-Graph-Meta zum Abschnitt Facebook-OG-Tags hinzu. Klicken Sie dann auf Speichern. Im Folgenden finden Sie das Format: `<meta property="og:image" content="http://example.com/example.jpg"/>`

[ Informationen zu Open-Graph-Meta](https://developers.facebook.com/docs/sharing/best-practices)Tags finden Sie in der Entwicklerdokumentation von Facebook .

Veröffentlicht am _2014-11-17_ von _Murta_

## Weiterleitungsseite basierend auf Referrer

Angenommen, Sie möchten den direkten Traffic zu einer Marketo-Landingpage verhindern. Stellen Sie sich vor, diese Seite enthält herunterladbare Inhalte wie eine PDF, auf der ein Benutzer vor dem Empfang ein Formular ausfüllen soll. Sie können dies beheben, indem Sie überprüfen, ob der Benutzer von einer bestimmten Seite kommt. In diesem Fall wäre es die Seite, auf der der Benutzer ein Formular ausfüllen muss. Wenn der/die Benutzende nicht von dieser Seite kommt, können Sie den/die Benutzende(n) zur Seite zum Ausfüllen des Formulars weiterleiten. Dazu müssen Sie überprüfen, ob die Referrer-Seite zur Landingpage mit Inhalten die Formular-Ausfüllseite ist.
Ersetzen Sie beide Instanzen von `http://example.com/PageWithForm` im folgenden Ausschnitt durch einen Link zu der Seite, von der die Benutzerin oder der Benutzer kommen soll. Dies könnte die Seite zum Ausfüllen des Formulars sein.**

```javascript
<script>
window.onload = function() {
  if (document.referrer !== "http://example.com/PageWithForm") {
    document.location.href = "http://example.com/PageWithForm";
  };
 };
</script>
```

Fügen Sie das angepasste JavaScript-Snippet vor dem schließenden -Tag mit dem Inhalt Ihrer Marketo-Landingpage ein.** Wenn der Benutzer nicht mit Inhalten von der Seite zum Ausfüllen des Formulars zur Landingpage weitergeleitet wird, wird er jetzt zur Seite zum Ausfüllen des Formulars weitergeleitet.

Veröffentlicht am _2014-11-18_ von _Murta_

## Integrieren von Trello mit Marketo

Trello ist eine [beliebte Web-basierte Projekt-Management-Anwendung](https://trello.com/). Wenn Ihr Team Trello verwendet, können Sie Marketo-Benachrichtigungen einfach in Ihren Workflow integrieren. Dieser Beitrag zeigt Ihnen, wie Sie Ihrem Trello-Board eine Karte mit einer Marketo-Benachrichtigung hinzufügen. Diese Karte wird hinzugefügt, wenn in Marketo eine bestimmte Lead-Aktivität stattfindet. Zu den möglichen Anwendungsfällen gehören die Benachrichtigung des gesamten Teams über ein ausgefülltes Formular, der Besuch einer Preisseite oder ein Lead, der innerhalb von 30 Tagen nicht kontaktiert wurde.

1. Anmelden bei Trello. Navigieren Sie zum Trello-Board, auf dem Marketo-Benachrichtigungen hinzugefügt werden sollen. Klicken Sie auf Liste hinzufügen und benennen Sie sie.
1. Klicken Sie auf Seitenleiste anzeigen . Klicken Sie auf E-Mail-zu-Pinnwand-Einstellungen. Notieren Sie die E-Mail in dem Feld „Ihre E-Mail-Adresse für dieses Board“. Diese E-Mail wird in Schritt 6 verwendet. Wählen Sie aus, welche Liste der Marketo-Benachrichtigung hinzugefügt werden soll.
1. Melden Sie sich bei Marketo an. Klicken Sie auf die neue Smart-Kampagne. Geben Sie einen Namen ein, und klicken Sie dann auf Speichern.
1. Navigieren Sie zur Smart-Liste. Wählen Sie einen Trigger für diese intelligente Kampagne. In diesem Beispiel verwenden wir den Trigger zum Ausfüllen von Formularen. Ausgefüllten Formular-Trigger in das mittlere Bedienfeld ziehen. Wählen Sie das Formular aus, in dem diese Benachrichtigung Trigger erhalten soll.
1. Erstellen einer E-Mail. Klicken Sie auf Neu. Klicken Sie auf Lokales Asset. Klicken Sie auf Neue E-Mail. Benennen Sie die E-Mail. Klicken Sie dann auf Erstellen.
1. Klicken Sie auf „Entwurf bearbeiten“ für die E-Mail, die Sie in Schritt 5 erstellt haben. Ziehen Sie die entsprechenden Token, die Sie in Ihrer Trello-Karte anzeigen möchten. Die Betreffzeile der Marketo-E-Mail erscheint im Titel der Trello-Karte, und der Hauptteil der Marketo-E-Mail wird in der Beschreibung der Trello-Karte angezeigt. Beispielsweise können Sie „Lead-Warnhinweis: `{{lead.First Name:default=edit me}}` `{{lead.Last Name:default=edit me}}` verwenden, wenn der Vor- und Nachname des Leads im Titel der Trello-Karte enthalten sein soll. Genehmigen Sie dann die E-Mail.
1. Navigieren Sie zu Smart Campaign. Klicken Sie auf Fluss . Ziehen Sie Warnhinweis senden in das mittlere Bedienfeld. Wählen Sie die soeben erstellte E-Mail aus. Wählen Sie „Senden an“ als Keine aus. Wählen Sie „Zu anderen E-Mails“ als Trello-E-Mail aus Schritt zwei.
1. Klicken Sie im oberen Menü auf Zeitplan . Klicken Sie auf Aktivieren. Klicken Sie auf Bestätigen .
1. Testen Sie die Integration. Eine Karte mit dem Vor- und Nachnamen des Leads im Titel wird im Trello-Board angezeigt. Weitere Informationen finden [ in der ](https://support.atlassian.com/trello/) von Trello.

Veröffentlicht am _2014-11-18_ von _Murta_

## Suchen von Leads nach benutzerdefiniertem Feldwert

Angenommen, Sie möchten Leads von der Marketo-API erhalten, die bestimmten Aktivitäts- oder Inaktivitätskriterien entsprechen. Vielleicht möchten Sie zum Beispiel einen Lead finden, dessen Bewertung sich in den letzten 30 Tagen nicht geändert hat. Wenn Sie die Schritte in diesem Beitrag befolgen, können Sie diese Liste der Leads erhalten. Zu diesem Zweck erstellen wir in Marketo eine intelligente Kampagne, die Leads identifiziert, deren Score in den letzten 30 Tagen nicht geändert wurde, und speichern dann einen Wert auf diesen Leads, um sie zu identifizieren. Anschließend fragen wir die API mit diesem Wert ab.

1. Erstellen Sie ein neues benutzerdefiniertes Feld mit dem Namen customLeadStatus** Melden Sie sich bei Marketo an und wechseln Sie zum Admin-Bedienfeld. Klicken Sie auf Feldverwaltung . Klicken Sie auf Neues benutzerdefiniertes Feld .  Benennen Sie das Feld. Klicken Sie dann auf Erstellen.
1. Erstellen Sie eine Smart-Kampagne mit einer Smart-Liste, die nach Leads sucht, die in 30 Tagen nicht aktualisiert wurden.** klicken Sie auf Neue intelligente Kampagne. Benennen Sie die neue intelligente Kampagne. Nicht-Punktzahl ziehen wurde vom rechten zum mittleren Bereich geändert.
1. Fügen Sie der Smart-Kampagne aus Schritt 3 einen Flussschritt hinzu, um das Feld customLeadStatus mit einem neuen Wert zu aktualisieren.** Ziehen Sie „Datenwert ändern“ aus dem rechten Bedienfeld in den mittleren Bereich.
1. Aktualisieren Sie Smart Campaign, damit Leads mehrmals durchlaufen können.** Klicken Sie auf Zeitplan. Klicken Sie dann auf Bearbeiten.  Jedes Mal auswählen. Klicken Sie dann auf Speichern. Die Kampagne startet jetzt.
1. Abfrage der [Abrufen mehrerer Leads nach Filtertyp REST-API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Bereitstellung der Parameter filterType=customLeadStatus &amp; filterValue=needsEnrichment.**

Dies ist eine Beispielanfrage, die diese Daten zurückgibt.

`<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?access_token=><yourAccessToken>&filterType=customLeadStatus&filterValues=needsEnrichment`

Ein erfolgreicher API-Aufruf gibt JSON-Daten mit Leads zurück, deren customLeadStatus-Feld mit dem Wert von needsEnrichment übereinstimmt. Weitere Informationen finden Sie [ Abschnitt Abrufen mehrerer Leads nach Filtertyp ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) REST API .

Veröffentlicht am _2014-11-22_ von _Murta_

## Opportunity-Synchronisation über SOAP API

Dieser Beitrag beschreibt, wie Opportunities über die SOAP-API in Marketo eingefügt und mit Unternehmen und Leads verknüpft werden können. Es beginnt mit einer Erläuterung der Funktionsweise dieses Prozesses und bietet dann Code-Beispiele für jedes der Szenarien.

**Tabellenstrukturdiagramm** Zunächst wird im folgenden Diagramm die Tabellenstruktur beschrieben. Die ID wird bei der Erstellung eines Datensatzes (sequenzielle Nummer) automatisch generiert. Die fett gedruckten Felder sind erforderlich: Nur die Rolle „Opportunity-Person“ verfügt über erforderliche Felder. Die Felder in Klammern sind optional, ebenso die Verbindungen mit einer gepunkteten Linie.

**Einfache Opportunity-**: Zunächst fügen Sie die Opportunity ein und dann die Rolle Opportunity-Person, die die Opportunity mit dem/den Lead(s) verknüpft. Verwenden Sie für dieses Beispiel die Lead-ID und die Opportunity-ID, um die Rolle der Opportunity-Person anzugeben. Sie erhalten die Opportunity-ID in der SOAP-Antwort, wenn Sie die Opportunity erstellen. Die Lead-ID ist auf jedem Lead in der Marketo-Lead-Datenbank sichtbar.

**Firmenlink** In den meisten Fällen möchten Sie eine Opportunity zusätzlich zu einer Person mit einer Firma verknüpfen. In Marketo können Sie nicht über die SOAP-API auf die Unternehmensdatensätze zugreifen, sondern nur auf die Lead-Datensätze (die Lead-Datensätze enthalten die Unternehmensfelder). Es ist weiterhin möglich, Opportunities mit einem Unternehmen zu verknüpfen, indem zu jedem Lead eine eindeutige Unternehmenskennung hinzugefügt und diese ID in der Opportunity verwendet wird. Schritt 1 besteht darin, im Lead-Datensatz ein Feld „Firmen-ID“ zu erstellen und mit einer eindeutigen Kennung zu füllen, normalerweise aus einem Back-End-System. Schritt 2 besteht darin, bei der Opportunity ein Feld „Unternehmens-ID“ zu erstellen: Sie müssen den Marketo Support oder Consulting bitten, ein solches Feld für Sie zu erstellen. Füllen Sie dann dieses Feld beim Erstellen von Opportunities aus, wodurch die Opportunity mit dem Unternehmen verbunden wird. Dies ist besonders wichtig, wenn Sie Marketo Revenue Cycle Analytics verwenden und den Opportunity Influence Analyzer verwenden möchten, der von Unternehmensinformationen abhängig ist.

**Verwendung externer Kennungen** In vielen Fällen können Sie bei der Integration in ein Backend-System über eigene eindeutige Kennungen verfügen. Es ist möglich, diese eindeutigen Kennungen in Marketo über Fremdschlüssel zu verwenden. Für Leads würden Sie normalerweise die Foreign System Person ID (FSPID) verwenden, die anstelle der Marketo-ID oder E-Mail-Adresse als eindeutige Kennung verwendet wird. Die FSPID ist ein ausgeblendetes Systemfeld, das innerhalb von Marketo nicht sichtbar ist. Wenn Sie dies noch nicht tun, ist es für Opportunity Sync erforderlich, die FSPID auch in einem benutzerdefinierten Feld zu speichern, z. B. „Foreign ID“ (Sie können das Feld beliebig benennen). Sie können dieses Feld selbst als Marketo-Administrator erstellen. Für Opportunitys muss der Marketo-Support ein weiteres benutzerdefiniertes Feld für Opportunity erstellen, z. B. „Foreign ID“ (Sie können es beliebig benennen). Füllen Sie dann dieses Feld beim Einfügen von Opportunities aus. Wenn Sie schließlich die Rolle Opportunity-Person erstellen, verwenden Sie beide Fremdschlüssel, um die Relation zwischen Leads und Opportunities anzugeben, anstatt die Marketo-IDs zu verwenden. Sie können auch die Fremdschlüssel verwenden, um Leads von Opportunities zu aktualisieren. Derzeit ist es nicht möglich, den Opportunity-Personenrollendatensätzen einen Fremdschlüssel hinzuzufügen, sodass Sie dafür die automatisch generierte Marketo-ID verwenden müssen (die Sie nach dem Erstellen der Opportunity-Personenrolle in der SOAP-Antwort erhalten).

**Code-Beispiel** Schritte: 1. Lead mit Fremdschlüssel und Unternehmens-ID 1 einfügen/aktualisieren. Gelegenheit mit Fremdschlüssel 1 einfügen. Funktion „Opportunity-Person“ mit Fremdschlüsseln 1 einfügen. SOAP-Anfrage - Aktualisieren eines bestehenden Leads mit Fremdschlüssel und Unternehmens-ID Damit wird ein bestehender Lead (Marketo-ID „6„) mit dem Fremdschlüssel „12346“ und der Konto-/Unternehmens-ID „C123“ aktualisiert. Wir speichern den Fremdschlüssel auch in einem benutzerdefinierten Feld, da wir ihn für die Rolle der Opportunity-Person benötigen. Die Verwendung eines Fremdschlüssels ist optional: Sie können auch die Marketo-ID verwenden, um diesen Lead mit der Opportunity zu verknüpfen. Die Verwendung der Unternehmens-ID ist ebenfalls optional, aber sie ist erforderlich, wenn Sie den Opportunity Influence Analyzer in RCA verwenden möchten. Anfrage:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncLead>
         <leadRecord>
            <Id>6</Id>
            <ForeignSysPersonId>12346</ForeignSysPersonId>
            <leadAttributeList>
               <attribute>
                  <attrName>FSPID</attrName>
                  <attrValue>12346</attrValue>
               </attribute>
               <attribute>
                  <attrName>cAccountFSID</attrName>
                  <attrValue>C123</attrValue>
               </attribute>
            </leadAttributeList>
         </leadRecord>
         <returnLead>false</returnLead>
      </mkt:paramsSyncLead>
   </soapenv:Body>
</soapenv:Envelope>
```

Antwort:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncLead>
         <result>
            <leadId>6</leadId>
            <syncStatus>
               <leadId>6</leadId>
               <status>UPDATED</status>
               <error xsi:nil="true"/>
            </syncStatus>
            <leadRecord xsi:nil="true"/>
         </result>
      </ns1:successSyncLead>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-Anfrage - Opportunity-Erstellung In diesem Fall wurden zwei benutzerdefinierte Felder in der Opportunity-Tabelle erstellt: - `opportunityId` → enthält die eindeutige Opportunity-ID - `cAccountFSID` → enthält die Unternehmensreferenz, anstatt Ihre eigene Opportunity-ID anzugeben. Sie können auch die von Marketo generierte Opportunity-ID verwenden. In diesem Fall lassen Sie den Knoten Externer Schlüssel aus. Die Unternehmensverknüpfung ist ebenfalls optional, aber erforderlich, wenn Sie den Opportunity Influence Analyzer in RCA verwenden möchten. Anfrage:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:03:28-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_4</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 4 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>501.00</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>501</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Antwort:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>40</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-Anfrage - Rolle der Opportunity-Person Diese Anfrage verknüpft den Lead mit der Opportunity. Sie können mehrere Links in einer einzigen SOAP-Anfrage angeben (in diesem Beispiel wird die Opportunity nur mit einem Lead verknüpft). Dabei werden die Fremdschlüssel zur Angabe des Links verwendet, aber in den Kommentaren wird auch gezeigt, wie die tatsächlichen IDs verwendet werden (in diesem Fall: 6 für die Lead-ID und 40 für die Opportunity-ID). Die Felder „IsPrimary“ und „Role“ sind optional. Anfrage:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <!--id>40</id-->
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_4</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Antwort:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>11</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Alternativer Ansatz (Führen Sie Schritt 2 und 3 in einem Aufruf aus)** Während Sie zunächst die Opportunity und dann die Rolle des Opportunity-Benutzers einfügen können, ist es auch möglich, dies in einem SOAP-Aufruf zu tun. Sie müssen jedoch den Fremdschlüssel für die Opportunity verwenden (Sie können die automatisch generierte Opportunity-ID nicht in der Rolle der Opportunity-Person verwenden, da die Opportunity noch nicht generiert wurde). Natürlich können Sie in demselben API-Aufruf auch mehrere Leads mit dieser Opportunity verknüpfen (in diesem Beispiel wird die Opportunity nur mit einem Lead verknüpft). Anfrage:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:44:08-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_5</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 5 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
             <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_5</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Antwort:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>41</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
               <mObjStatus>
                  <id>12</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Veröffentlicht am _26.11.2014_ von _Jep_

## Multithread-REST-API-Anfragen

Wenn Sie die Leistung beim Aufrufen der Marketo-API verbessern möchten, können Sie gleichzeitige Anfragen stellen. Mit diesem Ansatz können Sie mehr Daten in einem kürzeren Zeitraum erhalten. Wenn Sie eine API-Anfrage stellen, ist ein Teil der Roundtrip-Zeit zwischen dem Client und dem Server die Übertragungszeit auf dem Kabel. Wenn wir also die Übertragungszeit auf der Leitung für die Anforderungen insgesamt reduzieren können, verbessern wir die Leistung. Der folgende Beispiel-Code zeigt, wie Sie dies in Ruby tun können. Sie verwendet EventMachine, eine [Ereignisverarbeitungsbibliothek für Multithreadanforderungen](https://github.com/igrigorik/em-http-request/wiki/Parallel-Requests). Im folgenden Beispiel wird die [Lead-Aktivitäts-API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) aufgerufen, wobei zwei gleichzeitige Anfragen gestellt werden. Dadurch entfällt die Übertragungszeit vom Client zum Server für die zweite Anforderung. Dies erfolgt, indem die zweite Anfrage gleichzeitig mit der ersten Anfrage eingeschlossen wird. Die API-Antworten werden in eine Textdatei geschrieben.

```java
require 'em-http-request'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = ["&nextPageToken=A5YMOYZQBOGD2OSYYBYDAQGEMGLBDGDANAABQGRAQWAAKKID", "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"]
# Specify activities needed
activity_type_ids = "&activityTypeIds=1&activityTypeIds=12"
requesturl_a = marketo_instance + endpoint + auth_token + since_date_time.at(0) + activity_type_ids
requesturl_b = marketo_instance + endpoint + auth_token + since_date_time.at(1) + activity_type_ids

# Make request
EventMachine.run do
  http1 = EventMachine::HttpRequest.new(requesturl_a).get
  http2 = EventMachine::HttpRequest.new(requesturl_b).get

# When API response is received, write response to a text file
  http1.callback {
  File.open('response1.txt', 'w') do |t|
  t.puts http1.response
  end }

  http2.callback {
  File.open('response2.txt', 'w') do |t|
  t.puts http2.response
  end }
end
```

Veröffentlicht am _2014-12-03_ von _Murta_

## Performance Tuning-API-Anfragen

In diesem Beitrag werden Strategien zur Verbesserung der Leistung beim Anfordern von Daten von der Marketo-API erläutert. Sie müssen jedoch die Vorteile dieser Strategien gegen die Betriebsbeschränkungen der täglichen Beschränkungen der Marketo-API abwägen.
**Strategie 1: Weniger Daten bei jedem API-Aufruf anfordern** Im Allgemeinen nimmt die Zeit, die für das Suchen der Daten in der Datenbank durch den Marketo-Server benötigt wird, zu, wenn Sie mehr Daten in einem API-Aufruf anfordern. Wenn Sie einen API-Aufruf mit Datumsbereichen durchführen, wie etwa die [getMultipleLeads-SOAP-API](/help/soap-api/getmultipleleads.md), verkürzen Sie den Zeitbereich pro Aufruf und kompensieren Sie dies mit weiteren Aufrufen. Anstatt beispielsweise Daten vom 1. Juni bis zum 1. Juli anzufordern, fordern Sie jeweils einen einzelnen Tag an, z. B. einen Aufruf für den 1. bis 2. Juni und dann einen weiteren Aufruf für den 2. bis 1. Juni. Wenn Sie einen API-Aufruf ausführen, der Daten aus Marketo-Lead-Feldern zurückgibt, fordern Sie nur diese Felder an, die erforderlich sind. Jedes zusätzliche Lead-Feld erhöht inkrementell die Zeit, die ein API-Aufruf benötigt. Ein weiterer Ansatz besteht darin, die Batch-Größe oder die Anzahl der pro Aufruf angeforderten Leads zu reduzieren.
**Strategie 2: Gleichzeitige Anfragen** Um die Leistung zu verbessern und mehr Daten gleichzeitig abzurufen. Sie können gleichzeitige Anfragen an die -API richten. Durch diesen Ansatz wird die Zeit reduziert, die insgesamt für Wire-API-Anfragen aufgewendet wird. Angenommen, Sie stellen Anfragen an den Filtertyp Mehrere Leads abrufen . Sie können gleichzeitig Anfragen für eine Anfrage stellen, die Leads 1 bis 300 abfragt, und für eine andere Anfrage, die Leads 301 bis 600 abfragt.
**Strategie 3 - Daten zwischenspeichern** Einige Daten in Marketo werden seltener geändert, wie z. B. die Liste der Lead-Felder, als andere Daten, z. B. Lead-Aktivitätsdaten. Wenn Sie Daten zwischenspeichern, die weniger häufig aktualisiert werden, verringern Sie die Anzahl der API-Aufrufe, die Sie ausführen müssen. Sie erhalten auch eine bessere Leistung, da die lokale Suche nach den Daten im Allgemeinen schneller ist als der Zugriff über einen Remote-Webservice.

Veröffentlicht am _2014-12-05_ von _Murta_

## Senden von Marketo-Formulardaten an Google Analytics

In Google Analytics können Sie benutzerdefinierte Datenereignisse senden und dann die Daten verwenden, um Ihre Website-Leistung zu segmentieren und zu analysieren. Mit dem folgenden JavaScript-Code-Snippet können Sie automatisch Marketo 2.0-Formulardaten an Google Analytics senden, nachdem eine Besucherin oder ein Besucher ein Web-Formular gesendet hat. Gehen Sie wie folgt vor, um dies einzurichten.

**Schritt 1** Fügen Sie das JavaScript-Tag auf jeder Seite ein, die Marketo Forms enthält, am unteren Rand des Codes (vor dem -Tag). Der JavaScript sendet nur nicht ausgeblendete Felder (sendHiddenFields : false). Dies kann angepasst werden, indem sendHiddenFields von „false“ auf „true“ geändert wird. Sie können auch Felder auswählen, die ausgeschlossen werden sollen, indem Sie dem Array „fieldsToExclude“ zusätzliche Feld-IDs hinzufügen.

```javascript
function pushFormDataToGa(a){
setTimeout(function () {
document.getElementsByTagName('form')[0].getElementsByClassName(a.submitButton)[0].addEventListener('click', function() {
  allFields = document.getElementsByTagName('form')[0].getElementsByTagName('input');
  for(i=0;i<allFields.length;i++){
   if( (allFields[i].type !="hidden" && allFields[i].type !="submit" && allFields[i].value !="" && a.fieldsToExclude.indexOf(allFields[i].id) === -1  ) || (allFields[i].type === "hidden" && a.sendHiddenFields) ){
    console.log( allFields[i].name + ": "  + allFields[i].value);
    if(typeof(_gaq) != "undefined"){
    //Classic
    _trackEvent("Marketo Form Submission", allFields[i].value , allFields[i].name
{'nonInteraction': 1});
    }else if(typeof(ga) !="undefined"){
    //Universal
    ga('send', 'event',"Marketo Form Submission", allFields[i].value , allFields[i].name, {'nonInteraction': 1});
}}}}, false);
}, 3000);}
pushFormDataToGa({
 submitButton: "mktoButton",
 fieldsToExclude: ["Email","LastName", "FirstName"],
 sendHiddenFields : false
});
```

**Schritt 2** Die Daten in GA werden im Abschnitt Reporting angezeigt. Gehen Sie zu Verhalten > Ereignisse > Top-Ereignisse. **Skripteinschränkungen:** - Dieses Codebeispiel ist nur mit [Marketo Forms 2.0 kompatibel](/help/javascript-api/forms-api-reference.md). - Aufgrund der Datenschutzrichtlinien von Google dürfen Sie keine personenbezogenen Daten (E-Mail oder Name) senden. Abgesehen von möglichen Datenschutzbedenken ist dies eine personenbezogene Angabe und verstößt somit gegen [Nutzungsbedingungen von Google Analytics](https://marketingplatform.google.com/about/analytics/terms/us/):

„Sie werden (und werden es keinem Dritten erlauben) den Service zum Nachverfolgen, Erfassen oder Hochladen von Daten, die eine Person persönlich identifizieren (z. B. einen Namen, eine E-Mail-Adresse oder Rechnungsinformationen), oder anderen Daten, die von Google vernünftigerweise mit solchen Informationen verknüpft werden können, verwenden.“

Veröffentlicht am _2014-12-16_ von _Yanir_

## Hinzufügen eines vollständigen Namensfelds zu einem Marketo-Formular

Wir wissen, dass kürzere Web-Formulare die Konversionsraten verbessern. Im folgenden JavaScript-Codebeispiel können Sie Ihre Formulare noch kürzer gestalten, indem Sie die Felder „Vorname“ und „Nachname“ zu einem Feld „Vollständiger Name“ zusammenführen. Wenn Besucherinnen und Besucher ihren vollständigen Namen eingeben, unterteilt das Skript den Text automatisch in die Felder Vor- und Nachname. Bei bekannten Besuchern werden der Vor- und Nachname durch das Skript verbunden und in das neue Feld kopiert, damit sie das Feld nicht erneut ausfüllen müssen. Gehen Sie wie folgt vor, um dies einzurichten.

**Schritt 1** Erstellen Sie in Marketo ein neues benutzerdefiniertes Feld namens „Vollständiger Name“. Es ist nicht erforderlich, ihn in Ihrer CRM-Plattform zu erstellen, da das Skript dieses Feld nur verwendet, um den vollständigen Namen anzuzeigen.
**Schritt 2** Dieses Feld zu allen Web-Formularen hinzufügen. Legen Sie die Felder Vorname und Nachname als ausgeblendet fest. Ändern Sie in der JavaScript die Konfiguration „splitFullName“, sodass sie die drei Feldnamen enthält. Hinweis: Stellen Sie sicher, dass diese Namen nicht an anderer Stelle auf der Seite angezeigt werden.
**Schritt 3** Fügen Sie die JavaScript unten im Code in alle Landingpages ein, und zwar vor dem -Tag.

```javascript
<script>
MktoForms2.whenReady(function (form){
        function splitFullName(a,b,c){
                String.prototype.capitalize = function(){
                        return this.replace( /(^|s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase(); } );
                };
                document.getElementsByName[c](0).oninput=function(){
                        var fullName = document.getElementsByName[c](0).value;
                        if((fullName.match(/ /g) || []).length ===0 || fullName.substring(fullName.indexOf(" ")+1,fullName.length) === ""){
                                var first = fullName.capitalize();;
                                var last = "null";
                        }else if(fullName.substring(0,fullName.indexOf(" ")).indexOf(".")>-1){
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize() + " " + fullName.substring(fullName.indexOf(" ")+1,fullName.length).substring(0,fullName.substring(fullName.indexOf(" ")+1,fullName.length).indexOf(" ")).capitalize();
                                var last = fullName.substring(first.length +1,fullName.length).capitalize();
                        }else{
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize();
                                var last = fullName.substring(fullName.indexOf(" ")+1,fullName.length).capitalize();
                        }
                        document.getElementsByName[a](0).value = first;
                        document.getElementsByName[b](0).value = last;
                };
                //Initial Values
                if(document.getElementsByName[c](0).value.length < 2 && document.getElementsByName[b](0).value.length.length >2 && document.getElementsByName[a](0).value.length.length >2 ){
                        var first = document.getElementsByName[a](0).value.capitalize();
                        var last = document.getElementsByName[b](0).value.capitalize();
                        var fullName =  first + " " + last ;
                        console.log(fullName);
                        document.getElementsByName[c](0).value = fullName;
                }
        }
        splitFullName("FirstName","LastName","leadFullName");
});
</script>
```

Hinweis: Dieser Code funktioniert nur mit Marketo Forms 2.0.

Veröffentlicht am _2014-12-16_ von _Yanir_

## Verwenden von cURL zum Importieren von Leads über die REST-API

Möchten Sie Leads aus einer CSV-Datei über die REST-API importieren, haben jedoch festgestellt, dass dies mit der Verwendung der Postman Chrome-Erweiterung eine Herausforderung darstellt. In diesem Beitrag zeigen wir Ihnen, wie Sie dies mit cURL tun können.

1. [Herunterladen und Installieren von cURL](https://curl.se/download.html), einem Befehlszeilen-Tool, mit dem wir Daten an die REST-API von Marketo übertragen.
1. Öffnen Sie die Befehlszeile und navigieren Sie dann zum Speicherort der CSV-Datei. Die Spaltenüberschriften in der CSV-Datei müssen mit den API-Feldnamen übereinstimmen, nicht mit den Marketo-Feldnamen.
1. Sie benötigen ein Zugriffs-Token. Melden Sie sich bei Marketo an, gehen Sie zu Admin und dann zu LaunchPoint. Suchen Sie Ihren REST API-Benutzer und klicken Sie auf „Details anzeigen“. Klicken Sie auf „Token abrufen“.
1. Sie benötigen außerdem Ihren REST-Endpunkt, der speziell für Ihre Marketo-Instanz gilt. Melden Sie sich bei Marketo an und gehen Sie zu „Admin“ > „Web-Services“. Im mit „REST API“ markierten Abschnitt finden Sie die Endpunkt-URL.
1. Folgen Sie in der Befehlszeile diesem Format für den cURL-Aufruf. Ersetzen Sie `<accesstoken>` durch Ihr Zugriffs-Token aus Schritt 3 und ersetzen Sie `<REST API Endpoint URL>` durch Ihre REST-API-Endpunkt-URL aus Schritt 4. Weitere Informationen finden [hier](https://developer.adobe.com/marketo-apis/api/mapi/#operation/importLeadUsingPOST). Das &quot;/bulk“ hier ersetzt das &quot;/rest“ am Ende der Endpunkt-URL. Wenn Sie den Endpunkt für &quot;/rest/bulk“ festgelegt haben, gibt er einen Fehler zurück.

`curl -i -F format=csv -F file=@leaddata.csv -F access_token=<accesstoken> <REST API Endpoint URL>/bulk/v1/leads.json`

Veröffentlicht am _2014-12-16_ von _Jordan_

## Hinzufügen eines Bestätigungs-Warnhinweises zu einer Marketo für

Beispiel: Wenn ein(e) Benutzende(r) in einem Marketo-Formular auf die Schaltfläche „Senden“ klickt, soll eine Benachrichtigung angezeigt werden, in der der/die Benutzende gefragt wird, ob „Ist das Senden wirklich in Ordnung?“. Dies ist möglich, indem Sie einige Zeilen von JavaScript implementieren, die ein Bestätigungsfeld anzeigen, wenn jemand auf die Schaltfläche Senden klickt. Im Folgenden finden Sie ein Beispiel dafür. Fügen Sie die onSubmit-Funktion wie unten dargestellt zu Ihrem Marketo-Formular hinzu. Weitere Informationen zur Marketo Forms-API finden [ in der Entwicklerdokumentation ](/help/javascript-api/forms-api-reference.md).

```javascript
<script src="//app-e.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_19"></form>
<script>
MktoForms2.loadForm("//app-e.marketo.com", "212-RBI-463", 19,function(form){

//Add this function to your Marketo form script
form.onSubmit(function(){
alert("Do you really want to submit the form?");
});
});
</script>
```

Veröffentlicht am _2014-12-17_ von _David_

## Dankesnachricht ohne Follow-up-Landingpage anzeigen

Wenn Sie Marketo-Formulare verwenden, erstellen Sie in der Regel zwei Landingpages: eine zum Platzieren des Formulars auf und eine zum Umleiten nach dem Ausfüllen des Formulars. In einigen Fällen möchten Sie jedoch möglicherweise nicht zwei separate, aber sehr ähnliche Landingpages verwalten. Mit der JavaScript-API für Forms 2.0 können Sie dieselbe Landingpage für das Formular und die Dankesnachricht verwenden. Erstellen Sie dazu zunächst Ihre Registrierungs-Landingpage und Ihr Formular und platzieren Sie das Formular wie gewohnt auf der Landingpage. Fügen Sie dann der Seite ein HTML-Element hinzu. In diesem Element fügen wir Code hinzu, der bei der Übermittlung des Formulars aktiviert wird. Anschließend wird das Formular ausgeblendet und ein ausgeblendetes angezeigt <div> Das enthält die Dankesnachricht. Ihr JavaScript sollte wie folgt aussehen:

```javascript
//Edit host with your Marketo instance info
<script src="//<host>/js/forms2/js/forms2.js"></script>
<script>
MktoForms2.whenReady(function (form){
 //Add an onSuccess handler
  form.onSuccess(function(values, followUpUrl){
   //get the form's jQuery element and hide it
   form.getFormElem().hide();
   document.getElementById('confirmform').style.visibility = 'visible';
   //return false to prevent the submission handler from taking the lead to the follow up url.
   return false;
 });
});
</script>
```

Bearbeiten Sie den Text der Dankesnachricht.

`<div id="confirmform" style="visibility:hidden;"><p><strong>Thank you. Check your email for details on your request.</strong></p></div>`

Sie sollten den Hostnamen und die Dankesmeldung im Code-Beispiel bearbeiten. Im ersten Beispiel sollte auf Ihre Marketo-Instanz verwiesen werden (z. B. &quot;//app-sj06.marketo.com/js/forms2/js/forms2.js„), im zweiten Fall sollte der Dankestext enthalten sein, den Sie nach Abschluss des Formulars anzeigen möchten. Der Text wird auf der Landingpage genau an der Stelle angezeigt, an der Sie das HTML-Element platzieren. Bearbeiten Sie den Text daher unbedingt im Eigenschaftenblatt. Sie sollten auch sicherstellen, dass die Ebene Ihres HTML-Elements kleiner ist als die Ebene für Ihr Formular. Standardmäßig werden beide auf Ebene 15 platziert, sodass Sie sicher sind, wenn Sie Ihr HTML-Element auf Ebene 11 platzieren. Andernfalls können Sie keine Formularfeldfelder eingeben, die sich mit der Dankesnachricht überschneiden. Es ist nicht erforderlich, den Folgetyp auf dem Formular oder auf der Landingpage zu ändern, da die JavaScript diese Einstellungen überschreibt. Weitere Informationen zur Marketo Forms-API finden Sie in der [Entwicklerdokumentation](/help/javascript-api/forms-api-reference.md).

Veröffentlicht am _2014-12-19_ von _Kristin_

## Hervorheben offener Source-Projekte, die auf der Marketo-Plattform basieren

Dies ist der erste Beitrag in einer fortlaufenden Reihe von Projekten, die von der Entwickler-Community rund um die Marketo-Plattform erstellt wurden. Wir pflegen [eine Liste im GitHub-Konto von Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) in der wir Client-Bibliotheken und Projekte nachverfolgen, die von der Marketo-Entwickler-Community erstellt wurden. Im Folgenden finden Sie drei Projekte, die um die Marketo-REST- und SOAP-APIs entwickelt wurden. Daniel Chesterton erstellte [eine Client-Bibliothek in PHP für die Marketo REST API](https://github.com/dchesterton/marketo-rest-api). Die Client-Bibliothek deckt derzeit 12 REST-API-Endpunkte ab.** Kyle Halstvedt von Elixiter hat ein Projekt erstellt, um [Leads aus statischen Marketo-Listen in eine Google-Tabelle zu ](https://github.com/Elixiter/mkto_google-spreadsheet). Kyles Projekt nutzt die Marketo REST API.  David Santoso hat ein [Ruby-Juwel für die Marketo SOAP-API erstellt.](https://github.com/davidsantoso/markety) Dieses Projekt kann Ihnen dabei helfen, die Marketo SOAP-API schneller in eine Ruby on Rails-App zu integrieren.  Wir freuen uns, dass auf der Marketo-Plattform mehr Projekte von der Entwickler-Community erstellt werden. Wenn Sie an einem Open-Source-Projekt für die Marketo-Plattform arbeiten, senden [ es über eine Pull-Anfrage an dieses GitHub-Repository](https://github.com/Marketo/Community-Supported-Client-Libraries).

Veröffentlicht am _2015-01-02_ von _Murta_

## Dynamisches Ändern des Seiteninhalts basierend auf dem Standort eines Benutzers

Angenommen, Sie möchten die Telefonnummer auf einer Landingpage dynamisch ändern, je nachdem, wo sich ein Benutzer befindet. Wenn sich die Person beispielsweise in Kalifornien befindet, möchten Sie ihr auf Ihrer Landingpage die Telefonnummer für Ihr Büro in Kalifornien anzeigen. Wenn sie sich jedoch in Japan befindet, möchten Sie ihr die Telefonnummer für das japanische Büro anzeigen.  Eine Möglichkeit, dies zu implementieren, besteht in der Verwendung von JavaScript und der [HTML5-Geolocation-API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). Der Vorteil dieses Ansatzes besteht darin, dass wir eine Landingpage erstellen und sie dynamisch basierend auf dem Standort eines Benutzers ändern können, anstatt mehrere statische Landingpages zu verwenden. Im Folgenden werden die technischen Implementierungsdetails beschrieben. Wir erstellen ein Objekt für Bürostandorte mit Längen- und Breitenkoordinaten und ein zweites Objekt mit Bürotelefonnummern. In der Produktionsumgebung ist es empfehlenswert, diese beiden Objekte zu einem einzigen Objekt zusammenzufassen.

```json
//Coordinates for Marketo offices
var officeLocations = {
    "San Mateo": {latitude: 37.5596465, longitude: -122.2870142},
    "Atlanta": {latitude: 33.8547013, longitude: -84.35552349999999},
    "Tokyo": {latitude: 35.6895, longitude: 139.6917},
    "Dublin": {latitude: 53.3478, longitude: -6.2603097},
    "Sydney": {latitude: -33.873651, longitude: 151.2068896},
    "Portland": {latitude: 45.512089, longitude: -122.6763367},
    "Tel Aviv": {latitude: 32.0852999, longitude: 34.78176759999999}
}

//Phone numbers for Marketo offices
var officePhoneNumbers = {
    "San Mateo": "+1-650-376-2300",
    "Atlanta": "+1-877-260-6586",
    "Tokyo": "+81-03-6759-8280",
    "Dublin": "+353-1-242-3000",
    "Sydney": "+61-2-9045-2711",
    "Portland": "+1-877-260-6586",
    "Tel Aviv": "+1-877-260-6586"
}
```

Wir erstellen eine Methode, um den Standort eines Benutzers anzufordern. Wenn der Standort des Benutzers nicht zugänglich ist, verwenden wir zur Fehlerbehebung standardmäßig den Hauptsitz und die Telefonnummer von Marketo.

```javascript
//Method to get user's current location. Returns a position object with user's geo coordinates
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(findNearestOffice);
    } else {
        x.innerHTML = "Marketo Location: San Mateo
Marketo Phone Number: +1-877-260-6586";
    }
}
```

Schließlich erstellen wir eine Methode, um das Büro zu finden, das dem Standort des Benutzers am nächsten ist, und geben dann die Telefonnummer für das Büro zurück, das sich auf der Seite am nächsten befindet. Diese Methode verwendet die findNearest-Methode aus [Geolib, einer JavaScript-Bibliothek, die Geodatenoperationen bereitstellt](https://github.com/manuelbieh/Geolib).

```javascript
//Find nearest Marketo office to user's location
function findNearestOffice(position) {
        var nearestOffice = geolib.findNearest({latitude: position.coords.latitude, longitude: position.coords.longitude}, officeLocations);
        x.innerHTML = "Marketo Location: " + nearestOffice.key + "
Marketo Phone Number: " +  officePhoneNumbers[nearestOffice.key];
}
```

Hier ist die vollständige Implementierung. Wenn der Benutzer auf die Schaltfläche auf der Seite klickt, wird die getLocation-Methode als Trigger verwendet. Dieses [GitHub-Repository](https://github.com/MurtzaM/Find-Nearest-Marketo-Office) verfügt über die Dateien, die zum Einrichten dieser Demo erforderlich sind.

Veröffentlicht am _2014-12-_) von _Murta_

## Lead-Tracking und mehrere Domains

Mit dem Munchkin-Trackingcode von Marketo können Sie Besuche auf Ihrer Website verfolgen. Es empfiehlt sich wahrscheinlich, den Munchkin-Trackingcode zu verwenden, um anonyme Leads für die meisten oder alle Seiten Ihrer Website zu Cookies zu senden. Lassen Sie uns die Funktionsweise von Munchkin erläutern. Besuche auf der Seite werden für bestehende Leads aufgezeichnet, und ein Besuch auf der Seite durch einen nicht-Cookie-Besucher führt dazu, dass ein neues Cookie erstellt und gespeichert wird und ein neuer anonymer Lead in Ihrer Marketo-Datenbank erstellt wird. Der Munchkin-Tracker Cookies automatisch, wenn noch kein Cookie für die aktuelle Domain vorhanden ist. In Marketo wird das Ereignis (Klick auf einen Link, Besuch einer Web-Seite oder eines neuen Leads) im Aktivitätsprotokoll des Leads protokolliert. Der im Cookie gespeicherte Wert ist für einen bestimmten Besucher eindeutig. Der Wert ist eine Kombination aus der eindeutigen Munchkin-Konto-Tracking-ID, dem Domain-Namen, dem Zeitstempel und einer zufälligen Ganzzahl.
**Was passiert, wenn ich mehrere Domains habe?** Angenommen, Sie verfügen über zwei Websites, die Sie verfolgen möchten: `<www.apples.com>` und `<www.bananas.com>`. Sie können den Trackingcode auf beiden Sites platzieren. Beachten Sie jedoch Folgendes. Marketo-Cookies sind „Erstanbieter-Cookies“ und daher domänenspezifisch. Das bedeutet, dass ein Besucher von Site 1 als anonymer Lead in Marketo erstellt wird. Wenn derselbe Lead dann zu Site 2 wechselt, wird ein zweiter separater anonymer Lead in Marketo erstellt. Wenn der Lead ein Formular auf Site 1 ausfüllt, wird dieser Datensatz bekannt. Der anonyme Datensatz für Site 2 bleibt bestehen und sammelt weiterhin nachfolgende Besuche auf dieser Site. Wenn der Lead dann ein Formular auf Site 2 mit derselben E-Mail-Adresse ausfüllt, die auch auf Site 1 verwendet wird, werden beide bekannten Leads automatisch zusammengeführt und das gesamte vergangene und zukünftige Verhalten wird in Marketo in einem einzigen Datensatz erfasst. Beide Cookie-IDs sind an denselben Lead gebunden und alle Web-Aktivitäten (aus beiden Domains) befinden sich in diesem Lead.
**Was ist mit mehreren Subdomains?** Subdomains sind kein Problem. Nehmen wir als Beispiel Marketo.com . Es verfügt über mehrere Subdomains für verschiedene Sprachen, z. B. fr.marketo.com und de.marketo.com. Bei Subdomains wird jede Aktivität für denselben Lead-Datensatz/Cookie aufgezeichnet.

Veröffentlicht am _2015-01-13_ von _David_

## Ändern der Textfarbe für Hinweise in einem Marketo-Formular

Angenommen, Sie möchten die Farbe des Hinweistextes (auch als Platzhaltertext bezeichnet) in Forms 2.0 ändern. Dies ist über benutzerdefiniertes CSS möglich. Im folgenden Screenshot habe ich beispielsweise den Hinweistext in diesem Marketo-Formular blau dargestellt. Je nachdem, wie Sie Marketo Forms verwenden, gibt es drei Optionen für die Vorgehensweise.

**Option 1: Wenn Sie ein Marketo-Formular einbetten, fügen Sie die unten stehende CSS-Datei direkt zu Ihrer Haupt-CSS-Datei hinzu.**

```css
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
```

**Option 2: Beim Einbetten eines Marketo-Formulars können Sie das CSS direkt auf der Seite zwischen `<style></style>` Tags im `<head>` Abschnitt hinzufügen.**

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

**Option 3: Wenn Sie ein Marketo-Formular auf einer Marketo-Landingpage verwenden, können Sie dieses benutzerdefinierte CSS über die Marketo-Benutzeroberfläche hinzufügen.** Suchen Sie die Landingpage im Navigationsbaum von Marketo. Klicken Sie dann auf Entwurf bearbeiten . Klicken Sie auf Seiten-Meta-Tags bearbeiten . Fügen Sie das unten stehende CSS zum Abschnitt Benutzerdefinierte HEAD HTML hinzu. Die `<style></style>` Tags sollten enthalten sein.

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

Klicken Sie auf Entwurf genehmigen. Wenn Sie jetzt die Marketo-Landingpage besuchen, ist der Hinweistext die Farbe, die Sie in CSS definiert haben. Weitere Informationen zu Marketo Forms finden [in der Dokumentation](/help/javascript-api/forms-api-reference.md).

Veröffentlicht am _2015-01-14_ von _Murta_

## Abrufen von Aktivitätsdaten über die REST-API

Nehmen wir an, Sie möchten alle Leads erhalten, die in diesem Monat zu einer Liste hinzugefügt wurden. Mithilfe der [Abrufen von Lead-Aktivitäten REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET)-API können Sie diese Daten abrufen. Bevor Sie die API Lead-Aktivitäten abrufen, müssen Sie ein Zugriffs-Token von der Authentifizierungs-API sowie ein Startdatum-Token von der API [Paging-Token abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getActivitiesPagingTokenUsingGET). Nachfolgend finden Sie einen Beispiel-Code in Ruby, der die einzelnen API-Endpunkte erläutert, die Sie aufrufen müssten, um alle Leads zurückzugeben, die in diesem Monat zu einer Liste hinzugefügt wurden. 1. Zugriffs-Token abrufen**

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com/identity/oauth/token?grant_type=client_credentials>"
# Relace with your client id
client_id = "99985d09-22a9-3jl2-84av-f5baae7c3a45"
# Replace with your your  client secret
client_secret = "tZPVrKiEmUDezE18yZfeaPlTJ2vKn2fw"
request_url = marketo_instance + "&client_id=" + client_id + "&client_secret=" + client_secret

# Make request
response = RestClient.get request_url

# Parse reponse and return only access token
results = JSON.parse(response.body)
access_token = results["access_token"]
puts access_token
```

1. Paging-Token abrufen

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities/pagingtoken.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify date
since_date_time = "&sinceDatetime=2015-01-01T00:00:00-08:00"
request_url = marketo_instance + endpoint + auth_token + since_date_time

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Aktivitätsdaten abrufen** Um die für diesen Aufruf benötigte Aktivitätstyp-ID zu ermitteln, fragen Sie die &quot;[ Aktivitätstypen-API“ ](/help/rest-api/activities.md). Die API zum Abrufen von Aktivitätstypen gibt ein Schema mit allen Aktivitätstypen und zugehörigen IDs zurück. Beispielsweise wird ID 12 für neu erstellte Leads und ID 1 für den Web-Seitenbesuch zurückgegeben.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
# Specify activities needed
activity_type_ids = "&activityTypeIds=24"
request_url = marketo_instance + endpoint + auth_token + since_date_time + activity_type_ids

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Die API zum Abrufen von Lead-Aktivitäten gibt mit jeder Antwort, die Sie zum Paginieren durch den Ergebnissatz verwenden können, ein Paging-Token zurück.** Weitere Informationen finden Sie in der [REST-API-Dokumentation](/help/rest-api/rest-api.md).

Veröffentlicht am _2015-01-_) von _Murta_

## Hervorheben offener Source-Projekte, die auf der Marketo-Plattform basieren: Teil 2

Dies ist der zweite Beitrag in einer fortlaufenden Serie, in der von der Entwickler-Community rund um die Marketo-Plattform erstellte Open-Source-Projekte vorgestellt werden. Wir pflegen [eine Liste im GitHub-Konto von Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) in der wir Client-Bibliotheken und Projekte nachverfolgen, die von der Marketo-Entwickler-Community erstellt wurden. Im Folgenden finden Sie drei Projekte, die um die Marketo SOAP- und Munchkin-APIs entwickelt wurden. [PunchTab](https://www.punchtab.com/) hat [eine Client-Bibliothek in Python für die Marketo SOAP-API erstellt](https://github.com/PunchTab/suds-marketo). [Flickerbox](https://www.flickerbox.com/) erstellte [eine Client-Bibliothek in PHP für die Marketo SOAP API](https://github.com/flickerbox/marketo).* [Richard Morrison](https://x.com/mozz100) erstellte [ein PHP-Skript, um Lead-Daten von der Marketo SOAP-API abzurufen und diese Daten dann mithilfe von JavaScript an den Client zu übergeben.](https://github.com/mozz100/marketo-whodat) Dieses Projekt kann Ihnen dabei helfen, eine Seite basierend auf den Daten eines Benutzers in Marketo zu ändern.  Wir freuen uns, dass auf der Marketo-Plattform mehr Projekte von der Entwickler-Community erstellt werden. Wenn Sie an einem Open-Source-Projekt für die Marketo-Plattform arbeiten, senden [ es über eine Pull-Anfrage an dieses GitHub-Repository](https://github.com/Marketo/Community-Supported-Client-Libraries).

Veröffentlicht am _2015-01-_) von _Murta_

## Klicks auf die RTP-Empfehlungs-Engine an Google Analytics senden

Hier finden Sie eine Lösung für Marketo Real-Time Personalization (RTP)-Benutzer, um Klicks von der Content Recommendation Engine in Google Analytics anzuzeigen. Sobald ein Besucher auf die Inhaltsempfehlungsleiste klickt, wird unter der Ereigniskategorie „RTP-Recommendations“ ein Ereignis an Google Analytics gesendet. In Analytics wird der Empfehlungstext (wie in der Leiste angezeigt) an die Ereignisbeschriftung und die URL des empfohlenen Assets an die Ereignisaktion angehängt. Das Skript funktioniert sowohl für Classic Google Analytics als auch für Google Universal Analytics. Dieses Tag sollte am Ende des HTML-Seiten-Codes eingefügt werden, sodass es das letzte Tag vor dem `</body>`-Tag ist.

```javascript
$( document ).ready(function() {
if(document.getElementsByClassName("insightera-bar-content").length
 >0){
document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].addEventListener("click",
 function(){
assetName
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].innerText;
assetURL
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].href;
assetURL=
 assetURL.substring(assetURL.lastIndexOf("/"),assetURL.indexOf("?iesrc"));
console.log(assetName

 * " | " + assetURL);
if(typeof(_gaq)
 != "undefined"){
//Classic
_trackEvent("RTP-Recommendations",
 assetName , assetURL , {'nonInteraction': 1});
}else
 if(typeof(ga) !="undefined"){
//Universal
ga('send',
 'event',"RTP-Recommendations", assetName , assetURL, {'nonInteraction': 1});
}
});
}
});
```

Veröffentlicht am _2015-01-22_ von _Yanir_

## Verwenden der Marketo REST-API mit Boomi: Abrufen und Speichern eines REST-Authentifizierungstokens

Ein gängiges Anwendungsbeispiel für Marketo ist die Einrichtung eines automatischen Exports von Leads, die bestimmte Kriterien erfüllen. Obwohl dies derzeit nicht in der Marketo-Benutzeroberfläche möglich ist, ist es ziemlich einfach, das Drittanbieter-Tool wie Dell Boomi, eine statische Liste mit einigen Datenverwaltungskampagnen und die Marketo REST-API zu verwenden. Die REST-API? Ich dachte, Boomi hätte keinen Marketo REST-API-Connector! Derzeit ist dies nicht der Fall, aber es ist möglich, dasselbe mit dem HTTP-Connector zu erreichen und die JSON-Antwort-Formen manuell zu definieren. Der erste Schritt besteht darin, Ihre Marketo-Instanz so einzurichten, dass sie die REST-API verwendet, wie auf der [REST-API Marketo Developer Page](/help/rest-api/rest-api.md) beschrieben. Ich gehe auch davon aus, dass Sie Zugriff auf ein Dell Boomi-Konto haben und über die Boomi-Fähigkeiten verfügen, um diese Arten von Integrationsprozessen zu erstellen. Der endgültige Prozess sieht wie folgt aus und umfasst Aufrufe an die folgenden Marketo-REST-API-Vorgänge, die jeweils eine zugehörige jSON-Antwort-Form aufweisen, die auf der Entwickler-Site zu finden ist. Um Zeit zu sparen, habe ich sie unter Beispiel-JSON für [Authentifizierung“ ](/help/rest-api/authentication.md)

```json
{
  "access_token": "",
  "token_type": "",
  "expires_in": 0,
  "scope": ""
}
```

Beispiel-JSON für [Abrufen mehrerer Leads nach Listen-ID](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Beispiel-JSON für [Leads aus Liste entfernen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Eigenschaften definieren: Bevor wir mit dem Aufruf von REST beginnen, müssen Sie die verwendeten Variablen externalisieren und einkapseln. Ich habe Folgendes definiert, das unten gezeigt wird.

* Client-ID: Dies von Ihrem REST Launchpoint-Service abrufen
* Client-Geheimnis: Dies von Ihrem REST Launchpoint-Service abrufen
* AccessToken: Dies erhalten wir von einem REST-Aufruf.
* Statische ListID: Die LIST-ID der statischen Liste, mit der wir arbeiten werden. Dies von der URL in Marketo abrufen
* Felder: Eine kommagetrennte Liste von Feldern, die der REST-Service für jeden Lead von Marketo erhält. Meine ist „id, email,firstName,lastName“ * IDStringToDelete: Enthält schließlich die ID aller Leads in der statischen Liste, die bei ihrer Entfernung aus der Liste verwendet werden sollen
* ActivityTypes: Wird in Teil 2 dieses Blogs verwendet, wo ich auf diese erweitern!
* SinceDateTime: Wird in Teil 2 dieses Blogs verwendet, wo ich auf diese erweitern!
* PagingToken: wird in Teil 2 dieses Blogs verwendet, wo ich auf diese erweitern!
* Ordner - Ausgehend: Der Pfad zum ausgehenden Ordner auf dem SFTP-Server. Ich verwende in diesem Beispiel &quot;/data/outgoing“. Damit können wir den SFTP-Vorgang parametrisieren, um ihn generisch zu machen.

Das Authentifizierungs-Token: Wie bereits erwähnt, platzieren wir einen Connector auf der Arbeitsfläche, nachdem wir den Prozess mit der Startform „Keine Daten“ erstellt haben (dies ist nur eine persönliche Wahl, ich mag alle meine Connectoren, die wie britische Stecker aussehen).
Der Connector sollte wie folgt konfiguriert werden: - Connector ist ein HTTP-GET-Client - Verbindung verwendet URL: `https://123-ABC-456.mktorest.com` (beachten Sie am Ende /rest, damit wir dies für REST-Aufrufe sowie für den Abruf des Identitäts-Zugriffstokens verwenden können. und ändern Sie 123-ABC-456 in den richtigen Pfad für Ihre Marketo-Instanz) - Vorgang ist „OAuth-Token abrufen“ (neu!) - Anfrageprofil = Keine - Antwortprofil = JSON - Neues Profil namens „Authentifizierungs-Token-Antwort“ - Inhaltstyp: Einfach - HTTP-Methode: GET - Ressourcenpfad (fügen Sie 4 ohne Anführungszeichen hinzu): „identity/oAuth/token?grant_type=client_credentials&amp;client_id=&quot;; „ClientID (Ersatzvariable)“; &quot;&amp;client_secret=&quot;; „ClientSecret (Ersatzvariable)“ - Parameter konfigurieren —> —>(+): Set ClientID = Process Property Client ID; Set ClientSecret = Process Property Client Secret Danach speichern Sie das Erfolgs-Token in der Variablen „AccessToken“, wie dargestellt, und extrahieren es aus der JSON-Antwort.
Das Muster für diesen Schritt wird für die nächsten Schritte wiederholt, jedoch unter Verwendung neuer Vorgänge mit anderen JSON-Rückgabeprofilen. In der Tat werden viele der REST-Aufrufe auf die gleiche Weise mit geringfügigen Änderungen behandelt! Im nächsten Teil werden wir dies erweitern und mithilfe von REST eine Liste der Leads aus einer statischen Liste abrufen! Führen Sie zunächst den Prozess aus, setzen Sie jedoch eine Stoppform nach „Eigenschaften festlegen“ und führen Sie dann in Debug aus, um sicherzustellen, dass Sie dasselbe Token sehen, das Sie in Marketo sehen. Sie sollten perfekt zusammenpassen!

Veröffentlicht am _2015-01-26_ von _John_

## Verwenden einer Google Font-API, um einer Marketo-Landingpage eine benutzerdefinierte Schriftart hinzuzufügen

**Hinweis: Dies ist ein Blogpost von [Murtza Manzur](https://www.linkedin.com/in/murtzam). Murtza ist ein Marketo Developer Evangelist aus der San Francisco Bay Area.**
Angenommen, Sie erstellen eine Landingpage in Marketo und möchten eine benutzerdefinierte Schriftart verwenden. Dies ist mit der Google Font-API möglich.  Fügen Sie Ihrer CSS-Datei eine Importmethode hinzu, die auf Google Fonts verweist:

`@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,300,600);`

Veröffentlicht am _2015-01-26_ von _Murta_

## Den Vornamen eines Leads mithilfe von E-Mail-Skripting großschreiben

Nehmen wir an, ein Lead gibt seinen Namen in Kleinbuchstaben ein, z. B. „Martin Müller“. Aber wenn Sie eine E-Mail-Kampagne versenden, sollten Sie den Namen des Leads in der E-Mail großschreiben, so wie John Doe. Sie können den Namen eines Leads mithilfe von E-Mail-Skripting großschreiben. Gehen Sie wie folgt vor:

1. Klicken Sie in Ihrem E-Mail-Programm auf die Registerkarte „Meine Token“.
1. Erstellen Sie ein neues E-Mail-Skript-Token, indem Sie „E-Mail-Skript“ aus dem rechten Bedienfeld in das mittlere Bedienfeld ziehen. Benennen Sie das Token.
1. Fügen Sie im Textfeld Skript-Token bearbeiten den folgenden Code ein. Aktivieren Sie im rechten Bedienfeld unter Lead-Objekt das Kontrollkästchen Vorname . Klicken Sie dann auf Speichern.

```javascript
# set($name = ${lead.FirstName})
# set($formattedFirstName = $name.substring(0).toUpperCase())
$formattedFirstName
```

1. Referenzieren Sie das Token in Ihrem E-Mail-Asset. Der Vorname des Leads wird ausgegeben, wobei der erste Buchstabe großgeschrieben wird. Weitere Informationen zu E-Mail-Scripting finden Sie unter [E-Mail-Scripting-Dokumentation](/help/email-scripting.md).

Veröffentlicht am _2015-01-26_ von _Murta_

## Abrufen aller Leads von der Marketo REST-API

Es gab eine [Frage zu StackOverflow mit der Frage, wie man eine Liste aller Leads von Marketo über die REST-API abrufen ](https://stackoverflow.com/questions/28184900/how-do-i-get-the-list-of-all-the-leads-in-marketo). Sie können diese Daten mit dem [Abrufen mehrerer Leads nach REST-API-Endpunkt vom Typ Filter](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) abfragen. Leads in Marketo werden Lead-IDs in sequenzieller Reihenfolge zugewiesen, die mit 1 beginnt. Mithilfe des [Abrufen mehrerer Leads nach Filtertyp REST-API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)Endpunkt können Sie bei jedem Aufruf 300 Leads nach Lead-ID abfragen. Bei jedem Aufruf dieses Endpunkts müssen Sie id als filterType und die Lead-IDs als filterValues angeben. Um alle Leads zu erhalten, iterieren Sie durch die Gesamtzahl der Leads 300 gleichzeitig. Y
Sie können die Gesamtzahl der Leads in einer Marketo-Instanz über die Marketo-Benutzeroberfläche abrufen. Wechseln Sie in der Benutzeroberfläche von Marketo zur Registerkarte Lead-Datenbank , klicken Sie auf System-Smart-Listen, klicken Sie auf Alle Leads Smart-Liste und schließlich auf die Registerkarte „Leads“. Klicken Sie dann auf die Spalte ID und sortieren Sie absteigend. Nachdem Leads sortiert wurden, ist die ID des ersten Leads die obere Grenze für Leads-ID, wenn Sie alle Leads abfragen. Wenn Sie keinen Zugriff auf die Marketo-Benutzeroberfläche haben, um die Gesamtzahl der Leads zu erhalten, gibt es einen [ Ansatz, um diesen Wert mithilfe der REST-API für Lead-Aktivitäten abzurufen](https://stackoverflow.com/questions/28419967/get-all-leads-programmatically-in-marketo-v1).

1. Erster API-Aufruf: Ersetzen Sie … durch alle Werte dazwischen:

`/rest/v1/leads.json?filterType=Id&filterValues=1,2,3,...,298,299,300`

Im Folgenden finden Sie einen Beispiel-Code in Ruby für den ersten Aufruf.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Replace with filter type and values
ids_needed = (1..300).to_a.join(",")
filter_type_and_values = "&filterType=Id&filterValues=" + ids_needed
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Der zweite API-Aufruf und jeder nachfolgende API-Aufruf folgen demselben Muster, bis die Gesamtzahl der Leads erreicht ist:

```
//replace ... with all the values in between
/rest/v1/leads.json?filterType=Id&filterValues=301,302,303,...,598,599,600
```

Weitere Informationen finden Sie in der [REST-API-Dokumentation](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Veröffentlicht am _2015-01-28_ von _Murta_

## Ausführen von Formularübermittlungsaktionen vom IFrame zur übergeordneten Seite

Wir haben einige Fälle gesehen, in denen Benutzer iframe-Formulare verwenden und Besucher, die das Formular ausgefüllt haben, zu einer Dankeseite oder PDF, Video usw. weiterleiten möchten. Da das Formular in eine Landingpage eingebettet ist, die sich von der übergeordneten unterscheidet, tritt die Aktion nur auf der inneren Seite auf, auf der sich das Formular befindet. Um dies zu beheben, sehen Sie unten zwei von uns erstellte JavaScript-Tags. Fügen Sie als HTML-Element in Ihre iframe-Seiten oder direkt in die Landingpage-Vorlage ein, die Sie für iframes verwenden. Platzieren Sie sie vor dem letzten `</body>` Tag. Mit dem ersten Tag wird die Aktion auf der übergeordneten Seite ausgeführt, und mit dem zweiten Tag wird sie in einer neuen Registerkarte geöffnet.

**Formularaktion auf einer übergeordneten Seite**

```javascript
<script>
MktoForms2.whenReady(function (form){
form.onSuccess(function (values, url){
window.parent.location.assign(url);
return false;
           });
});
</script>
```

**Formularaktion in einer neuen Registerkarte**

```javascript
<script>
MktoForms2.whenReady(function (form){
var newWin;
form.onSubmit(function (){
newWin = window.open('about:blank', 'myWindow');
});
form.onSuccess(function (values, url){
newWin.location.replace(url);
return
 false;
});
});
</script>
```

Veröffentlicht am _2015-02-02_ von _Yanir_

## Senden von Ansichtsdaten aus einem YouTube-Video an den Markt

Angenommen, Sie möchten Leads in Marketo segmentieren, je nachdem, ob sie ein bestimmtes Video gestartet oder abgeschlossen haben. Dies ist mit Munchkin, der Iframe-API von YouTube und Smart Lists in Marketo möglich. Mit dem Beispielcode in diesem Beitrag können Sie über Munchkin Videostartereignisse und Videoabschlussereignisse an Marketo senden. Dazu muss Munchkin auch auf der Seite geladen werden, bevor Sie Videoansichtsereignisse an Marketo senden können. Das Video wurde gestartet und ist beendet. Es wird im Aktivitätsprotokoll des Leads angezeigt. Nachdem die Daten in Marketo gespeichert wurden, können Sie eine Smart-Liste und Segment-Leads erstellen, die ein Video gestartet oder abgeschlossen haben.

1. Rufen Sie die ID des YouTube-Videos ab, das Sie einbetten möchten.** Notieren Sie sich in der URL des YouTube-Videos, das Sie verwenden möchten, die ID, d. h. die Reihe zufälliger Zeichen nach dem `v=`.
1. Platzieren Sie die YouTube-Video-ID aus Schritt 1 in der achten Zeile dieses Codebeispiels. Platzieren Sie dann den Code vor dem `</body>` im HTML Ihrer Seite.

```javascript
<div id="player"></div>
<script>
var tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
document.getElementsByTagName('head')[0].appendChild(tag);

//Change 'iiqxcjxJ5Us' to video needed
var player, videoId = 'iiqxcjxJ5Us';
function onYouTubeIframeAPIReady() {
player = new YT.Player('player', {
height: '390',
width: '640',
videoId: videoId,
events: {
'onStateChange': onPlayerStateChange
}
});
}

function onPlayerStateChange(event) {
switch( event.data ) {
//Send video started event to Marketo
case YT.PlayerState.PLAYING: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=started'
}
);
break;
//Send video finished event to Marketo
case YT.PlayerState.ENDED: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=finished'
}
);
break;
}

}
</script>
```

1. Erstellen Sie in Marketo eine Smart-Liste mit der URL des Videos und dem Ansichtsereignis, nach dem Sie suchen, als Wert von „Abfragezeichenfolge enthält“. Weitere Informationen zur YouTube Iframe-API [ Sie in der API-Dokumentation von YouTube](https://developers.google.com/youtube/iframe_api_reference). Weitere Informationen zu Munchkin finden Sie [in der Entwicklerdokumentation zu Marketo](/help/javascript-api/lead-tracking.md).

Veröffentlicht am _2015-02-02_ von _Murta_

## Tipps und Tricks zur Marketo SOAP-API

HINWEIS: Dies ist ein Gast-Blogpost. [Ed Blachman ist Senior Architect](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D2777965) bei [TIBCO Software, einem bekannten Anbieter von Unternehmenssoftware](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Ed arbeitet an Produkten, die es so genannten „Bürgerentwicklern“ ermöglichen, die von ihnen verwendeten Cloud-Services zu integrieren, ohne selbst programmieren zu müssen. [Marketos SOAP-API](/help/soap-api/soap-api.md) ist ein leistungsstarkes Tool, mit dem Entwickler die Leistungsfähigkeit von Marketo nutzen und in unsere eigenen Programme integrieren können. Zwischen [formalen Dokumentation](./getting-started.md) und [Community-](https://nation.marketo.com/)) gibt es viele Informationen zur Verwendung. Als ich anfing, stützte ich mich stark auf diese Informationen und fand sie unbezahlbar. Dabei habe ich mir aber Tipps und Tricks aufgebaut, die ich sonst nirgends gesehen hatte. Hier sind einige der Dinge, die ich herausgefunden habe.

**Die Sandbox für Entwickler** Die Sandbox ist natürlich eine wunderbare Ressource für API-Entwickler: ein sicherer Ort, an dem Sie mit Marketo-Funktionen experimentieren und Objekte hinzufügen und entfernen können, ohne die echten Marketing-Aktivitäten der eigentlichen Marketo-Benutzenden Ihres Unternehmens zu beeinträchtigen. Die Sandbox ist jedoch kein Allheilmittel.
Beispielsweise musste ich unsere Sandbox mit einer anderen Entwicklungsgruppe teilen. Dafür war einiges nötig, weil sie sich daran gewöhnt hatten, dass sie die Sandbox besaßen. Schließlich haben wir ein paar Best Practices für die Freigabe herausgefunden: - Schreiben Sie keine Tests, die auf vollständiges Wissen über den Inhalt Ihrer Sandbox angewiesen sind. Als gemeinsam genutzte Ressource können sich Schemata ohne Vorankündigung ändern. Ebenso können sich komplette Einträge in Ihrer Lead-Datenbank, in Programmen oder anderen Entitäten ändern. Wenn für Ihre Tests vollständige Kenntnisse der Sandbox vorausgesetzt werden, erstellt Ihr Entwicklungszyklus Ausfallzeiten für die Gruppen, für die Sie die Sandbox freigeben. Da ihr Entwicklungszyklus normalerweise nicht mit dem Ihres übereinstimmt, läuft dies darauf hinaus, die Ressource zu blockieren und nicht zu kühlen. Es ist auch nicht notwendig, wenn man es durchdenkt. - Benutzen Sie eine Konvention, um all Ihre Sachen zu beschriften - Ihre Leads, Ihre Lead-Schema-Felder, Ihre Programme, was auch immer. Wenn Sie jeweils Ihre eigenen Objekte identifizieren können und Sie mit Ihren Mitmandanten vereinbaren können, dass jeder von Ihnen die Objekte der anderen in Ruhe lässt, sollten Sie für die Freigabe auf einer soliden Grundlage stehen. Für Leads können Sie ein benutzerdefiniertes Feld erstellen und eine Konvention mit diesem benutzerdefinierten Feld erstellen, um diese Leads als Ihre Test-Leads zu identifizieren. Bei Listen oder Programmen können Sie die Namen Ihrer Objekte mit einer Zeichenfolge beginnen, die angibt, dass diese Objekte zu Ihnen gehören. - Erwägen Sie, Tests zu schreiben, die nach sich selbst bereinigen - zuerst erstellen Sie die Objekte, die Sie interessieren, dann greifen Sie auf sie zu oder aktualisieren oder löschen Sie sie selektiv, dann entfernen Sie sie schließlich. (Beachten Sie, dass dies in der SOAP-API nicht zu 100 % möglich ist, da nicht alles in der Sandbox oder in einer echten Instanz über die SOAP-API verwaltet werden kann. Trotzdem lohnt es sich, dies so viel wie möglich zu tun.)

**Reale Instanzen** Das Problem mit der Sandbox besteht darin, dass sie nicht in der Produktion verwendet wird, sodass es schwierig ist, einen Eindruck davon zu gewinnen, wie die reale Nutzung in einer Marketo-Instanz aussieht. Wenn Sie das Glück haben, einen Marketo-Power-User in Ihrem Team zu haben, oder wenn Sie eine maßgeschneiderte Entwicklung für interne Marketo-Anwender durchführen, ist das kein solches Problem. Aber im Falle meines Teams war es tatsächlich eine große Sache. Keiner von uns war Marketo-Experte, und da man uns nach dem Verständnis einer großen Anzahl von Cloud-Services fragte, hatten wir einfach nicht die nötige Personalstärke, um Experten für irgendetwas zu werden. Im Folgenden finden Sie einige der Erkenntnisse, die wir aus dem Zugriff auf eine echte Instanz gewonnen haben: - Große Lead-Schemata. Das Lead-Schema in der Produktionsinstanz, auf die wir zugegriffen haben, enthält über 200 Felder. Dadurch wurde unseren Benutzeroberflächen-Designern unmissverständlich klar, dass die von ihnen entworfene Benutzeroberfläche Schemata dieser Größe (oder größer) enthalten musste. - Bursty-Nutzung. Wir sahen einen Unterschied in zwei Größenordnungen zwischen den Zeiten mit der höchsten Nutzung und den Zeiten mit geringer Nutzung (in Bezug auf die Anzahl der erstellten oder aktualisierten Leads). Dies wirkte sich sowohl auf das Datenvolumen aus, das wir von API-Aufrufen erhalten würden (offensichtlich), als auch auf die Zeit aus, die ein API-Aufruf benötigt, um zu reagieren (möglicherweise weniger offensichtlich).

**API-Aufruf-Antwortzeit** Je nach Tageszeit, den Details Ihres API-Aufrufs und dem Inhalt Ihrer Instanz kann es vorkommen, dass die Antwortzeit der SOAP-API länger als durchschnittlich dauert. Gelegentlich hatten wir API-Aufrufe, die anderthalb Minuten brauchten, um zu antworten. Sie müssen sich der Möglichkeit bewusst sein, damit umzugehen: - Test. Vielleicht ist das kein Problem für Ihre Nutzung. Aber nehmen Sie das nicht einfach an, machen Sie ein paar Tests. - Anpassen der Nutzung. In unserem Fall war das größte Problem, dass wir die Seitengröße für unsere Aufrufe auf [getMultipleLeads](/help/soap-api/getmultipleleads.md) so groß festgelegt haben, wie die API es zulässt. In unserem Kontext ergibt das einen gewissen Sinn, denn unser Ziel ist es, mit dem API-Kontingent unseres Kunden so effizient wie möglich zu sein. In Ihrem Kontext müssen Sie sich jedoch möglicherweise nicht so intensiv um die API-Aufrufkontingente Ihrer Benutzer sorgen. In diesem Fall erhalten Sie definitiv eine bessere Antwortzeit, indem Sie nach kleineren Datenseiten fragen.

**Lead-Partitionierung** Marketo bietet leistungsstarke Tools - Partitionen und Arbeitsbereiche -, die es mehreren Marketing-Gruppen ermöglichen, eine Marketo-Instanz gemeinsam zu nutzen. Diese Tools werden jedoch nicht direkt in der SOAP-API angezeigt. Wenn Sie z. B. getMultipleLeads verwenden, um alle Leads abzurufen, die seit einem Datum oder einer Uhrzeit aktualisiert oder erstellt wurden, erhalten Sie alle Leads in Ihrer Instanz zurück, für die dies der Fall ist, ohne Rücksicht auf (und ohne Angabe von) welche Partition oder welcher Arbeitsbereich einen bestimmten Lead enthält. Lead-Erstellung und Hinzufügen von Leads zu Listen sind weitere Kontexte, in denen die Lead-Partitionierung Auswirkungen auf die tatsächlichen API-Aufrufe haben kann. Beachten Sie, dass dies bedeutet, dass Partitionen und Arbeitsbereiche möglicherweise nicht die Lösung sind, die Sie für das oben beschriebene Problem der Sandbox-Freigabe benötigen. Wie findet man also heraus, ob das ein Problem für einen ist? Ich habe all dies für hilfreich gehalten: Die Entwickler-Evangelisten engagieren sich für unseren Erfolg bei der Verwendung der APIs. Und wenn es Fragen gibt, sind sie erstaunlich gut darin, Antworten zu finden. - [API-](./getting-started.md). Die Evangelisten haben dieses Thema bereits in die Dokumentation eingebracht und sind als Teil ihres Engagements für unseren Erfolg sehr gut darin, die Dokumentation zu aktualisieren. - Ihre eigenen Testfälle. Auch wenn die Verwendung von Partitionen und Arbeitsbereichen für die Freigabe der Sandbox keine gute Idee ist, ist die Sandbox ein guter Ort, um mit Partitionen und Arbeitsbereichen zu spielen und herauszufinden, ob sie Herausforderungen für Ihre beabsichtigte Verwendung darstellen. (Es ist auch eine gute Möglichkeit, Ihre Fragen für die Evangelisten einzugrenzen, die immer eine gute Idee sind.)

**TIMTOWTDI and Testing** „Es gibt mehr als nur einen Weg“ - das Perl-Programmiermotto gilt in bestimmten Kontexten tatsächlich für die Marketo SOAP API. Ich wollte zum Beispiel die Aktualisierung von Leads mit dem Hinzufügen dieser Leads zu einer Liste kombinieren. Die SOAP-API bietet Ihnen zwei Möglichkeiten dazu: 1. [importToList](/help/soap-api/importtolist.md) + [getImportToListStatus](/help/soap-api/getimporttoliststatus.md). Beim Lesen der Dokumentation ist dies offensichtlich der „normale“ Weg, dies zu tun. Die Tatsache, dass Sie den Status Ihres Importvorgangs abfragen müssen, hat für mich jedoch eine gelbe Markierung ausgelöst. Wollte ich meinen Import wirklich so implementieren? 1. [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) + [listOperation](/help/soap-api/listoperation.md). Dies scheint deutlich weniger elegant als ein unitärer importToList-Aufruf zu sein, ist jedoch nicht auf die Abfrage angewiesen. War es eine praktikable Option? Fälle wie diese sind für die Evangelisten schwer zu bewältigen, weil sie wirklich von der Art der Vorfälle abhängen, mit denen man es zu tun hat und davon, was man genau zu tun versucht. Glücklicherweise sollten Sie, wenn Sie eine robuste Unit-Test-Umgebung eingerichtet haben, diese auch verwenden können, um Fragen wie diese zu untersuchen. In diesem speziellen Fall stellte sich heraus, dass Option 2 für meinen Anwendungsfall besser war als Option 1 - nicht wegen der Abfrage, sondern weil ich auf feldorientierte Einschränkungen bei importToList stieß, und auch weil ich versuchte, Code zu schreiben, der in Kontexten und Instanzen verwendet werden konnte, auf die ich keine Kontrolle hatte. Ihr Anwendungsfall kann jedoch anders sein - Tests sind die einzige Möglichkeit, dies herauszufinden.

**Fazit** Ich glaube nicht, dass das ein riesiges Geheimnis ist. Andererseits wäre ich dem Spiel voraus gewesen, wenn ich das alles gewusst hätte, bevor ich angefangen habe. Ich hoffe, Sie finden es nützlich.

Veröffentlicht am _2015-02-05_ von _David_

## Verwenden der Marketo REST-API mit Boomi: Abrufen und Löschen von Leads aus einer statischen Liste

In Teil 1 dieser Serie habe ich besprochen, wie es möglich war, die REST-API über Boomi mit dem Boomi-HTTP-Connector zu verwenden, insbesondere das Authentifizierungstoken abzurufen, das für den Zugriff auf die REST-API erforderlich ist, und es in einer Prozessvariablen zu speichern. Als Nächstes fangen wir an, Marketo aufzurufen, und in dieser Folge zeige ich Ihnen, wie Sie sowohl [Mehrere Leads nach Listen-ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists) als auch [Leads aus Liste entfernen](/help/rest-api/lead-database.md). Achten Sie besonders auf die Entfernung von Leads aus einer Liste, weil es dort einen sehr „leicht dokumentierten“ und subtilen Aspekt von Boomi am Werk gibt, auf den ich eingehe, wenn wir dort ankommen.

In unserer NÄCHSTEN Folge erweitern wir diese Funktionalität, um interessante Dinge zu tun, wie z. B. Lead-Aktivitäten zu erhalten, aber das ist ein Blog für einen weiteren Tag. In diesem Teil schauen wir uns den zweiten und dritten hervorgehobenen Bereich an. Als Rezension habe ich unten die JSON-Antworten eingefügt, die wir benötigen. Denken Sie daran, dass Sie zum Erstellen eines JSON-Profils in Boomi nur eine Profilkomponente vom Typ JSON erstellen müssen, und klicken Sie auf „Importieren“ und wählen Sie die Datei aus. Boomi macht den Rest, indem er Dinge hochrechnet wie wenn es mehrere IDs geben soll. Beispiel-JSON für [Abrufen mehrerer Leads nach Listen-ID](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Beispiel-JSON für [Leads aus Listenanfrage entfernen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
   "input":[
      {
         "id": ""
      },
      {
         "id": ""
      }
   ]
}
```

Beispiel-JSON für [Entfernen von Leads aus einer Listenantwort](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Die Option Mehrere Leads nach Liste abrufen ** Ziehen Sie einen anderen Connector (GET) in Ihren Prozess und verwenden Sie dieselbe Verbindung wie im vorherigen Artikel. Erstellen Sie einen neuen Vorgang mit dem Namen „Mehrere Leads nach Listen-ID abrufen“ (ich bin ein Stickler für die Konsistenz). Seine Attribute lauten wie folgt: - Anfrageprofil: Keine (dieses verwendet die Anfrage-URL) - Antwortprofiltyp: JSON - Antwortprofil: Erstellen Sie ein neues Profil basierend auf der oben genannten Antwort „Mehrere Leads nach Liste-ID abrufen“. Beachten Sie, dass Sie sie so ändern können, dass sie die gewünschten Felder zurückgibt, nicht nur die aufgelisteten. Beachten Sie, dass das JSON-Antwortprofil wirklich mit der Liste der Felder übereinstimmen sollte, die Sie von der REST-API anfordern, und Sie sollten nur die benötigten Felder anfordern. Im Objekt Prozesseigenschaften haben wir eine Eigenschaft mit der Bezeichnung „Felder“ definiert. Dies ist eine kommagetrennte Liste der Felder, die REST zurückgeben soll. Und das ist die Liste, die mit dem Profil übereinstimmen muss. Content-Typ: text/plain (dies ist nur eine URL-Anfrage) HTTP-Methode: GET (Sie suchen dies in den REST-API-Dokumenten nach, es ist immer aufgeführt) Ressourcenpfad (fügen Sie 5 hinzu) rest/v1/list/ listID (Ersatzvariable) /leads.json?access_token= access_token (Ersatzvariable) &amp;fields= fields (Ersatzvariable). Anschließend können Sie auf der Registerkarte Parameter des Connectors die Variablenwerte eingeben, die zuvor in die Prozesseigenschaften eingefügt wurden. Im nächsten Abschnitt werde ich darüber sprechen, wie Sie vermeiden können, diese manuell zu füllen. Ich werde den Teil des Prozesses überspringen, in dem ich die Antwort für „Mehrere Leads nach Listen-ID abrufen“ einem flachen Dateiprofil zuordnen und auf einen FTP-Server kleben werde, da dies eine einfache Boomi-Funktion ist.

Löschen von Leads aus einer Liste. Dieser ist interessant. Einer meiner Kollegen, [Ken Niwa](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D7429494), hat mir diese nächste Technik beigebracht und es ist ziemlich cool und basiert auf einem Boomi-Artikel mit dem Titel „Wie man eine POST-Anfrage für eine RESTful-Anwendung erstellt“, wie unten gezeigt.  …aber das Wichtigste zuerst. In dem Prozess haben wir das Antwortformular Mehrere Leads nach Liste abrufen und wir müssen dies dem Antwortformular „Mehrere Leads nach Liste abrufen“ zuordnen, das ziemlich einfach ist, indem wir einfach die ID, die wir von den Leads in der ursprünglichen Liste erhalten haben, der ID-Liste zuordnen, die wir an die Löschungs-JSON übergeben. Ziehen Sie als Nächstes einen anderen Connector mit der Aktion „Senden“ und verwenden Sie dieselbe Verbindung Erstellen Sie einen neuen Vorgang mit dem Namen „Leads aus Listenanfrage entfernen“. deren Attribute Anfrageprofil sind: jSON-Inhaltstyp: application/json-Anfrageprofil: [JSON-Profil] Leads aus Listenanfrage entfernen (aus der obigen Datei erstellt) Antwortprofiltyp: jSON-Antwortprofil: [JSON-Profil] Leads aus Listenantwort entfernen (aus der obigen Datei erstellt) Inhaltstyp: application/json-HTTP-Methode: DELETE-Ressourcenpfad (add 4) rest/v1/lists/ listID (Ersatzvariable) /leads.json?access_token= access_token (Ersatzvariable) Hier finden Sie das Interessante an diesem Connector. Wir werden die Parameter auf der Registerkarte „Connector“ NICHT explizit hinzufügen. Stattdessen erstellen wir dynamische Dokumenteigenschaften, die dieselben Namen wie die Ersatzvariablen haben, wie im Artikel angegeben. In diesem Fall sind diese Variablen listID und access_token. Dabei fließt die JSON-Form in den REST-Aufruf und die Parameter werden an der richtigen Stelle in der URL angezeigt. Wir können dies mit dem vorherigen Aufruf nicht tun, da es sich um einen GET und nicht um einen POST handelt. An dieser Stelle haben Sie einen GET- und einen POST-REST-API-Aufruf gesehen und können das Muster für diese REST-Aufrufe sehen. Im nächsten Teil schauen wir uns den Export von Lead-Aktivitäten über die REST-API an, die etwas komplexer ist.

Veröffentlicht am _2015-02-06_ von _John_

## Einbetten eines YouTube-Videos mit Lead-Tracking auf einer Marketo-Landingpage

In einem vorherigen Blogpost habe ich beschrieben, wie Sie Leads in Marketo segmentieren, je nachdem, ob sie ein bestimmtes YouTube-Video gestartet oder abgeschlossen haben. In diesem Blogpost zeigen wir Ihnen, wie Sie die Implementierung aus diesem Beitrag übernehmen und auf einer Marketo-Landingpage verwenden können.

1. Navigieren Sie zu dem Programm in Marketo, in dem Sie die neue Landingpage erstellen möchten. Klicken Sie auf Neues lokales Asset und dann auf Landingpage .
1. Benennen Sie die Landingpage. Seiten-URL zuweisen. Wählen Sie eine Vorlage. Klicken Sie dann auf Erstellen.
1. Nachdem die Landingpage erstellt wurde, klicken Sie auf Entwurf bearbeiten .
1. Ziehen Sie die Schaltfläche HTML aus dem rechten Bereich in die Hauptarbeitsfläche auf der linken Seite.
1. Im Dialogfeld Benutzerdefinierter HTML-Editor wird Folgendes angezeigt. Klicken Sie dann auf Speichern.
1. Passen Sie die Größe eines HTML-Elements an, indem Sie den Umriss des Feldes ziehen. Klicken Sie dann auf Genehmigen und schließen.
1. Testen Sie die Live-Version der Landingpage, indem Sie auf Genehmigte Seite anzeigen klicken. Eine Landingpage mit dem YouTube wird in einem neuen Fenster geöffnet. Das Video wurde gestartet und beendet wird im Aktivitätsprotokoll des Leads angezeigt, wie im ersten und zweiten Screenshot unten dargestellt. Nachdem die Daten in Marketo gespeichert wurden, können Sie eine Smart-Liste und Segment-Leads erstellen, die ein Video gestartet oder beendet haben, wie im folgenden Screenshot gezeigt. Weitere Informationen zur YouTube Iframe-API [ Sie in der API-Dokumentation von YouTube](https://developers.google.com/youtube/iframe_api_reference). Weitere Informationen zu Munchkin ([ Sie in der Entwicklerdokumentation zu Marketo](/help/javascript-api/lead-tracking.md).

Veröffentlicht am _2015-02-09_ von _Murta_

## Webtracking für Einzelseitenanwendungen mit Munchkin

Ein Einzelseiten-Programm ist eine Website, die alle Ressourcen lädt, die zum Navigieren auf der Website beim ersten Laden der Seite erforderlich sind. Wenn ein(e) Benutzende(r) auf einen Link klickt, wird der Inhalt von den ersten Ladedaten der Seite geladen. Für den Benutzer verhält sich die Website wie erwartet, da die URL in der Adressleiste der herkömmlichen Seitennavigation entspricht. Munchkin funktioniert gut mit herkömmlichen Websites, da Munchkin jedes Mal ausgeführt wird, wenn Benutzende eine neue Seite laden. Wenn Sie jedoch mit einer Einzelseitenanwendung keine neue Seite laden, wird Munchkin nur einmal ausgeführt. Der Ansatz, den ich in diesem Beitrag erläutere, besteht darin, zu verfolgen, wann ein Benutzer auf einen Link klickt, und diese Informationen dann an Munchkin zu senden. Wir implementieren dies mithilfe der `clickLink` Munchkin-Funktion. Im Folgenden finden Sie eine Beispielimplementierung in jQuery, die für Klickereignisse an die `clickLink` Munchkin-Methode bindet. Beim Aufrufen der `clickLink` Munchkin-Methode wird der Parameter für die angeklickte URL übergeben.

```javascript
<script>
$("a").on('click', function(event) {
    var urlThatWasClicked = $(this).attr('href');
    Munchkin.munchkinFunction('clickLink', { href: urlThatWasClicked});
});
</script>
```

Veröffentlicht am _2015-02-11_ von _Murta_

## Ändern der Bewertung eines Leads über die REST-API

Angenommen, Sie möchten die Punktzahl eines Leads in Marketo mithilfe der -APIs ändern. Dies ist in Verbindung mit der REST-API mithilfe des Lead-Endpunkts „Erstellen/Aktualisieren“ möglich. Im Folgenden finden Sie ein Codebeispiel in Ruby, das zeigt, wie dieser Aufruf erfolgt.

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "https://AAA-BBB-CCC.mktorest.com"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Im JSON-Text der Anfrage geben wir `updateOnly` als Aktion an. Das bedeutet, dass die Anfrage nur funktioniert, wenn der Lead vorhanden ist. Andernfalls schlägt sie fehl. Wenn Sie einen Lead erstellen möchten, falls noch kein Lead vorhanden ist, geben Sie `createOrUpdate` als Aktion an. Wir verwenden die E-Mail-Adresse des Leads als primäre Kennung, um den Lead-Datensatz in Marketo zu finden. Schließlich geben wir den Wert für die Bewertung des Leads mithilfe der `leadScore` an. Mit dieser Methode ist es möglich, 300 Leads gleichzeitig zu aktualisieren.

Veröffentlicht am _2015-02-19_ von _Murta_

## Hervorheben offener Source-Projekte, die auf der Marketo-Plattform basieren: Dritter Teil

Dies ist der dritte Beitrag in einer fortlaufenden Serie, in der von der Entwickler-Community rund um die Marketo-Plattform erstellte Open-Source-Projekte vorgestellt werden. Wir pflegen [eine Liste im GitHub-Konto von Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) in der wir Client-Bibliotheken und Projekte nachverfolgen, die von der Marketo-Entwickler-Community erstellt wurden. Im Folgenden finden Sie drei Projekte, die um die Marketo-REST-APIs entwickelt wurden. **[Usermind](http://www.usermind.com/) hat [eine Node.js-Client-Bibliothek für die Marketo REST-API erstellt](https://github.com/MadKudu/node-marketo).** **[Arunim Samat](https://github.com/asamat) erstellte [eine Client-Bibliothek in Python für die Marketo REST-API](https://github.com/asamat/python_marketo).** **[Jacques Lemieux von Marketo](https://www.linkedin.com/in/jalemieux) erstellte [eine Client-Bibliothek in Ruby für die Marketo-REST-API.](https://github.com/jalemieux/mkto_rest)** Wir freuen uns, dass auf der Marketo-Plattform mehr Projekte von der Entwickler-Community erstellt werden. Wenn Sie an einem Open-Source-Projekt für die Marketo-Plattform arbeiten, senden [ es über eine Pull-Anfrage an dieses GitHub-Repository](https://github.com/Marketo/Community-Supported-Client-Libraries).

Veröffentlicht am _2015-02-20_ von _Murta_

## Einfügen eines Marketo-Formulars in eine RTP-Kampagne

Viele Marketing-Fachleute sind daran interessiert, ein Marketo-Formular in eine Marketo Real-Time Personalization (RTP)-Kampagne einzufügen. Unabhängig davon, ob es sich um eine Kampagne vom Typ Dialog, In Zone oder Widget RTP handelt, können Sie den HTML-Code des Formulars kopieren und in den Kampagnen-Editor von RTP einfügen. Ich habe diese Beispiele gesehen: - Besucher dazu bringen, sich nach einem zweiten oder dritten Klick auf Ihre Website für Ihren Newsletter anzumelden - Schnelles, effektives Anmeldeformular für Webinare - Herunterladen einer Gated-Fallstudie - Angebot von Leads, die sich in der Vergangenheit abgemeldet haben, ein Formular in der Kampagne auszufüllen und das angeforderte Dankeschön oder den angeforderten Inhalt zu erhalten, was zu einem weniger Klicks führt, um zu Ihren Zielen zu gelangen. Im Folgenden wird erklärt, wie Sie dies tun und ein Marketo Form 2.0 in eine Marketo RTP-Kampagne einbetten können. Im Folgenden finden Sie ein gutes Beispiel von eMarketer, RTP-Benutzer, die einen Schritt weiter gegangen sind und die Besucher nicht zu einer Dankeseite geleitet haben, sondern sich dafür entschieden haben, innerhalb der RTP-Kampagne eine Dankesnachricht anzuzeigen. Der Code für diese Option ist ebenfalls unten aufgeführt. Viel Spaß und ich freue mich über eure Erfahrungen damit!

1. Rechtsklick auf ein genehmigtes Formular. Wählen Sie **Einbettungs-Code.**
1. Kopieren Sie den **Code.**
1. Navigieren Sie in Marketo RTP zu **Kampagnen**.
1. Klicken Sie **NEUE KAMPAGNE ERSTELLEN**.
1. Klicken Sie im Rich-Text-Editor auf das Symbol **HTML**.
1. Fügen Sie den Einbettungs-Code des Formulars in den HTML Source-Editor ein. Klicken Sie auf **Aktualisieren**.
1. Das Formular wird nicht in der Editor-Ansicht angezeigt, Sie können es jedoch in der Vorschau anzeigen, um zu sehen, wie es in einer Kampagne gerendert wird.
1. Klicken Sie **Starten**, um die Kampagne zu starten.

### Hinweis

Alle Änderungen am Formular müssen innerhalb der Marketing-Aktivitäten von Marketo in „Formularentwurf bearbeiten“ vorgenommen werden.

### Ähnliche Artikel

* [Formulare 2.0](/help/javascript-api/forms-api-reference.md)

Veröffentlicht am _2015-12-20_ von _Yanir_

## Hinzufügen einer Zurücksetzen-Schaltfläche zu einem Marketo-Formular

```javascript
<script src="//app-sj01.marketo.com/js/forms2/js/forms2.min.js"></script>
<form id="mktoForm_116"></form>
<script>MktoForms2.loadForm("//app-sj01.marketo.com", "410-XOR-673", 116,
function(form) { form.getFormElem()[0].querySelector('button[type="submit"]').insertAdjacentHTML('afterend','<button type="reset" class="mktoButton">Reset</button>') });
</script>
```

Veröffentlicht am _2015-03-18_ von _Murta_

## Updates vom März 2015

Die [Marketo REST Asset-API wurde in der Version vom März 2015 veröffentlicht](https://developer.adobe.com/marketo-apis/api/asset/). Diese API ermöglicht den Zugriff auf Datei-, Ordner-, Token-, E-Mail- und E-Mail-Vorlagenobjekte von Marketo. Beachten Sie, dass zwei Rollenberechtigungen hinzugefügt wurden, um Zugriff auf die Asset-API-Endpunkte zu gewähren: Nur-Lese-Assets, Lese-Schreib-Assets. Wenn Ihre API-Benutzerrolle vor der Veröffentlichung der Asset-APIs erstellt wurde, müssen Sie eine neue API-Benutzerrolle mit diesen Berechtigungen erstellen, um den Zugriff zu aktivieren. Andernfalls erhalten Sie die Fehlerantwort 603 „Zugriff verweigert“. Zusätzlich zur Veröffentlichung der REST-Asset-API wurden auch vorhandene REST-API-Endpunkte aktualisiert. Der [Zusammenführen von Lead-REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST)API-Endpunkt wurde aktualisiert, um das Zusammenführen mehrerer Leads zu ermöglichen. Der [Endpunkt Campaign REST-API planen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) wurde aktualisiert, um das Klonen einer Kampagne während der Planung einer Kampagne zu ermöglichen.

Veröffentlicht am _2015-03-23_ von _Murta_

## Auslösen von RTP-Kampagnen mit einer Verzögerung

Mit dieser benutzerdefinierten JavaScript können RTP-Benutzer Kampagnen einige Sekunden nach dem Laden der Web-Seite anzeigen. Dies wird für Dialogfeld- und Widget-Kampagnen empfohlen. Er kann verwendet werden, um eine Kampagne nach einer Verzögerung anzuzeigen, sobald der Besucher den regulären Inhalt auf der Seite angesehen hat. Es wird empfohlen, diesen Code nur auf bestimmten Seiten zu implementieren, auf denen die Kampagne(n) angezeigt werden soll(en). Die Verwendung dieses Codes auf allen Seiten wird nicht empfohlen, da er die Leistung beeinträchtigen kann. **Setup-Anweisungen** Der benutzerdefinierte Code sendet ein benutzerdefiniertes RTP-Datenereignis (t=timeOnPage, d. h. t=60) und lädt dann die Kampagne, die diesem Ereignis entspricht. Standardmäßig wird sie nach 60 Sekunden ausgelöst. Sie können sie anpassen, indem Sie den Parameter „sendCustomRTPEvent“ in eine beliebige andere Zahl ändern. Platzieren Sie den Code unmittelbar nach dem standardmäßigen RTP-Code:

```javascript
<script>
function sendCustomRTPEvent(a){
 var eventValue="t="+a;
 setTimeout(function(){
  rtp('send', 'event', {value: eventValue});
  rtp('get', 'campaign',true);
 }, 1000 \* a);
}
sendCustomRTPEvent(60); //Seconds
</script>
```

So richten Sie eine RTP-Kampagne ein, um nach einer Zeitverzögerung zu reagieren: 1. Melden Sie sich bei Ihrem RTP-Konto an 1. Erstellen Sie ein neues Segment 1. Fügen Sie im Abschnitt Segmentereignisse Folgendes hinzu: `t=60`. Ein Besucher kann jedes RTP-Segment nur einmal pro Sitzung abgleichen und sieht daher jede Kampagne nur einmal (es sei denn, es ist auf „Sticky“ gesetzt).

Veröffentlicht am _2015-03-24_ von _Yanir_

## Versionsaktualisierungen April 2015

### Marketo Mobile Engagement SDK v0.3.2

Marketo bietet jetzt Marketing-Automatisierung und Benutzerinteraktion für Mobile Apps. Wenn Sie [Marketo Mobile SDK](/help/mobile/mobile.md) in Ihrer iOS- oder Android-App installieren, können Marketing-Fachleute In-App-Ereignisse überwachen und entsprechende Push-Benachrichtigungen senden.

### Verbesserungen der REST-API

* Benutzerdefinierte Objekte

Es [ neue Endpunkte für benutzerdefinierte Objekte ](/help/rest-api/custom-objects.md), mit denen Sie die Daten, die sich in einem benutzerdefinierten Marketo-Objekt befinden, programmgesteuert auflisten, beschreiben und CRUD durchführen können.

Beachten Sie, dass Rollenberechtigungen hinzugefügt wurden, um Zugriff auf die benutzerdefinierten Objekt-API-Endpunkte zu gewähren: Schreibgeschütztes benutzerdefiniertes Objekt, Lese-/Schreibzugriff benutzerdefiniertes Objekt. Wenn Ihre API-Benutzerrolle vor der Veröffentlichung der APIs für benutzerdefinierte Objekte vorliegt, müssen Sie eine neue API-Benutzerrolle mit diesen Berechtigungen erstellen, um den Zugriff zu aktivieren. Andernfalls erhalten Sie die Fehlerantwort 603 „Zugriff verweigert“.

* Kampagne planen - Programm klonen

In der [Zeitplan-Kampagnen-API“ wurde der neue optionale Parameter „cloneToProgramName“ ](/help/rest-api/data-ingestion.md). Wenn dieser Parameter vorhanden ist, wird das übergeordnete Programm der Kampagne geklont und die neu erstellte Kampagne geplant. Der Parameter gibt den gewünschten Namen für das resultierende Programm an.

Veröffentlicht am _2015-04-28_ von _Travis Kaufman_

## Synchronisieren von E-Mail-Abmeldungen über Instanzen hinweg

Verwalten Sie mehrere Instanzen von Marketo? Es kann schwierig sein, Lead-Informationen über Instanzen hinweg zu synchronisieren. Hier finden Sie eine Möglichkeit, E-Mail-Abmeldungen über Instanzen hinweg mithilfe eines Webhooks zu synchronisieren, der einen externen Webservice aufruft. Der externe Webservice durchsucht jede Instanz nach dem bekannten Lead, der das Abmeldeereignis ausgelöst hat. Wenn ein übereinstimmender Lead gefunden wird, wird das Feld „Abgemeldet“ im entsprechenden Lead-Datensatz aktualisiert. Hier ist ein Diagramm, das die Idee veranschaulicht.  Es liegt an Ihnen, den Webservice zu implementieren, aber der unten stehende Code sollte Ihnen dabei helfen, den Prozess zu starten!

### Externer Webdienst

Der externe Webservice führt für jede Marketo-Instanz, die synchronisiert werden muss, die folgenden Schritte aus:

1. Erstellt eine instanzspezifische REST-API [Endpunkt-URL](/help/rest-api/endpoint-reference.md)
1. Abrufen des Zugriffs-Tokens mit [Identität](/help/rest-api/authentication.md)
1. Ruft eine Liste der Lead-Datensätze ab, die mit der E-Mail[Adresse übereinstimmen, indem „Mehrere Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)
1. Aktualisiert das Feld „Abgemeldet“ jedes Lead-Eintrags mithilfe von Leads erstellen/aktualisieren

Im Folgenden finden Sie ein weiteres Diagramm, das den externen Webservice-Aufruf und die Marketo REST-API-Aufrufe im Detail zeigt.  Der folgende Beispiel-Code ist kein vorkonfigurierter Web-Service. Es handelt sich vielmehr um ein Konsolenmodusprogramm, an das Sie Argumente über die Befehlszeile übergeben können. Hier soll gezeigt werden, wie die entsprechenden Marketo-APIs aufgerufen werden können, um Lead-Datensätze instanzenübergreifend zu aktualisieren. Die Implementierung des Webdiensts bleibt dem Leser als Übung.
**Beispiel-Code** Um den Beispiel-Code einzurichten und auszuführen, müssen Sie ein Java-Projekt in Ihrer bevorzugten IDE erstellen. Danach müssen Sie die folgenden Änderungen vornehmen: 1. Der Beispiel-Code verwendet [json-simple](https://code.google.com/archive/p/json-simple), um JSON-Zeichenfolgen zu analysieren. Fügen Sie die JSON-Simple-JAR-Datei zu Ihrem Java-Projekt hinzu. 1. Der Beispiel-Code hat eine Struktur, die Metadaten für jede Marketo-Instanz enthält. Platzieren Sie die tatsächlichen Werte Ihrer Instanzen wie folgt in die Struktur:

```java
public static String instanceInfo[][] = {
{ "AccountId1", "ClientId1","ClientSecret1" },    // Instance 1 metadata
{ "AccountId2", "ClientId2","ClientSecret2" },    // Instance 2 metadata
{ "AccountId3", "ClientId3","ClientSecret3" }     // Instance 3 metadata
};
```

Die Metadaten für die Instanz finden Sie im Admin-Bedienfeld von Marketo:

* Konto-ID Admin > Integration > Munchkin > Munchkin-Konto
* Client-ID und Client-Geheimnis: Admin > Integration > LaunchPoint > E-Mail-Abmeldung - Synchronisierung > Details anzeigen

Im Beispielcode werden zwei Befehlszeilenargumente verwendet, die die Abfrageparameter „id“ und „email“ für den oben beschriebenen externen Webservice simulieren.

1. args[0] = Konto-ID
1. args[1] = E-Mail-Adresse

Übergeben Sie die tatsächlichen Werte aus Ihrer -Instanz als Argumente an das Programm. Hier finden Sie einen Screenshot zur Projektkonfiguration von IntelliJ IDEA.

**SyncEmailUnsubscribe.java**

```java
package com.marketo;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Scanner;

public class SyncEmailUnsubscribe {
    // Define Marketo instance meta data here.
    // Each row contains three elements: Account Id, Client Id, Client Secret.
    // For example:
    //  public static String instanceData[][] = {
    //    {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    //  };
    //
    public static String instanceData[][] = {
            // ADD YOUR INSTANCE META DATA HERE
    };

    public static void main(String[] args) {
        String accountId = args[0];     // Account id that processed the unsubscribe
        String emailAddress = args[1];  // Email address of lead that unsubscribed

        SyncEmailUnsubscribe seu = new SyncEmailUnsubscribe();

        // Loop through each Marketo instance
        for (int i = 0; i < instanceData.length; i++) {

            // Make sure we skip instance that triggered the webhook
            if (!accountId.equals(instanceData[i][0])) {
                String endpointUrl = String.format("https://%s.mktorest.com", instanceData[i][0]);

                // Generate access token
                String identityUrl = String.format("%s/identity/oauth/token?grant_type=client_credentials&client_id=%s&client_secret=%s", endpointUrl, instanceData[i][1], instanceData[i][2]);
                String token = seu.getToken(identityUrl);

                // Get lead records for given email address (may be duplicates)
                String getLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=email&filterValues=%s", endpointUrl, token, emailAddress);
                String leads = seu.getLeads(getLeadsUrl);

                // Update unsubscribed field in lead record
                String updateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", endpointUrl, token);
                seu.updateLeads(updateLeadsUrl, leads, accountId);
            }
        }

        System.exit(0);
    }

    // Call Identity Service to generate access token
    public String getToken(String url) {
        // Call Identity Service
        String tokenData = getData(url);

        // Convert response into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(tokenData);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }

        // Retrieve access_token
        JSONObject jsonObject = (JSONObject)obj;
        return jsonObject.get("access_token").toString();
    }

    // Call Get Multiple Leads by Filter Type Service to get lead records
    public String getLeads(String url) {
        return getData(url);
    }

    // Call Create/Update Lead Service to update "unsubscribed" flag in lead record
    public void updateLeads(String url, String leads, String account) {
        JSONObject body = composeBody(leads, account);
        if (body != null) {
            postData(url, body);
        }
    }

    // Compose JSON body for Create/Update Leads Service
    private JSONObject composeBody(String leads, String account) {
        JSONObject body = new JSONObject();

        // Convert leads into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(leads);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }
        JSONObject leadsObj = (JSONObject)obj;

        Object success = leadsObj.get("success");
        if (success.equals(true)) {
            body.put("action", "updateOnly");
            body.put("lookupField", "id");
            body.put("asyncProcessing", "true");

            // Build array of lead objects
            JSONArray input = new JSONArray();
            JSONArray result = (JSONArray) leadsObj.get("result");
            Iterator<JSONObject> iterator = result.iterator();
            while (iterator.hasNext()) {
                JSONObject leadIn = (JSONObject)iterator.next();
                JSONObject lead = new JSONObject();
                lead.put("id", leadIn.get("id"));
                lead.put("unsubscribed", "true");
                lead.put("unsubscribedReason", "Cross instance synch triggered by webhook from: " + account);
                input.add(lead);
            }

            body.put("input", input);
        }
        return body;
    }

    // HTTP POST request
    private String postData(String endpoint, JSONObject body) {
        String data = "";
        try {
            // Make request
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("POST");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "application/json");
            urlConn.connect();
            OutputStream os = urlConn.getOutputStream();
            os.write(body.toJSONString().getBytes());
            os.close();
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    // HTTP GET request
    private String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

### Marketo-Setup

Führen Sie für jede Marketo-Instanz, die Sie synchronisieren möchten, die folgenden Schritte aus.

1. Erstellen Sie einen benutzerdefinierten Service mit Rollenberechtigung: Lead lesen/schreiben. Wenn Sie nicht mit dem Erstellen eines benutzerdefinierten Services vertraut sind, klicken Sie [hier](/help/rest-api/custom-services.md).
1. Erstellen Sie einen Webhook, der Ihren externen Webservice aufruft. Wenn Sie mit dem Erstellen von Webhooks nicht vertraut sind, klicken Sie [hier](/help/webhooks/webhooks.md).
1. Webhook als Flussschritt in Smart Campaign hinzufügen.

Der folgende Screenshot zeigt, wie Sie einen Webhook erstellen, um den oben angegebenen Service mithilfe von Token aufzurufen, die die Abfrageparameter automatisch ausfüllen. Nachdem wir nun unseren Webhook erstellt haben, können wir ihn als Flussaktion zu einer Smart-Kampagne hinzufügen.  Die Smart-Liste sollte den Trigger „Abmeldungen von E-Mails“ enthalten.

### Validierung

Um dies alles zu testen, erstellen Sie in mehreren Marketo-Instanzen einen Lead mit derselben E-Mail-Adresse. Stellen Sie sicher, dass Sie Eigentümer der E-Mail-Adresse sind! In einem Instanzvorgang öffnen Sie im Trigger E-Mail-Fluss senden die resultierende E-Mail und klicken Sie auf Abmelden . Um die Ergebnisse zu überprüfen, melden Sie sich bei jeder der anderen Instanzen an und überprüfen Sie die Lead-Datensätze, die mit der E-Mail-Adresse verknüpft sind. Das Kontrollkästchen „Abgemeldet“ sollte aktiviert werden und das Feld „Abmeldegrund“ sollte einen Hinweis mit der Quellkonto-ID enthalten, die die Synchronisierung initiiert hat.

Veröffentlicht am _2015-05-11_ von _David_

## Synchronisieren von Lead-Datenänderungen mithilfe der REST-API

Dieser Beitrag präsentierte ein Codebeispiel, das wiederkehrend ausgeführt werden konnte, um Marketo nach Updates zu fragen. Der Grundgedanke war, mithilfe von Marketo-APIs Änderungen an Lead-Daten zu identifizieren und geänderte Lead-Daten zu extrahieren. Diese Daten können dann zu Synchronisierungszwecken an ein externes System gesendet werden. Das vorgestellte Codebeispiel verwendete unsere SOAP-API. Nun, wir haben eine [neue Art zu gehen](https://www.youtube.com/watch?v=G-7ZJjLy5D8&feature=youtu.be) und diese Art verwendet die [Marketo REST-API](/help/rest-api/rest-api.md). Dieser Beitrag zeigt Ihnen, wie Sie dasselbe Ziel mit zwei REST-Endpunkten erreichen: [Lead-Änderungen abrufen](/help/rest-api/rest-api.md) [Lead nach ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Das Programm umfasst zwei Hauptschritte:

1. Rufen Sie Lead-Änderungen abrufen auf, um eine Liste aller Lead-IDs zu generieren, bei denen entweder bestimmte Lead-Felder geändert wurden oder die während eines bestimmten Zeitraums hinzugefügt wurden.
1. Rufen Sie Lead-ID abrufen für jede Lead-ID in der Liste auf, um Felddaten aus dem Lead-Datensatz abzurufen.

Wir nehmen die in Schritt 2 abgerufenen Daten und formatieren sie für die Verwendung durch ein externes System.  **Programmeingabe** Standardmäßig „kehrt“ das Programm einen Tag nach dem aktuellen Datum zurück, um nach Änderungen zu suchen. Sie können dieses Programm beispielsweise jeden Tag zur gleichen Zeit ausführen. Um einen längeren Zeitraum zu verwenden, können Sie die Anzahl der Tage als Befehlszeilenargument angeben, wodurch das Zeitfenster effektiv vergrößert wird. Das Programm enthält mehrere Variablen, die Sie ändern können: CUSTOM_SERVICE_DATA - Dieses enthält Ihre Marketo [Custom Service](/help/rest-api/custom-services.md)-Daten (Konto-ID, Client-ID, Client-Geheimnis). LEAD_CHANGE_FIELD_FILTER - Enthält eine kommagetrennte Liste von Lead-Feldern, die wir auf Änderungen überprüfen. READ_BATCH_SIZE - Anzahl der gleichzeitig abzurufenden Datensätze. Hier können Sie die Reaktion auf die Körpergröße einstellen. **Programmausgabe** Das Programm sammelt alle geänderten Lead-Datensätze und formatiert sie in JSON als Array von Lead-Objekten wie folgt:

```json
{
    "result": [
        {
            "leadId": "318592",
            "updatedAt": "2015-07-22T19:19:07Z",
            "firstName": "David",
            "lastName": "Everly",
            "email": "<deverly@marketo.com>"
        },

        ...more lead objects here...
    ]
}
```

Die Idee ist, dass Sie dann diese JSON als Anfrage-Payload an einen externen Webservice übergeben können, um die Daten zu synchronisieren. **Programmlogik** Zunächst richten wir unser Zeitfenster ein, erstellen unsere REST-Endpunkt-URLs und erhalten unser Authentifizierungs-Zugriffstoken. Als Nächstes starten wir eine Schleife „Paging-Token abrufen/Lead-Änderungen abrufen“, in der so lange läuft, bis wir den Vorrat an Lead-Änderungen erschöpft haben. Der Zweck dieser Schleife ist es, eine Liste eindeutiger Lead-IDs zu sammeln, damit wir sie später im Programm an „Get Lead by ID“ weitergeben können. In diesem Beispiel weisen wir Get Lead Changes an, nach Änderungen in den folgenden Feldern zu suchen: firstName, lastName, email. Es steht Ihnen frei, für Ihre Zwecke eine beliebige Kombination von Feldern auszuwählen. Lead-Änderungen abrufen gibt „Ergebnis“-Objekte zurück, die eine Aktivitätstyp-ID enthalten, die wir zum Filtern der Ergebnisse verwenden können. Hinweis: Sie können eine Liste der Aktivitätstypen abrufen, indem Sie den REST[Endpunkt „Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getAllActivityTypesUsingGET) aufrufen. Wir interessieren uns für 2 Aktivitätstypen, die zurückgegeben werden: 1. Neuer Lead (12)

```json
{
    "id": 12024682,
    "leadId": 318581,
    "activityDate": "2015-03-17T00:18:41Z",
    "activityTypeId": 12,
    "primaryAttributeValueId": 318581,
    "primaryAttributeValue": "David Everly",
    "attributes": [
        {
            "name": "Created Date",
            "value": "2015-03-16"
        },
        {
            "name": "Source Type",
            "value": "New lead"
        }
    ]
}
```

1. Datenwert ändern (13) Wir können feststellen, welches Feld geändert wurde, indem wir die Eigenschaft „name“ in der Antwort „Datenwert ändern“ überprüfen.

```json
{
    "id": 12024689,
    "leadId": 318581,
    "activityDate": "2015-03-17T22:58:18Z",
    "activityTypeId": 13,
    "fields": [
        {
            "id": 31,
            "name": "lastName",
            "newValue": "Evely",
            "oldValue": "Everly"
        }
    ],
    "attributes": [
        {
            "name": "Source",
            "value": "Web form fillout"
        }
    ]
}
```

Wenn einer dieser beiden Aktivitätstypen zurückgegeben wird, speichern wir die zugehörige Lead-ID in einer Liste. Sobald wir unsere Liste haben, können wir sie durchlaufen, indem wir für jedes Element Lead nach ID abrufen. Dadurch werden die neuesten Lead-Daten für jeden Lead in der Liste abgerufen. Für dieses Beispiel rufen wir die folgenden Lead-Felder ab: `leadId`, `updatedAt`, `firstName`, `lastName` und `email`. Es steht Ihnen frei, für Ihre Zwecke eine beliebige Kombination von Feldern auszuwählen. Dies geschieht durch Angabe des Feldparameters , um den Lead anhand der ID abzurufen. Und schließlich werden die JSON-Ergebnisse wie oben beschrieben als Array von Lead-Objekten kodiert.

**Programm-Code**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadChanges {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            {INSERT YOUR CUSTOM SERVICE DATA HERE};

    // Lead fields that we are interested in
    private static final String LEAD_CHANGE_FIELD_FILTER = "firstName,lastName,email";

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Activity type ids that we are interested in
    private static final int ACTIVITY_TYPE_ID_NEW_LEAD = 12;
    private static final int ACTIVITY_TYPE_ID_CHANGE_DATA_VALE = 13;

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String leadChangesUrl = String.format("%s/rest/v1/activities/leadchanges.json?access_token=%s&fields=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_CHANGE_FIELD_FILTER, READ_BATCH_SIZE);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        HashSet leadIdList = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {

            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;

            do {
                moreResult = false;

                // Call Get Lead Changes API
                JsonObject leadChangesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadChangesUrl, nextPageToken)));
                if (leadChangesObj.get("success").asBoolean()) {
                    moreResult = leadChangesObj.get("moreResult").asBoolean();
                    nextPageToken = leadChangesObj.get("nextPageToken").asString();

                    if (leadChangesObj.get("result") != null) {
                        JsonArray resultAry = leadChangesObj.get("result").asArray();
                        for (JsonValue resultObj : resultAry) {
                            int activityTypeId = resultObj.asObject().get("activityTypeId").asInt();


                            // Store lead ids for later use
                            boolean storeThisId = false;
                            if (activityTypeId == ACTIVITY_TYPE_ID_NEW_LEAD) {
                                storeThisId = true;
                            } else if (activityTypeId == ACTIVITY_TYPE_ID_CHANGE_DATA_VALE) {
                                // See if any of the changed fields are of interest to us
                                JsonArray fieldsAry = resultObj.asObject().get("fields").asArray();
                                for (JsonValue fieldsObj : fieldsAry) {
                                    String name = fieldsObj.asObject().get("name").asString();
                                    if (LEAD_CHANGE_FIELD_FILTER.contains(name)) {
                                        storeThisId = true;
                                    }
                                }

                            }

                            if (storeThisId) {
                                leadIdList.add(resultObj.asObject().get("leadId").toString());
                            }
                        }
                    }
                }

            } while (moreResult);
        }

        JsonObject result = new JsonObject();
        JsonArray leads = new JsonArray();

        for (Object o : leadIdList) {
            String leadId = o.toString();

            // Compose Get Lead by Id URL
            String getLeadUrl = String.format("%s/rest/v1/lead/%s.json?access_token=%s",
                    baseUrl, leadId, accessToken);

            // Call Get Lead by Id API
            JsonObject leadObj = JsonObject.readFrom(getData(getLeadUrl));
            if (leadObj.get("success").asBoolean()) {
                if (leadObj.get("result") != null) {
                    JsonArray resultAry = leadObj.get("result").asArray();
                    for (JsonValue resultObj : resultAry) {

                        // Create lead object
                        JsonObject lead = new JsonObject();
                        lead.add("leadId", leadId);
                        lead.add("updatedAt", resultObj.asObject().get("updatedAt").asString());
                        lead.add("firstName", resultObj.asObject().get("firstName").asString());
                        lead.add("lastName", resultObj.asObject().get("lastName").asString());
                        lead.add("email", resultObj.asObject().get("email").asString());

                        // Add lead object to leads array
                        leads.add(lead);
                    }
                }
            }
        }

        // Add leads array to result object
        result.add("result", leads);

        // Print out result object
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Da haben Sie es also[ „Das Leben ist ein fröhliches Lied](https://www.youtube.com/watch?v=zFaBwZDywLk). Viel Spaß!

Veröffentlicht am _2015-07-31_ von _David_

## Versionsaktualisierungen Mai 2015

### REST-API

* Opportunity-API. Es [ neue Opportunity](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)Endpunkte eingeführt, mit denen Sie die Daten in einem Opportunity-Objekt in Marketo programmgesteuert auflisten, beschreiben und CRUD nutzen können.

Hinweis: Rollenberechtigungen wurden hinzugefügt, um Zugriff auf die Opportunity-Endpunkte zu gewähren: Schreibgeschützte Opportunity, Lese- und Schreibgelegenheit. Wenn Ihre API-Benutzerrolle vor der Veröffentlichung der Opportunity-APIs veröffentlicht wurde, müssen Sie eine neue API-Benutzerrolle mit diesen Berechtigungen erstellen, um den Zugriff zu aktivieren. Andernfalls erhalten Sie die Fehlerantwort 603 „Zugriff verweigert“.

* Asset-API - Ausschnitte. Es [ neue Asset-Endpunkte für Snippets ](https://developer.adobe.com/marketo-apis/api/asset/#snippet_endpoints), mit denen Sie Snippet-Objekte programmgesteuert bearbeiten können. Snippets können als dynamische Inhaltsbausteine in E-Mails und Landingpages verwendet werden.
* Leads-API - Lead-Partition aktualisieren. Ein neuer [Lead-Endpunkt für Partitionen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updatePartitionsUsingPOST) wurde hinzugefügt, damit Sie die Partition für einen oder mehrere Leads aktualisieren können.
* Es wurde ein Problem behoben, bei dem bei Lead-bezogenen APIs der Zeitzonenversatz in den Attributen „createdAt“ und „updatedAt“ fehlte.
* Es wurde ein Problem behoben, bei dem der Kampagnenzeitplan nicht den richtigen Fehler-Code zurückgab, wenn die tägliche maximale Anzahl von Aufrufen überschritten wurde.
* Es wurde ein Problem behoben, bei dem „Ordner nach ID abrufen“ manchmal null für die Attribute „Übergeordnet“ und „Beschreibung“ zurückgab.
* Fehlerkorrektur - Bei der Option E-Mail nach ID genehmigen tritt in bestimmten Fällen kein Systemfehler mehr auf.
* Es wurde ein Problem behoben, bei dem das Erstellen eines Tokens nach Ordner-ID in bestimmten Fällen zu einem Token führte, das nicht verwendbar war.

### Echtzeit-Personalization (RTP)

* Rich-Media-Recommendations-API. Der RTP[JavaScript-API wurden neue ](/help/javascript-api/web-personalization.md)Rich-Media-Recommendations“ hinzugefügt. Die Rich-Media-Inhaltsempfehlung interagiert mit Web-Besuchern mit den relevantesten Inhalten, die auf maschinellem Lernen und prädiktiver Analyse basieren. Verbessern Sie Ihre Inhalts-Assets mit Textbeschreibungen und Bildern und betten Sie mehrere Inhaltsempfehlungen in Ihre Website ein.

### SDK für mobile Interaktion

iOS v0.3.4/Android v0.3.3

* Benutzerdefinierte Aktionen. Es wurde die Möglichkeit hinzugefügt, Benutzerinteraktionen durch Senden benutzerdefinierter Aktionen zu verfolgen. Weitere Informationen finden Sie unter „Senden benutzerdefinierter Aktionen“ [hier](/help/mobile/mobile.md).
* Die Methode trackPushNotification wird nicht mehr unterstützt.

Veröffentlicht am _2015-05-26_ von _David_

## Updates vom Juni 2015

### REST-API

* Unternehmens-API

Es [ neue ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies) (Unternehmensendpunkte) eingeführt, mit denen Sie die in einem Marketo-Unternehmensobjekt enthaltenen Daten programmgesteuert auflisten, beschreiben und CRUD nutzen können.

Hinweis: Rollenberechtigungen wurden hinzugefügt, um Zugriff auf die Programmendpunkte zu gewähren: Unternehmen mit Lesezugriff, Unternehmen mit Lese-/Schreibzugriff. Wenn Ihre API-Benutzerrolle älter ist als die Veröffentlichung der Unternehmens-APIs, müssen Sie Ihre API-Benutzerrolle mit diesen Berechtigungen aktualisieren, um den Zugriff zu aktivieren. Andernfalls erhalten Sie die Fehlerantwort 603 „Zugriff verweigert“

* Asset-API - Programme

Die Asset-API wurde aktualisiert, um sowohl Programmordner als auch Programmordner zu unterstützen. Mehrere Asset-APIs hatten in der Anfrage eine Ordner-ID in Form einer Ganzzahl akzeptiert. Die Ordner-ID kann je nach API als Teil des URI oder als Teil des Anfragetexts übergeben werden.

Jetzt müssen Sie den Ordnertyp zusammen mit der Ordner-ID angeben. Der Ordnertyp lautet entweder „Programm“ oder „Ordner“. „Ordner“ gibt einen Anwendungsordner an, während „Programm“ einen Programmordner angibt. Der Ordnertyp wird je nach aufzurufender API auf zwei verschiedene Arten angegeben:

1. Verwenden Sie die Datenstruktur „FolderIdType“. FolderIdType ist ein einfacher JSON-Block, der die ID und das Typpaar wie folgt enthält:

`{ "id" : **id**, "type" : "**type**" }`

Dabei **id** die Ordner-ID und &quot;**type**&quot; der Ordnertyp. Zulässige Werte für &quot;**type**&quot; sind „Folder“ oder „Program“.

Beispiel - Ordner erstellen

`POST /asset/v1/folders.json`

`parent={"id":416,"type":"Folder"}&name=Test Folder&description=This is a test folder`

1. Verwenden Sie den vorhandenen ID-Parameter und geben Sie dessen Typ mithilfe eines Abfrageparameters „type“ an.

Beispiel - Ordner nach ID abrufen

`GET /rest/asset/v1/folder/1016.json?type=Program`

Alle API-Antworten, die eine Ordner-ID im Ergebnisobjekt enthalten hatten, enthalten jetzt auch ein folderId-Attribut, dessen Wert ein FolderIdType ist. Damit kann der Ordnertyp für eine bestimmte Ordner-ID bestimmt werden.

Beispiel - Ordner nach Namen abrufen

`GET /rest/asset/v1/folder/byName.json?name=Social Media`

```json
"result": [
{
...

"folderId": {"id":341, "type": "Program"},
...

"id":"341"
}
]
```

Um den Ordnertyp für eine bestimmte ID zu ermitteln, können Sie die API [Ordner durchsuchen](https://developer.adobe.com/marketo-apis/api/asset/browse-folders/) verwenden.

Das Attribut „type“ in API-Antworten wurde in „folderType“ umbenannt. Dadurch soll eine Verwechslung mit dem in FolderIdType enthaltenen Element „type“ vermieden werden.

Zum Beispiel von diesem:

&quot;**type**&quot;:„Marketing-Ordner“

... wie folgt ändern:

&quot;**folderType**&quot;: „Marketing-Ordner“

### SDK für mobile Interaktion

iOS 0.3.5

* Es wurde ein Problem behoben, bei dem das Dialogfeld Testgerät festlegen im Haupt-Thread ausgeführt wurde. [MOB-638]
* Fehlerhandhabung für den Fall hinzugefügt, dass das Testgerät nicht registriert werden kann. [MOB-639]

Android 0.3.3

* Das Android:configChanges-Attribut wurde zum AndroidManifest.xml-`<activity>` hinzugefügt, damit das Fortschrittsdialogfeld nicht verworfen wird, wenn Sie ein Testgerät hinzugefügt und die Ausrichtung geändert haben. [MOB-687]

Veröffentlicht am _2015-06-30_ von _David_

## In-App Web Personalization (Beta) mit der RTP-API

Einige unserer Kunden bieten Web-App-Lösungen für ihre Benutzer an und wir erhalten Anfragen, ob sie Marketo Real-Time Personalization (RTP) in ihrer gesicherten Web-App-Umgebung verwenden können. Die Antwort lautet ja! Wir haben eine API für In-App-Messaging veröffentlicht, mit der Sie Inhalte personalisieren und Marketing-Aktivitäten wie Webinare, neue Funktionsveröffentlichungen, Upsell-Aktionen sowie die Interaktion mit Ihren Benutzern auf der Grundlage Ihrer Web-App-Daten bewerben können. Beispiel: Personalisieren von In-App-Inhalten für:

* Testangebote basierend auf der Benutzeraktivität
* Verschiedene Abonnementtypen (Upsell-, Crosssell- oder Webinar-Schulung)
* Neue Funktionen, die für die Benutzeraktivität relevant sind

**Anwendungsbeispiel:** Customer Success-Team von Marketo verwendet In-App Web Personalization, um mit bestimmten Abonnementtypen (Spark, Standard, Select oder Enterprise) mit personalisierten Inhalten zu kommunizieren, um sicherzustellen, dass sie progressive Kampagnen sehen und In-App-Benutzer basierend auf ihrer Interaktion fördern. Sehen wir uns an, wie dies für einen Benutzer mit einem Enterprise-Abonnementtyp geschehen kann. **Voraussetzung** Grundlegendes zur [RTP User Context API](/help/javascript-api/web-personalization.md). **Benutzerkontext-API aktivieren** Fordern Sie beim Marketo-Support an, die Benutzerkontext-API für Ihr RTP-Konto zu aktivieren. **Benutzerdefinierte Variable festlegen** In RTP stehen fünf benutzerdefinierte Variablenslot zum Senden von Daten an zur Verfügung. In diesem Beispiel senden wir ein Benutzerabonnement vom Typ „Enterprise“ an die benutzerdefinierte Variable 1.

`rtp('set', 'customVar1', 'Enterprise');`

**Neues RTP-Segment erstellen** Wechseln Sie zu **Segmente** und klicken Sie auf **NEU ERSTELLEN**.

1. Ziehen Sie den Filter **User Context API** in den Segment Builder.
1. Wählen Sie die **Benutzerdefinierte Variable 1** (Abonnementtyp) aus, **„Wert** ist **Enterprise**.

**Kampagne basierend auf dem Verlauf früherer Besuche anzeigen** Um dem Besucher eine andere Kampagne zu zeigen, wenn er bereits bei einem vorherigen Besuch auf eine Kampagne geklickt hat.

1. Klicken Sie in **User Context API** auf **(+)**, um ein weiteres Benutzerkontext-API-Attribut hinzuzufügen
1. Fügen Sie den **Operator (UND oder ODER) hinzu.**
1. Wählen Sie **Kampagnen - angeklickt aus.** Legen Sie **Kampagnen-ID** auf die ID der Kampagne fest. (Wie Sie die Kampagnen-ID finden, erfahren Sie in dem Hinweis unten.)
1. Klicken Sie auf **SPEICHERN UND DEFINIEREN**, um die kreative Kampagne zu erstellen.

Insgesamt stimmt dieses Segment überein, wenn ein Besucher mit der benutzerdefinierten Variablen (Abonnementtyp) verknüpft ist, die Enterprise entspricht, und wenn er bei einem vorherigen Besuch auf die Kampagne geklickt hat (ID: 5390). Der nächste Schritt besteht darin, eine personalisierte Kampagne für dieses Segment zu definieren. Der folgende Screenshot zeigt eine RTP-Dialogfeldkampagne (unten links), die auf der Seite „My Marketo&quot; erscheint, um ein Webinar für Enterprise-Benutzer zu bewerben.  **HINWEIS:** **Auffinden der Kampagnen-ID** Wechseln Sie zu **Kampagnen**, bewegen Sie den Mauszeiger über **Kampagnenname**, um die Kampagnen-ID zu finden.
Veröffentlicht am _2015-06-17_ von _David_

## Senden von Transaktions-E-Mails mit der Marketo REST-API: Teil 1

Es gibt einige Konfigurationsanforderungen in Marketo, um den erforderlichen Aufruf mit der Marketo REST-API auszuführen.

* Die Empfängerin bzw. der Empfänger muss einen Datensatz in Marketo haben
* Es muss eine Transaktions-E-Mail in Ihrer Marketo-Instanz erstellt und genehmigt sein.
* Es muss eine aktive Trigger-Kampagne mit der angeforderten Kampagne (Source: Web Service API) geben, die zum Senden der E-Mail eingerichtet ist

Zunächst [E-Mail erstellen und ](https://experienceleague.adobe.com/de/docs/marketo/using/home). Wenn es sich bei der E-Mail wirklich um eine Transaktions-E-Mail handelt, müssen Sie sie wahrscheinlich für einsatzbereit festlegen, aber sicherstellen, dass sie rechtlich als funktionsfähig eingestuft wird. Dies wird über den Bildschirm Bearbeiten unter E-Mail-Aktionen > E-Mail-Einstellungen konfiguriert. Genehmigen Sie ihn, und wir sind bereit, unsere Kampagne zu erstellen. Wenn Sie mit dem Erstellen von Kampagnen noch nicht vertraut sind, lesen Sie den Artikel [Erstellen einer neuen intelligenten Kampagne](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign) auf docs.marketo.com. Nachdem Sie Ihre Kampagne erstellt haben, müssen wir diese Schritte durchlaufen. Konfigurieren Sie Ihre Smart-Liste mit dem Trigger Kampagne ist angefordert : Jetzt müssen wir den Fluss so konfigurieren, dass er auf den Schritt „E-Mail senden“ zu unserer E-Mail verweist. Vor der Aktivierung müssen Sie einige Einstellungen auf der Registerkarte Zeitplan festlegen. Wenn diese E-Mail nur einmal an einen bestimmten Datensatz gesendet werden soll, lassen Sie die Qualifizierungseinstellungen unverändert. Wenn sie die E-Mail jedoch mehrmals erhalten müssen, sollten Sie dies entweder auf jedes Mal oder auf eine der verfügbaren Kadenzen anpassen. Jetzt sind wir bereit, zu aktivieren.

### API-Aufrufe senden

**Hinweis:** In den unten stehenden Java-Beispielen verwenden wir das Paket „minimal-json“, um JSON-Darstellungen in unserem Code zu verarbeiten. Weitere Informationen zu diesem Projekt finden Sie hier: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) Der erste Teil des Versands einer Transaktions-E-Mail über die API besteht darin sicherzustellen, dass in Ihrer Marketo-Instanz ein Datensatz mit der entsprechenden E-Mail-Adresse vorhanden ist und dass wir Zugriff auf die Lead-ID haben. Für diesen Beitrag gehen wir davon aus, dass sich die E-Mail-Adressen bereits in Marketo befinden und wir nur die ID des Datensatzes abrufen müssen. Zu diesem Zweck verwenden wir den Aufruf [Mehrere Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) und wir verwenden einen Teil des Java-Codes aus dem vorherigen Beitrag in wieder. Sehen wir uns nun unsere Hauptmethode für die Anfrage der Kampagne an:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("<requestCampaign.test@marketo.com>");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Um diese Ergebnisse aus der JsonObject-Antwort von leadRequest zu erhalten, müssen wir Code schreiben. Um das erste Ergebnis im Array abzurufen, müssen wir das Array aus dem JsonObject extrahieren und das -Objekt bei 0 indizieren lassen:

`JsonArray leadsResult = leadsRequest.getData().get("result").asArray();`
`int leadId = leadsResult.get(0).asObject().get("id").asInt();`

Von hier aus müssen wir nur noch den Aufruf Kampagne anfragen ausführen. Zu diesem Zweck sind die erforderlichen Parameter die ID in der URL der Anfrage und ein Array von JSON-Objekten, die ein Element „id“ enthalten. Sehen wir uns den Code dafür an:

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
 private String endpoint;
 private Auth auth;
 public ArrayList leads = new ArrayList();
 public ArrayList tokens = new ArrayList();

 public RequestCampaign(Auth auth, int campaignId) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
 }
 public RequestCampaign setLeads(ArrayList leads) {
  this.leads = leads;
  return this;
 }
 public RequestCampaign addLead(int lead){
  leads.add(lead);
  return this;
 }
 public RequestCampaign setTokens(ArrayList tokens) {
  this.tokens = tokens;
  return this;
 }
 public RequestCampaign addToken(String tokenKey, String val){
  JsonObject jo = new JsonObject().add("name", tokenKey);
  jo.add("value", val);
  tokens.add(jo);
  return this;
 }
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   System.out.println("Executing RequestCampaign calln" + "Endpoint: " + s + "nRequest Body:n"  + requestBody);
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   System.out.println("Result:n" + result);
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonObject input = new JsonObject();
  JsonArray leadsArray = new JsonArray();
  for (int lead : leads) {
   JsonObject jo = new JsonObject().add("id", lead);
   leadsArray.add(jo);
  }
  input.add("leads", leadsArray);
  JsonArray tokensArray = new JsonArray();
  for (JsonObject jo : tokens) {
   tokensArray.add(jo);
  }
  input.add("tokens", tokensArray);
  requestBody.add("input", input);
  return requestBody;
 }

}
```

Diese Klasse verfügt über einen Konstruktor, der eine Authentifizierung akzeptiert, und die ID der Kampagne. Leads werden dem Objekt entweder durch Übergabe eines ArrayList-`<Integer>` mit den IDs der zu setzenden Datensätze oder durch Verwendung von addLead hinzugefügt, das eine Ganzzahl benötigt und an die vorhandene ArrayList in der Leads-Eigenschaft anhängt. Um den API-Aufruf zum Übergeben der Lead-Datensätze an die Kampagne Trigger, muss postData aufgerufen werden, wodurch ein JsonObject zurückgegeben wird, das die Antwortdaten aus der Anfrage enthält. Wenn eine Anfragekampagne aufgerufen wird, wird jeder Lead, der an den Aufruf weitergeleitet wird, von der Target-Trigger-Kampagne in Marketo verarbeitet und ihm wird die zuvor erstellte E-Mail gesendet. Herzlichen Glückwunsch! Sie haben über die Marketo-REST-API eine E-Mail ausgelöst. Sehen Sie sich Teil 2 an, in dem wir uns mit der dynamischen Anpassung des Inhalts einer E-Mail über die Anfragekampagne befassen.

Veröffentlicht am _2015-07-17_ von _Kenny_

## Authentifizieren und Abrufen von Lead-Daten aus Marketo mit der REST-API

**Hinweis:** In den unten stehenden Java-Beispielen verwenden wir das Paket „minimal-json“, um JSON-Darstellungen in unserem Code zu verarbeiten. Weitere Informationen zu diesem Projekt finden Sie hier: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) Eine der häufigsten Anforderungen bei der Integration mit Marketo ist das Abrufen von Lead-Daten. Die meisten, wenn nicht alle Integrationen erfordern entweder das Abrufen oder Senden von Lead-Daten aus einem Marketo-Abonnement. Daher werfen wir heute einen Blick auf eine grundlegende Aufgabe zum Abrufen von Lead-Informationen, indem wir uns mit einem Abonnement [authentifizieren](/help/rest-api/authentication.md) und dann Lead-Daten daraus abrufen. Um unsere Leads abzurufen, müssen wir uns zunächst mit der Marketo-Zielinstanz authentifizieren und dabei OAuth 2.0 verwenden. Es gibt drei Informationen, die wir für die Authentifizierung bei Marketo benötigen: die Client-ID, den geheimen Client-Schlüssel und den Host der Marketo-Instanz. Hier ist die Klasse, die wir zur Authentifizierung verwenden:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Auth {
 protected String marketoInstance; //Instance Host obtained from Admin -> Web Service
 private String clientId; //clientId obtained from Admin -> Launchpoint -> View Details for selected service
 private String clientSecret; //clientSecret obtained from Admin -> Launchpoint -> View Details for selected service
 private String idEndpoint; //idEndpoint constructed to authenticate with service when constructor is used
 private String token; //token is stored for reuse until expiration
 private Long expiry; //used to store time of expiration

 //Creates an instance of Auth which is used to Authenticate with a particular service on a particular instance
 public Auth(String id, String secret, String instanceUrl) {
  this.clientId = id;
  this.clientSecret = secret;
  this.marketoInstance = instanceUrl;
  this.idEndpoint = marketoInstance + "/identity/oauth/token?grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
 }
 //The only public method of Auth, used to check if the current value of Token is valid, and then to retrieve a new one if it is not
 public String getToken(){
  long now  = System.currentTimeMillis();
  if (expiry == null || expiry < now){
   System.out.println("Token is empty or expired. Trying new authentication");
   JsonObject jo = getData();
   System.out.println("Got Authentication Response: " + jo);
   this.token = jo.get("access_token").asString();
                        //expires_in is reported as seconds, set expiry to system time in ms + expires \* 1000
   this.expiry = System.currentTimeMillis() + (jo.get("expires_in").asLong() \* 1000);
  }
  return this.token;
 }
 //Executes the authentication request
 private JsonObject getData(){
  JsonObject jsonObject = null;
  try {
   URL url = new URL(idEndpoint);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
   urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "application/json");
            System.out.println("Trying to authenticate with " + idEndpoint);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                Reader reader = new InputStreamReader(inStream);
                jsonObject = JsonObject.readFrom(reader);
            }else {
                throw new IOException("Status: " + responseCode);
            }
  } catch (MalformedURLException e) {
   e.printStackTrace();
  }catch (IOException e) {
            e.printStackTrace();
        }
  return jsonObject;
 }
}
```

Mit diesem Code können Sie eine Instanz der Authentifizierung mit Ihrer Client-ID, Ihrem Client-Geheimnis und dem Host (als marketoInstance) von Admin -> Launchpoint (ID und Geheimnis) und Admin -> Web Services (Host) erstellen. Sie stellt die getToken-Methode zur Verfügung, die prüft, ob das aktuell gespeicherte Token null oder abgelaufen ist, und dann entweder das vorhandene Token zurückgibt oder eine neue Authentifizierungsanfrage ausführt und dann das neue Token aus dem Element „access_token“ der JSON-Antwort zurückgibt. Nachdem Sie sich jetzt bei Ihrer Marketo-Instanz authentifizieren können, besteht der nächste Schritt darin, unsere Leads abzurufen. Wir verwenden diese Klasse:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Leads {
 private StringBuilder endpoint;
 private Auth auth;
 public String filterType;
 public ArrayList filterValues = new ArrayList();
 public Integer batchSize;
 public String nextPageToken;
 public ArrayList fields = new ArrayList();

 public Leads(Auth a) {
  this.auth = a;
  this.endpoint = new StringBuilder(this.auth.marketoInstance + "/rest/v1/leads.json");
 }
 public Leads setFilterType(String filterType) {
  this.filterType = filterType;
  return this;
 }
 public Leads setFilterValues(ArrayList filterValues) {
  this.filterValues = filterValues;
  return this;
 }
 public Leads addFilterValue(String filterVal) {
  this.filterValues.add(filterVal);
  return this;
 }
 public Leads setBatchSize(Integer batchSize) {
  this.batchSize = batchSize;
  return this;
 }
 public Leads setNextPageToken(String nextPageToken) {
  this.nextPageToken = nextPageToken;
  return this;
 }
 public Leads setFields(ArrayList fields) {
  this.fields = fields;
  return this;
 }

 public JsonObject getData() {
        JsonObject result = null;
        try {
         endpoint.append("?access_token=" + auth.getToken() + "&filterType=" + filterType + "&filterValues=" + csvString(filterValues));
         if (batchSize != null && batchSize > 0 && batchSize <= 300){
             endpoint.append("&batchSize=" + batchSize);
            }
            if (nextPageToken != null){
             endpoint.append("&nextPageToken=" + nextPageToken);
            }
            if (fields != null){
             endpoint.append("&fields=" + csvString(fields));
            }
            URL url = new URL(endpoint.toString());
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "text/json");
            InputStream inStream = urlConn.getInputStream();
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader);
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return result;
    }
 private String csvString(ArrayList fields) {
  StringBuilder fieldCsv = new StringBuilder();
     for (int i = 0; i < fields.size(); i++){
      fieldCsv.append(fields.get(i));
      if (i + 1 != fields.size()){
       fieldCsv.append(",");
      }
     }
  return fieldCsv.toString();
 }
}
```

Diese Klasse verfügt über einen einzelnen Konstruktor, der ein Authentifizierungsobjekt akzeptiert und dann mehrere Setzer für optionale und erforderliche Parameter verfügbar macht. In diesem Fall müssen wir uns wirklich nur damit befassen, den filterType und filterValues so festzulegen, dass Leads nach E-Mail-Adresse abgerufen werden. Deshalb verwenden wir setFilterType für „email“ und addFilterValue für die E-Mail-Adresse, für die wir eine ID abrufen müssen. Wenn Sie Ihre Parameter festgelegt haben, können Sie die getData-Methode verwenden, um ein JSON-Objekt vom Lead-Endpunkt abzurufen, das ein Ergebnis-Array mit einer JSON-Darstellung der abgerufenen Lead-Datensätze enthält.

### Zusammensetzen

Nachdem wir nun den von uns verwendeten Beispiel-Code durchlaufen haben, sehen wir uns ein einfaches Beispiel an, um Leads abzurufen, die einer Test-E-Mail-Adresse <testlead@marketo.com> entsprechen. Dazu müssen wir setFilterType für „email“ und addFilterValue für die E-Mail-Adresse verwenden, für die wir Informationen abrufen müssen. Wenn Sie Ihre Parameter festgelegt haben, können Sie die getData-Methode verwenden, um ein JSON-Objekt vom Lead-Endpunkt abzurufen, das ein Ergebnis-Array mit einer JSON-Darstellung der abgerufenen Lead-Datensätze enthält.

```java
package dev.marketo.blog_leads;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        //Change the credentials here to your own
        Auth auth = new Auth("CHANGE ME", "CHANGE ME", "CHANGE ME");
        //Create and parameterize an instance of Leads
        Leads getLeads = new Leads(auth)
              .setFilterType("email")
              .addFilterValue("<testlead@marketo.com>");
        //get the inner results array of the response
        JsonObject leadsResult = getLeads.getData();
        System.out.println(leadsResult);
    }
}
```

In diesem Hauptmethodenbeispiel erstellen wir eine Instanz von Auth und übergeben sie dann an einen neuen Leads-Konstruktor. Mit setFilterType und addFilterValue konfigurieren wir unsere Instanz von Leads so, dass nur Leads abgerufen werden, die der E-Mail-Adresse &quot;<testlead@marketo.com>&quot; entsprechen. Dieses Beispiel druckt dies auf der Konsole aus:

Token ist leer oder abgelaufen. Neue Authentifizierung wird versucht
Authentifizierung mit <https://299-BYM-827.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id=b417d98f-9289-47d1-a61f-db141bf0267f&client_secret=0DipOvz4h2wP1ANeVjlfwMvECJpo0ZYc> wird versucht
Antwort zur Authentifizierung empfangen: {„access_token“:„ec0f02c0-28ac-4d6c-b7d7-00e47ae85ff1:st&quot;,„token_type“:„bearer“,„expires_in“:538,„scope“:“<apiuser@mktosupport.com>&quot;}
{„requestId“:„14fb6#14e6a7a9ad6“,„result“:[{„id“:1026322,„updatedAt“:„2015-07-07T21:43:25Z“,„lastName“:„Lead“,„email“:“<testlead@marketo.com>&quot;,„createdAt“:„2015-07-07T21:43:25Z“,„firstName“:„Test“},{„id“:1026323,„updated at“:„2015-07-07T21:43:43Z“,„lastName“:„Lead2“,„email“:“<testlead@marketo.com>&quot;,„createdAt“:„2015-07-07T21:43:43Z“,„firstName“:„Test“}],„success“:true}

Jetzt haben wir Lead-Daten, die wir verarbeiten können, auf welche Weise auch immer wir benötigen. Vielen Dank für die Lektüre, und hinterlassen Sie bitte jedes Feedback, das Sie in den Kommentaren haben.

Veröffentlicht am _2015-07-10_ von _Kenny_

## Versionsaktualisierungen Juli 2015

REST-API

* Vertriebspersonen-API

Es [ neue Endpunkte (](/help/rest-api/sales-persons.md)) eingeführt, mit denen Sie die Daten in einem Marketo-Verkaufspersonenobjekt programmgesteuert auflisten, beschreiben und CRUD durchführen können. Darüber hinaus kann ein Vertriebsmitarbeiter einem Lead, einer Opportunity oder einem Unternehmen zugewiesen werden. Dies geschieht durch Angabe eines „externalSalesPersonId“-Attributs beim Aufruf des Endpunkts „Create/Update/Upsert“ für Lead, Opportunity oder Unternehmen.

Hinweis: Rollenberechtigungen wurden hinzugefügt, um Zugriff auf die Programm-Endpunkte zu gewähren: Schreibgeschützter Vertriebsmitarbeiter, Lese-/Schreibzugriff-Vertriebsmitarbeiter. Wenn Ihre API-Benutzerrolle vor der Veröffentlichung der Sales Person-APIs veröffentlicht wurde, müssen Sie Ihre API-Benutzerrolle mit diesen Berechtigungen aktualisieren, um den Zugriff zu aktivieren. Andernfalls erhalten Sie die Fehlerantwort 603 „Zugriff verweigert“.

* Asset-API - Landingpage-Vorlage

Es [ neue Endpunkte für Landingpage](https://developer.adobe.com/marketo-apis/api/asset/#landing_page_templates_endpoints) mit denen Sie die mit einer Landingpage-Vorlage verknüpften Daten programmgesteuert auflisten, erstellen und aktualisieren können.

* Asset-API - Segmente

Es wurden zwei segmentbezogene Endpunkte eingeführt:

[Segmente abrufen](https://developer.adobe.com/marketo-apis/api/asset/get-segments)

[Abrufen der Segmentierung nach ID](https://developer.adobe.com/marketo-apis/api/asset/get-segmentation-by-id)

* Es wurde ein Problem behoben, bei dem beim Abrufen des Ordners nach Name der Parameter „workSpace“ nicht berücksichtigt wurde. [LM-61059]
* An den APIs für benutzerdefinierte Objekte wurden mehrere Leistungsverbesserungen vorgenommen.

Veröffentlicht am _2015-07-17_ von _David_

## Erstellen und Verknüpfen von Leads, Unternehmen und Opportunities mit der Marketo-REST-API

Um die Vorteile von Marketo Analytics optimal nutzen zu können, müssen Sie unbedingt korrekte und zuverlässige Verknüpfungen zwischen Ihrem Lead, Ihrem Unternehmen und Ihren Opportunities erstellen. Wenn Sie keine native CRM-Synchronisierung nutzen, kann die Erstellung dieser Beziehungen einige Schwierigkeiten bereiten, sodass wir heute die einzelnen Schritte durchlaufen.

### Objektbeziehungen

In Marketo gibt es einige wichtige Beziehungen, um Opportunity-Berichte vollständig zu etablieren:

* Leads und Opportunities haben eine Viele-zu-Viele-Beziehung zum OpportunityRole-Objekt.
* OpportunityRole verfügt sowohl über eine Lead-ID als auch über ein externes Opportunity-ID-Feld, um die Beziehung vom Lead zur Opportunity zu erstellen.
* Um sich für einen Smart List-Filter für Opportunities zu qualifizieren, muss ein Lead über eine Opportunity-Rolle verfügen, die mit einer Opportunity verbunden ist.
* Opportunities haben eine Viele-zu-Eins-Beziehung zum Firmenobjekt über das Feld „externalCompanyId“.
* Leads haben über das Feld externalCompanyId eine Eins-zu-Viele-Beziehung zu Unternehmen.
* Opportunities werden einem Programm auf der Grundlage des Akquise-Programms eines Leads oder seiner Mitgliedschaft und seines Erfolgs in einem Programm zugeschrieben (siehe [Grundlagen zur Attribution](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/reporting/revenue-cycle-analytics/revenue-tools/attribution/understanding-attribution)).

Durch den Aufbau dieser Beziehungen in Ihrer Lead-Datenbank können Sie Marketo Analytics vollständig nutzen und den Einfluss sehen, den Ihre Programme auf die Erstellung von Chancen und die Gewinnraten haben.

### Firmen

Der einfachste Weg, diese Beziehungen aufzubauen, besteht darin, mit der Unternehmensgründung zu beginnen. Dadurch wird sichergestellt, dass wir unsere Opportunities während der Erstellung an externalCompanyId weitergeben können, anstatt zusätzliche API-Aufrufe durchführen zu müssen, um Opportunities nach ihrer Erstellung zu aktualisieren. Abhängig von der vorhandenen Konfiguration kann dies ein notwendiger Schritt sein oder nicht, aber neue Leads und Kontakte mit verbundenen Unternehmen müssen diese Datensätze zu Ihrer Marketo-Instanz hinzugefügt werden, damit die Beziehungen erstellt werden können. Sehen wir uns also einen Code an, um Firmendatensätze zu erstellen.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertCompanies {
 public List<JsonObject> input; //a list of Companies to use for input.  Each must have a member "externalCompanyId".
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters(externalCompanyId), idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertCompanies(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/companies.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertCompanies(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 //adds input to existing list, creates arraylist if it was built without a list
 public UpsertCompanies addCompanies(JsonObject... companies){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : companies) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our company records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertCompanies instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 //sets or replaces existing input with list
 public UpsertCompanies setInput(List input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertCompanies setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertCompanies setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Die Firmen-API bietet zwei Deduplizierungsoptionen, die vom dedupeBy-Parameter in der Anfrage festgelegt werden, „dedupeFields“ und „idField“. Diese können explizit durch Aufruf von Describe Companies abgerufen werden. Wenn dedupeBy nicht festgelegt ist, wird standardmäßig dedupeFields verwendet. Bei Firmendatensätzen entspricht dedupeFields immer „externalCompanyId“, einer beliebigen Zeichenfolge, die von einer externen Quelle festgelegt wird, und idField, das dem Feld „marketoId“ entspricht, einer Ganzzahl, die von Marketo nach der Erstellung generiert und zurückgegeben wird. Je nach Auswahl für dedupeBy muss bei jedem Upsert-Aufruf für einen Firmendatensatz eine der Variablen externalCompanyId oder marketoId enthalten sein. Dieselben Anforderungen gelten für die Opportunity- und Opportunity-Objekt-APIs. Unser Code macht zwei Konstruktoren verfügbar: einer akzeptiert ein einzelnes Argument eines Auth-Objekts und ein anderer akzeptiert Auth und eine Liste von [JsonObject](https://github.com/ralfstx/minimal-json)-Firmendatensätzen. Wenn sie ohne Eingabeliste erstellt wird, müssen Firmendatensätze über die Methode addCompanies hinzugefügt werden, die prüft, ob eine neue ArrayList erstellt wird, wenn die Eingabe null ist, und dann alle JsonObject-Argumente zur Eingabeliste hinzufügt. Im Folgenden finden Sie ein Anwendungsbeispiel:

```java
//Create a new company to associate to
JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
JsonObject companiesResult = upsertCompanies.postData();
System.out.println(companiesResult);
```

Wir erstellen ein einzelnes Unternehmens-JsonObject mit nur einem Feld, `externalCompanyId`, erstellen dann eine Instanz von UpsertCompanies und fügen unser Unternehmen mit `addCompanies` zur Eingabeliste hinzu.

### Opportunitys

Ähnlich wie die Unternehmensobjekte verfügt die Opportunity-API über einen `dedupeBy`, der „dedupeFields“ oder „idField“ akzeptiert und „externalOpportunityId“ bzw. „marketoGUID“ entspricht. Hier ist unser Code, der der UpsertCompanies-Klasse ziemlich ähnlich sieht:

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunities {
 public List<JsonObject> input; //a list of Opportunities to use for input.  Each must have a member "externalopportunityid".  Each can optionally include "externalCompanyId" for company association
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertOpportunities(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunities(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunities addOpportunities(JsonObject... opp){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : opp) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our Opportunity records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunities setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunities setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunities setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Dieselben Konstruktoroptionen werden bereitgestellt, wobei eine Auth- oder `Auth+List<JsonObject>` und eine `addOpportunities` Methode zur Eingabe von JsonObject-Opportunities verwendet werden. Im Folgenden finden Sie ein Anwendungsbeispiel:

```java
//Create some JsonObjects for Opportunity Data
JsonObject opp1 = new JsonObject().add("name", "opportunity1")
    .add("externalopportunityid", "Opportunity1Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");
JsonObject opp2 = new JsonObject().add("name", "opportunity2")
    .add("externalopportunityid", "Opportunity2Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");

//Create an Instance of UpsertOpportunities and POST it
UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                        .setAction("createOnly")
                        .addOpportunities(opp1, opp2);
JsonObject oppsResult = upsertOpps.postData();
System.out.println(oppsResult);
```

Hier erstellen wir zwei Beispiel-Opportunities und geben ihnen dann Werte für die Felder Name, ExterneOpportunityId, ExterneCompanyId und ExterneCreatedDate. Wir haben noch nicht über externalCreatedDate gesprochen, aber es ist wichtig, es zu verwenden, da es als Master-Feld in RCE für den Zeitpunkt der Erstellung einer Opportunity behandelt wird, was es wichtig für die korrekte Attribution macht. Sie können die Geschäftslogik Ihres Unternehmens verwenden, um zu bestimmen, was Sie in diesem Feld eingeben, je nachdem, ob Sie vorhandene Opportunity-Daten aufstocken oder neue Daten spontan erstellen. Wir erstellen unsere Instanz von UpsertOpportunities und fügen dann unsere JsonObjects über addOpportunities hinzu. Nachdem die Instanz konfiguriert ist, können Sie diese mit „postData“ an Marketo pushen und Ihr Ergebnis ausdrucken

### Rollen

Rollen ähneln den beiden vorangehenden Objekten, mit dem Unterschied, dass sie beim Festlegen von dedupeBy auf dedupeFields eine etwas andere Anforderung haben. Rollen erfordern, dass beim Erstellen oder Aktualisieren eines Datensatzes über diese Methode drei Felder enthalten sind: „leadId“, „role“ und „externalOpportunityId“. „role“ kann ein beliebiger Zeichenfolgenwert sein, aber die beiden anderen müssen sich auf eine gültige ID eines Leads bzw. eine gültige ID einer Opportunity beziehen.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunityRoles {
 public List<JsonObject> input; //Array of Opportunity Roles as JsonObjects, must have "leadId", "role" and "externalopprtunityid"
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate, defaults to createOrUpdate if unset
 public String dedupeBy;//select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object

 //Constructs an UpsertOpportunityRoles with Auth, but with no input set
 public UpsertOpportunityRoles(Auth auth) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities/roles.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunityRoles(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunityRoles addRoles(JsonObject... role){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : role) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 //executes the request to Marketo, body will be empty if input is not set
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");//"application/json" content-type is required.
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream();  //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 public JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject();
  JsonArray in = new JsonArray();
  for (JsonObject jo : input) {
   in.add(jo);
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action);
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy);
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunityRoles setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunityRoles setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunityRoles setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Wir folgen einem ähnlichen Muster für die Methode „structors“ und „addRoles“ wie die vorherigen Beispiele. Hier ein Beispiel.

```java
//Create Some opp roles now
JsonObject opp1Role = new JsonObject()
                        .add("role", "Captain")
                        .add("externalopportunityid", opp1.get("externalopportunityid").asString())
                        .add("leadId", 318794);
JsonObject opp2Role = new JsonObject()
                        .add("role", "Commander")
                        .add("externalopportunityid", opp2.get("externalopportunityid").asString())
                        .add("leadId", 318795);

//Create an Instance of UpsertOpportunityRoles and POST it
UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
                        .setAction("createOnly")
                        .addRoles(opp1Role, opp2Role);
JsonObject rolesResult = upsertRoles.postData();
System.out.println(rolesResult);
```

Hier erstellen wir die neuen JsonObjects für unsere 2 Beispielrollen und fügen die erforderlichen deduplizierten Felder hinzu, ziehen die externe Opportunity-ID aus den bereits erstellten Opportunitys und schieben sie dann auf Marketo.

### Alles zusammenbringen

Im Folgenden finden Sie ein vollständiges Beispiel für unsere Hauptmethode:

```java
package dev.marketo.opportunities;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //create an Instance of Auth
        Auth auth = new Auth("CLIENT_ID_CHANGE_ME", "CLIENT_SECRET_CHANGE_ME", "MARKETO_HOST_CHANGE_ME");

        //Create a new company to associate to
        JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
        UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
        JsonObject companiesResult = upsertCompanies.postData();
        System.out.println(companiesResult);

        //Create some JsonObjects for Opportunity Data
        JsonObject opp1 = new JsonObject().add("name", "opportunity1")
           .add("externalopportunityid", "Opportunity1Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");
        JsonObject opp2 = new JsonObject().add("name", "opportunity2")
           .add("externalopportunityid", "Opportunity2Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");

        //Create an Instance of UpsertOpportunities and POST it
        UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                                .setAction("createOnly")
                                .addOpportunities(opp1, opp2);
        JsonObject oppsResult = upsertOpps.postData();
        System.out.println(oppsResult);

        //Create Some opp roles now
        JsonObject opp1Role = new JsonObject()
           .add("role", "Captain")
           .add("externalopportunityid", opp1.get("externalopportunityid").asString())
           .add("leadId", 318794);
        JsonObject opp2Role = new JsonObject()
           .add("role", "Commander")
           .add("externalopportunityid", opp2.get("externalopportunityid").asString())
           .add("leadId", 318795);

        //Create an Instance of UpsertOpportunityRoles and POST it
        UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
           .setAction("createOnly")
           .addRoles(opp1Role, opp2Role);
        JsonObject rolesResult = upsertRoles.postData();
        System.out.println(rolesResult);

    }
}
```

Sie können die Reihenfolge beim Erstellen von Unternehmen, Chancen und Rollen sehen. Jetzt können Sie Ihre Unternehmens- und Opportunity-Daten mit Marketo synchronisieren.

Veröffentlicht am _2015-08-07_ von _Kenny_

## Senden von Transaktions-E-Mails mit der Marketo REST-API: Teil 2, Benutzerdefinierte Inhalte

Diese Woche zeigen wir Ihnen, wie Sie dynamische Inhalte über den API-Aufruf von Request Campaign an Ihre E-Mails übergeben können. Campaign-Anfrage ermöglicht nicht nur das externe Auslösen von E-Mails, sondern Sie können auch den Inhalt von &quot;[ Token“ ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program) einer E-Mail ersetzen. Meine Token sind wiederverwendbare Inhalte, die auf Programm- oder Marketing-Ordnerebene angepasst werden können. Diese können auch nur als Platzhalter vorhanden sein, die durch Ihren Kampagnenanforderungsaufruf ersetzt werden.

### E-Mail erstellen

Um unseren Inhalt anzupassen, müssen wir zunächst ein [Programm“ ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program) eine [E-Mail](https://experienceleague.adobe.com/de/docs/marketo/using/home) in Marketo konfigurieren. Um unsere benutzerdefinierten Inhalte zu generieren, müssen wir Token innerhalb des Programms erstellen und sie dann in der E-Mail platzieren, die wir senden werden. Der Einfachheit halber verwenden wir in diesem Beispiel nur ein Token, aber Sie können eine beliebige Anzahl von Token in einer E-Mail ersetzen, in der Absender-E-Mail, im Absendernamen, in der Antwortadresse oder in einem beliebigen Teil des Inhalts in der E-Mail. Erstellen wir also ein Rich-Text-Token für die Ersetzung und nennen es „bodyReplacement“. Rich-Text ermöglicht es uns, alle Inhalte im Token durch beliebige HTML zu ersetzen, die wir eingeben möchten. Token können nicht gespeichert werden, wenn sie leer sind. Fügen Sie daher hier Platzhaltertext ein. Jetzt müssen wir unser Token in die E-Mail einfügen: Dieses Token ist jetzt über einen Aufruf der Kampagne anfordern zum Ersatz verfügbar. Dieses Token kann so einfach sein wie eine einzelne Textzeile, die pro E-Mail ersetzt werden muss, oder fast das gesamte Layout der E-Mail enthalten.

### Der Code

Wir erweitern den Code von letzter Woche, um benutzerdefinierte Token an unseren Anfrage-Kampagnen-Aufruf weiterzugeben.

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");

        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class="replacedContent"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Dieses Mal erstellen wir den Inhalt unseres Tokens in der bodyReplacement-Variablen und verwenden dann die addToken-Methode, um ihn der Anfrage hinzuzufügen. addToken nimmt einen Schlüssel und einen Wert, erstellt dann eine JsonObject-Darstellung und fügt sie dem internen Token-Array hinzu. Diese wird dann während der postData-Methode serialisiert und erstellt einen Hauptteil, der wie folgt aussieht:

`{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}`

In Kombination sieht unsere Konsolenausgabe wie folgt aus:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with&nbsp;...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"<apiuser@mktosupport.com>"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json?access_token=19d51b9a-ff60-4222-bbd5-be8b206f1d40:st
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

### Verpackung

Diese Methode ist auf eine Vielzahl von Arten erweiterbar, indem der Inhalt in E-Mails innerhalb einzelner Layout-Abschnitte oder außerhalb von E-Mails geändert wird, sodass benutzerdefinierte Werte an Aufgaben oder interessante Momente weitergegeben werden können. Überall dort, wo ein Token innerhalb eines Programms verwendet werden kann, kann mit dieser Methode angepasst werden. Ähnliche Funktionen sind auch beim Aufruf [Kampagne planen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) verfügbar, mit dem Sie Token für eine gesamte Batch-Kampagne verarbeiten können. Diese können nicht pro Lead angepasst werden, sind aber sehr nützlich, um Inhalte für eine Vielzahl von Leads anzupassen.

Veröffentlicht am _2015-07-24_ von _Kenny_

## Aktualisieren von Lead-Daten in Marketo mithilfe der API

Vor mehr als einem Jahr haben wir die Aktualisierung von Kunden- und Interessenteninformationen in Marketo mithilfe der -API veröffentlicht. Dieser Beitrag präsentierte ein Codebeispiel, das wiederkehrend ausgeführt werden konnte, um ein externes System nach Updates zu durchsuchen. Die Idee war, die externen Daten abzurufen und sie in Marketo zu übertragen, um Lead-Informationen zu aktualisieren. Das vorgestellte Codebeispiel verwendete unsere SOAP-API. REST-Endpunkt zum Erreichen desselben Ziels. **Programmeingabe** Wahrscheinlich müssen Sie die Daten aus dem externen System in ein Format umwandeln, das von Marketo-REST-APIs genutzt werden kann. Da wir die API zum Erstellen/Aktualisieren von Leads verwenden, müssen die Daten als JSON formatiert sein, die im Anfragetext gesendet wird. Dies lässt sich am besten anhand eines Beispiels erklären. Im folgenden Java-Beispiel-Code haben wir Pseudo-Lead-Daten in ein Array von Zeichenfolgen eingefügt. Jede Zeichenfolge enthält folgende Daten für jeden Lead: Vorname, Nachname, E-Mail-Adresse, Stellenbezeichnung.

```java
private static String externalLeadData[] = {
        "Henry, Adams, <henry@superstar.com>, Director of Demand Generation",
        "Suzie, Smith, <ssmith@gmail.com>, VP Marketing"
};
```

Der Beispiel-Code transformiert das -Array in den folgenden JSON-Block.

```json
{
"input":
    [
        {"firstName":"Henry", "lastName":"Adams", "email":"<henry@superstar.com>", "title":"Director of Demand Generation"},
        {"firstName":"Suzie", "lastName":"Smith", "email":"<ssmith@gmail.com>", "title":"VP Marketing"}
    ]
}
```

Jedes „Eingabe“-Array-Element entspricht einem individuellen Lead in Marketo. Die Array-Elemente sind JSON-Objekte, die einen oder mehrere Marketo-Lead-Feldnamen und die entsprechenden Werte enthalten. Die Feldnamen, die Sie angeben möchten (in diesem Fall „firstName“, „lastName“, „email“ und „title„), müssen mit dem REST-API-Namen übereinstimmen, der für das Marketo-Abonnement definiert wurde. Die REST-API-Namen finden Sie im Abschnitt Feldverwaltung im Admin-Bedienfeld von Marketo, indem Sie die Feldnamen exportieren. Die Feldnamen werden in eine Excel-Datei exportiert (siehe unten). Alternativ können Sie Feldnamen programmgesteuert finden, indem Sie die API [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2) aufrufen. Hier finden Sie beispielsweise ein Antwort-Snippet „Describe“, das den REST-API-Namen für das Feld „Vorname“ enthält.

```json
{
    "id": 29,
    "displayName": "First Name",
    "dataType": "string",
    "length": 255,
    "soap": {
        "name": "FirstName",
        "readOnly": false
    },
    "rest": {
        "name": "firstName",
        "readOnly": false
    }
},
```

**Programmlogik** Sobald die Payload das richtige Format aufweist, können wir Leads erstellen/aktualisieren aufrufen. Beachten Sie, dass wir in diesem Beispiel keinen der optionalen Parameter angeben. Das Standardverhalten ist das Erstellen oder Aktualisieren von Lead-Datensätzen (upsert), die Verwendung von E-Mails für Deduplizierungszwecke und die Verwendung der synchronen Verarbeitung. Wenn der Aufruf zum Erstellen/Aktualisieren von Leads erfolgreich ist, enthält der Antworttext ein „Ergebnis“-Array mit der Marketo-Lead-ID und dem Status des Erstellungs-/Aktualisierungsvorgangs. Je nach dem Parameter „action“, den Sie in der Anfrage übergeben haben, kann der Status entweder „updated“, „created“ oder „skipped“ sein. Nehmen wir weiter an, der erste Lead existiert nicht (Henry Adams) und der zweite Lead existiert nicht (Suzie Smith). Eine Antwort ähnlich der folgenden würde anzeigen, dass der erste Lead erstellt und der zweite Lead aktualisiert wurde.

```json
{
    "success":true,
    "requestId":"118f8#14f1a0b82fc",
    "result":[
        {
            "id":318798,
            "status":"created"
        },
        {
            "id":318797,
            "status":"updated"
        }
    ]
}
```

Das war&#39;s fürs Erste. Alles Gute zum Programmieren! **Programm-Code**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

public class CreateUpdateLeads {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            { INSERT YOUR CUSTOM SERVICE DATA HERE };

    //
    // Mock up data that could be read from an external data source.
    // An array of CSV strings the form:
    //   "firstName, lastName, email, title"
    private static String externalLeadData[] = {
        "Henry, Adams, henry@superstar.com, Director of Demand Generation",
        "Suzie, Smith, ssmith@gmail.com, VP Marketing"
    };

    public static void main(String[] args) {
        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URL for Create/Update Leads
        String createUpdateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", baseUrl, accessToken);

        // Convert String array into JsonArray to pass as part of request body
        JsonArray input = convertExternalLeadDataToJson(externalLeadData);

        // Build request body JSON
        JsonObject requestObj = new JsonObject();
        requestObj.add("input", input);

        // Call Create/Update Lead API
        JsonArray result = new JsonArray();
        JsonObject leadObj = JsonObject.readFrom(httpRequest("POST", createUpdateLeadsUrl, requestObj.toString()));
        if (leadObj.get("success").asBoolean()) {
            if (leadObj.get("result") != null) {
                result = leadObj.get("result").asArray();
            }
        }

        // Print out results object
        System.out.println(result);
        System.exit(0);
    }

    // Convert array of CSV formatted Strings into an array of JSON objects
    private static JsonArray convertExternalLeadDataToJson(String input[]) {
        JsonArray output = new JsonArray();

        // Loop through each CSV row in array
        for (int i = 0; i < input.length; i++) {
            // Split CSV row into separate fields
            List items = Arrays.asList(input[i].split(","));

            // Add fields to JSON object
            JsonObject lead = new JsonObject();
            lead.add("firstName", items.get(0));
            lead.add("lastName", items.get(1));
            lead.add("email", items.get(2));
            lead.add("title", items.get(3));
            output.add(lead);
        }
        return output;
    }

    // Perform HTTP request
    private static String httpRequest(String method, String endpoint, String body) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setDoOutput(true);
            urlConn.setRequestMethod(method);
            switch (method) {
                case "GET":
                    break;
                case "POST":
                    urlConn.setRequestProperty("Content-type", "application/json");
                    urlConn.setRequestProperty("accept", "text/json");
                    OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                    wr.write(body);
                    wr.flush();
                    break;
                default:
                    System.out.println("Error: Invalid method.");
                    return data;
            }
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Veröffentlicht am _2015-08-14_ von _David_

## Hinzufügen von SalesPerson-Daten zu Marketo

Mit den neuen SalesPerson-APIs können Sie Marketo-Leads in Instanzen ohne native CRM-Integration frei mit SalesPerson-Datensätzen verknüpfen. Dies ermöglicht die Verwendung von {{lead.Lead Owner Email Address}} und zugehörigen Feldern und Token in Marketo.

### Erstellen von SalesPerson-Datensätzen

Um Leads mit SalesPerson-Datensätzen zu verknüpfen, müssen wir zunächst unsere SalesPerson-Datensätze in Marketo eingeben. Hier ist eine Beispielklasse in PHP:

```php
<?php

class SalesPerson{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $dedupeBy;//dedupeFields or idField
 private $input;//array of salesperson objects for input

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->dedupeBy)){
   $body->dedupeBy = $this->dedupeBy;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/salespersons.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction($action){
  return $this->action;
 }
 public function setDedupeBy($dedupeBy){
  $this->dedupeBy = $dedupeBy;
 }
 public function getDedupeBy($dedupeBy){
  return $this->dedupeBy;
 }
 public function addSalesPersons($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else{
   $this->input = $input;
  }
 }
 public function getInput(){
  return $this->input;
 }

}
```

In der obigen Klasse ist die $input-Variable ein Array von stdClass-Objekten mit möglichen Membern:

* externalSalesPersonId(nur beim Erstellen gültig)
* id(nur als Schlüssel im UpdateOnly-Modus)
* E-Mail
* Fax
* firstName
* lastName
* Mobiltelefon
* Telefon
* Titel

Detailliertere Informationen zu Typen und Feldlängen können über den Aufruf Describe SalesPerson abgerufen werden.

### Leads synchronisieren

Im Folgenden finden Sie eine kurze Beispielklasse zum Synchronisieren der benötigten Leads:

```php
<?php

class Leads{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $lookupField;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 private $asyncProcessing;//specify whether input should be processed asynchronously
 private $partitionName;//partition name for update if requires

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->lookupField)){
   $body->lookupField = $this->lookupField;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/leads.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction(){
  return $this->action;
 }
 public function setDedupeBy($lookupField){
  $this->dedupeBy = $lookupField;
 }
 public function getDedupeBy(){
  return $this->dedupeBy;
 }
 public function addLeads($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else if (is_array($input)){
   $this->input = $input;
  }else{
   $this->input = [$input];
  }
 }
 public function getInput(){
  return $this->input;
 }
 public function setAsyncProcessing($asyncProcessing){
  $this->asyncProcessing = $asyncProcessing;
 }
 public function getAsyncProcessing() {
  return $this->asyncProcessing;
 }
 public function setPartitionName($partitionName){
  $this->partitionName = $partitionName;
 }
 public function getPartitionName() {
  return $this->partitionName;
 }

}
```

### Prozess

Im Folgenden finden Sie ein Beispiel für die Erstellung von zwei Verkaufsberater-Datensätzen und ihre Verknüpfung mit zwei Lead-Datensätzen:

```php
<?php

require 'Auth.php';
require 'SalesPerson.php';
require 'Leads.php';

//Create an Auth object for authentication
$auth = new Auth("https://284-RPR-133.mktorest.com", "7f99bd49-0e0e-4715-a72a-0c9319f75552", "O5lZHhrQDfDKRhulY89IU42PfdhRSe6m");

//Compose new SalesPerson Records
$sales1 = new stdClass();
$sales1->externalSalesPersonId = "SalesPerson 1";
$sales1->email = "sales1@example.com";
$sales1->firstName = "Jane";
$sales1->lastName = "Doe";

$sales2 = new stdClass();
$sales2->externalSalesPersonId = "SalesPerson 2";
$sales2->email = "sales2@example.com";
$sales2->firstName = "John";
$sales2->lastName = "Doe";

//Compose lead records
$lead1 = new stdClass();
$lead1->email = "marketoDev@example.com";
//Add the external id of the desired salesperson
$lead1->externalSalesPersonId = $sales1->externalSalesPersonId;

$lead2 = new stdClass();
$lead2->email = "devBlog@example.com";
$lead2->externalSalesPersonId = $sales2->externalSalesPersonId;

//Create a new instance of SalesPerson to submit records
$salesPerson = new SalesPerson($auth, [$sales1, $sales2]);
$spResponse = $salesPerson->postData();
print_r($spResponse . "rn");
$json = json_decode($spResponse);

//Check for success on synching SalesPersons
if ($json->success){
 $leads = new Leads($auth, [$lead1, $lead2]);
 $leadsResponse = $leads->postData();
 print_r($leadsResponse . "rn");
}else{
 print_r("SalesPerson request failed with errors:rn");
 foreach ($json->errors as $error){
  print_r("code: " . $error->code . ", message: " . $error->message . "rn");
 }
}
```

Für unsere Beispielklassen erstellen wir gerade stdClass-Objekte, um unsere SalesPerson- und Lead-Datensätze darzustellen, die synchronisiert werden müssen, wobei jedes gewünschte Feld als Member hinzugefügt wird. Nach Ausführung dieses Codes werden für die Lead-<marketoDev@example.com> und die Lead-<devBlog@example.com> die Felder „E-Mail-Adresse des Lead-Inhabers“, „Vorname des Lead-Inhabers“ und „Nachname des Lead-Inhabers“ ausgefüllt, sodass sie die entsprechenden Token für diese Felder verwenden und von den entsprechenden Smart-List-Filtern gefiltert werden können.

### Authentifizierungsklasse

```php
<?php

class Auth{
 private $host;//host of the target Marketo instance
 private $clientId;//client Id
 private $clientSecret;//client secret
 private $token;//access_token
 private $expiry;

 function _construct($host, $clientId, $clientSecret){
  $this->host = $host;
  $this->clientId = $clientId;
  $this->clientSecret = $clientSecret;
 }
 public function getToken(){
  if (!isset($this->token) || $this->expiry > time()){
   $data = $this->getData();
   $this->expiry = time() + $data->expires_in;
   $this->token = $data->access_token;
   return $this->token;
  }else{
   return $this->token;
  }
 }
 private function getData(){
  $url = $this->host . "/identity/oauth/token?grant_type=client_credentials&client_id=" . $this->clientId . "&client_secret="
     . $this->clientSecret;
  $ch = curl_init($url);
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  $response = curl_exec($ch);
  $json = json_decode($response);
  curl_close($ch);
  return $json;
 }
 public function getHost(){
  return $this->host;
 }
}
```

Veröffentlicht am _21.08.2015_ von _Kenny_

## Best Practices für API-Benutzer und benutzerdefinierte Services

Die REST-APIs von Marketo verwenden benutzerdefinierte Services zur Authentifizierung, und jeder dieser Services gehört einem Marketo-Benutzer, der nur über eine API verfügt. Die Funktionen der einzelnen benutzerdefinierten Services werden durch die Berechtigungen der einzelnen Rollen bestimmt, die diesem Benutzer zugewiesen sind. Die Zuweisung einzelner Benutzer und benutzerdefinierter Services zu Ihren Integrationen bietet Ihnen mehrere Vorteile:

* Sie können die [Berechtigungen“ anpassen](/help/rest-api/custom-services.md) die jedem einzelnen Service über die Rolle erteilt werden, die Ihrem Benutzer zugewiesen wurde.
* Sie können einzelne Webdienste davon abhalten, Ihre Instanz aufzurufen, indem Sie den entsprechenden benutzerdefinierten Dienst löschen, ohne andere zu deaktivieren.
* Die Berichte zur Nutzung der API-Aufrufe werden nach Benutzer aufgeschlüsselt, sodass Sie eine hohe und anormale Nutzung erkennen können
* Es ist einfacher zu ermitteln, auf welche Daten jeder Webservice Zugriff erhält
* Workspace-aktivierte Instanzen können den Zugriff auf bestimmte Geschäftsbereiche einschränken, indem sie nur Rollen für barrierefreie Arbeitsbereiche zuweisen

### API-Nutzung

Jeder Ihrer API-Benutzer wird einzeln im API-Nutzungsbericht gemeldet, sodass Sie durch die Aufspaltung Ihrer Web-Services nach Benutzer einfach die Nutzung jeder Ihrer Integrationen erfassen können. Wenn die Anzahl der API-Aufrufe an Ihre Instanz das Limit überschreitet und nachfolgende Aufrufe fehlschlagen, können Sie mit dieser Vorgehensweise das Volumen aus jedem Ihrer Services berücksichtigen und bewerten, wie Sie das Problem beheben können. Überprüfen Sie Ihre Nutzung, indem Sie zu Admin > Web-Services gehen und auf die Anzahl der Aufrufe in den letzten sieben Tagen klicken.

### Deaktivieren von Services

Wenn eine Integration unerwünschte Auswirkungen hat, kann es mühsam und schwierig zu deaktivieren sein, wenn Sie nicht jedem einen individuellen Service zugewiesen haben. Diese einzeln aufzuschlüsseln macht es Ihnen so einfach, den fehlerhaften Dienst in Ihrem Admin -> Launchpoint zu löschen.

### Verwaltung von Workspace

Bei Marketo Enterprise-Abonnements ist es üblich, dass ein Service nur Zugriff auf einen einzigen Arbeitsbereich benötigt. Dies kann [erzwungen werden, indem dem API-Benutzer eine Rolle zugewiesen ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/allow-user-access-to-a-workspace). Jede Benutzerrolle kann entweder global oder pro Arbeitsbereich zugewiesen werden, sodass der Zugriff in Arbeitsbereichen nach Bedarf eingeschränkt werden kann, wobei die minimalen Berechtigungen bereitgestellt werden.

Veröffentlicht am _2015-08-28_ von _Kenny_

## Angeben von Lead-Partitionen mithilfe der REST-API

**Lead-**: Marketo-Lead-Partitionen bieten eine praktische Möglichkeit, Leads zu isolieren. Partitionen können es verschiedenen Marketing-Gruppen in Ihrem Unternehmen ermöglichen, eine einzelne Marketo-Instanz gemeinsam zu nutzen. Weitere Informationen finden Sie unter [Arbeitsbereiche und Lead-Partitionen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/understanding-workspaces-and-person-partitions). Angenommen, Sie verwenden Lead-Partitionen und erstellen Leads programmgesteuert mithilfe der Marketo-REST-API. Wie stellen Sie sicher, dass die Leads, die Sie erstellen, in der richtigen Partition landen? Dieser Beitrag zeigt euch, wie! Für dieses Beispiel verwenden wir Arbeitsbereiche und Partitionen, um unsere Leads basierend auf der Geografie zu isolieren.

Zunächst definieren wir einen Arbeitsbereich mit dem Namen „Land“. Als Nächstes erstellen wir zwei Partitionen in diesem Arbeitsbereich namens „Mexiko“ und „Kanada“.  **Lead in Partition erstellen** Angenommen, wir möchten jetzt zwei Leads in der „Mexiko“-Partition erstellen. Um Leads zu erstellen, rufen wir auf. Um die Partition anzugeben, müssen wir das Attribut „partitionName“ in den Anfrageinhalt aufnehmen. Woher wissen wir, was für den Wert partitionName verwendet werden soll? Wir können eine Liste gültiger Partitionsnamenwerte für unsere Instanz abrufen, indem wir die API [Lead-Partitionen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeProgramMemberUsingGET) wie folgt aufrufen:

`GET /rest/v1/leads/partitions.json`

```json
{
    "requestId": "20ae#14f9a1a5a30",
    "result": [
        {
            "id": 1,
            "name": "Default",
            "description": "Initial system lead partition"
        },
        {
            "id": 2,
            "name": "Mexico",
            "description": "Leads that live in Mexico"
        },
        {
            "id": 3,
            "name": "Canada",
            "description": "Leads that live in Canada"
        }
    ],
    "success": true
}
```

In diesem Fall ist der richtige Wert „Mexiko“, sodass wir ihn wie folgt an „Leads erstellen/aktualisieren“ übergeben:

`POST /rest/v1/leads.json`

```json
{
    "input": [
        {"email":"enrique.pena-nieto@gob.mx"},
        {"email":"angelica.rivera@gob.mx"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Hier finden Sie unsere neu erstellten Leads in Marketo.  **Lead in Partition aktualisieren** Um vorhandene Leads in einer Partition zu aktualisieren, rufen wir einfach Leads erstellen/aktualisieren auf und geben partitionName genauso an wie zuvor. Für dieses Update weisen wir den Leads, die wir oben erstellt haben, Vornamen, Nachnamen und Titel zu.

`POST /rest/v1/leads.json`

```json
{
"input": [
        {"email":"enrique.pena-nieto@gob.mx", "firstName":"Enrique", "lastName":"Pena Neito", "title": "El Presidente"},
        {"email":"angelica.rivera@gob.mx", "firstName":"Angelica", "lastName": "Rivera", "title": "Primera Dama"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Hier finden Sie unsere neu aktualisierten Leads in Marketo.
**Partition für einen Lead identifizieren** Wie können wir erkennen, in welcher Partition sich ein Lead befindet? Hierfür verwenden wir die API [Lead nach ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) und geben im Abfrageparameter „fields“ die Zeichenfolge „leadPartitionId“ an. In diesem Fall rufen wir die Informationen für die Lead-ID 318816 ab, die wir oben erstellt haben.

`GET /rest/v1/lead/318816.json?fields=leadPartitionId,email,firstName,lastName,title`

```json
{
    "requestId": "5c57#14f9a495b1f",
    "result": [
        {
            "id": 318816,
            "lastName": "Pena Neito",
            "title": "El Presidente",
            "email": "enrique.pena-nieto@gob.mx",
            "firstName": "Enrique",
            "leadPartitionId": 2
        }
    ],
    "success": true
}
```

Beachten Sie, dass wir die leadPartitionId statt des partitionName zurückerhalten. Um den partitionName zu erhalten, müssen wir die Ausgabe von der GET Lead Partitions API von oben erneut aufrufen.

```json
{
    "id": 2,
    "name": "Mexico",
    "description": "Leads that live in Mexico"
},
```

In diesem Fall ist der Wert „leadPartitionId“ von 2 dem partitionName von „Mexico“ zugeordnet. Das ist alles für den Moment. Frohes Partitionieren!

Veröffentlicht am _2015-09-04_ von _David_

## Vergleichen von Bewertungsfeldern in Marketo Email Scripting

**Hinweis:** Dies ist ein Gastbeitrag von Cathal Moran. Cathal ist Solutions Consultant bei der Marketo EMEA-Niederlassung in Dublin, Irland.

### Vergleichen von Bewertungsfeldern

Viele Marketo-Kunden, insbesondere solche, die sich auf den Crossselling konzentrieren, haben mehrere Bewertungsfelder, und diese werden oft verwendet, um das Interesse eines Leads an einem bestimmten Produkt/Bereich zu messen. Stellen Sie sich vor, ich verkaufe Äpfel und Bananen, wenn ein Lead eine Bewertung von 50 für Äpfel und 10 für Bananen hat, dann ist klar, wo die Präferenz liegt, wäre es nicht schön, wenn mein Inhalt diese Präferenz widerspiegeln würde. E-Mail-Skripterstellung kann verwendet werden, um Bewertungen zu vergleichen und den Inhalt einer E-Mail in Abhängigkeit davon zu personalisieren, welcher Wert für den jeweiligen Lead, der die E-Mail erhält, am höchsten (oder am niedrigsten) ist.

### Da ich zwei Zahlen vergleichen möchte, muss ich meine Feldwerte in Ganzzahlen konvertieren

```
#set ($score1 = $math.toInteger(${lead.Apple_Score}))
#set ($score2 = $math.toInteger(${lead.Banana_Score}))
##check if the lead score is greater than feature score
#if($score1 >= $score2)
                ##if Apple score is greater
                #set($Interest = "Special offer on Apples")
##check is the feature score is higher
#elseif($score2 >= $score1)
                ##if Feature score is greater
                #set($Interest = "Special offer on Bananas")
#else
                ##otherwise
                #set($Interest = "Special offer on Fruit")
#end
##display the Interest as content
${Interest}
```

Im obigen Beispiel personalisiere ich nur Text, aber ich könnte genauso einfach zum Beispiel ein anderes Bild anzeigen, das auf der höheren Bewertung basiert.

Veröffentlicht am _2015-09-14_ von _Kenny_

## Durchführen einer Marketo-Formularübermittlung im Hintergrund

Wenn Ihr Unternehmen über viele verschiedene Plattformen zum Hosten von Web-Inhalten und Kundendaten verfügt, ist es relativ üblich, parallele Übermittlungen von einem Formular zu benötigen, damit die resultierenden Daten in separaten Plattformen erfasst werden können. Dazu gibt es mehrere Strategien, aber die beste ist oft die einfachste: Verwenden der Forms 2-API zum Senden eines ausgeblendeten Marketo-Formulars. Dies funktioniert mit jedem neuen Marketo-Formular, aber Sie sollten idealerweise ein leeres Formular dafür erstellen, das keine Felder enthält. Dadurch wird sichergestellt, dass das Formular nicht mehr Daten als nötig lädt, da wir nichts rendern müssen. Nehmen Sie nun einfach den [Einbettungs-Code](https://experienceleague.adobe.com/de/docs/marketo/using/home) aus Ihrem Formular und fügen Sie ihn dem Textkörper Ihrer gewünschten Seite hinzu, indem Sie eine kleine Änderung vornehmen. Ihr Einbettungs-Code enthält ein Formularelement wie das folgende:

`<form id="mktoForm_1068"></form>`

Sie sollten &#39;style=„display:none&quot;&#39; zum Element hinzufügen, damit es nicht sichtbar ist, wie in diesem Beispiel:

`<form id="mktoForm_1068" style="display:none"></form>`

Sobald das Formular eingebettet und ausgeblendet ist, ist der Code zum Senden des Formulars sehr einfach:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"test@example.com",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms hat auf diese Weise gesendet und verhält sich genau so, als ob der Lead ein sichtbares Formular ausgefüllt und gesendet hätte. Das Auslösen der Übermittlung variiert zwischen den verschiedenen Implementierungen, da jede eine andere Aktion hat, um sie zu veranlassen, aber Sie können sie für jede Aktion ausführen lassen. Wichtig ist, dass Sie Ihre Felder und Werte richtig einstellen. Verwenden Sie unbedingt den SOAP-API-Namen Ihrer Felder, den Sie unter [Feldnamen exportieren](/help/rest-api/list-of-standard-fields.md) finden, um eine korrekte Übermittlung der Werte sicherzustellen.

Migration vom Munchkin Associate-Lead

Die Übermittlung von Hintergrundformularen ist eine der empfohlenen Ersatzmethoden für Munchkin Associate Lead. Auf der folgenden Beispielseite für HTML wird die Migration auf einer allgemeinen Ebene veranschaulicht, indem dieselben Werte, die an Associate Lead übermittelt werden, wiederverwendet werden.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName('head')[0].appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //submit the form
            form.submit();


        })
    </script>
</body>
</html>
```

Veröffentlicht am _2015-09-25_ von _Kenny_

## Ausnahme- und Fehlerbehandlung bei der Marketo REST-API: Teil 1

Die Marketo-REST-API kann eine Ausnahme oder einen Fehler zurückgeben, die wir aus praktischen Gründen von hier an nur Fehler aufrufen. Fehler können auf drei verschiedenen Ebenen auftreten:

* auf HTTP-Ebene, werden diese Fehler durch einen 4xx-Code angezeigt
* Auf Antwortebene sind diese Fehler im Array „Fehler“ der JSON-Antwort enthalten
* Eintragsebene angegeben sind, werden diese Fehler in das Array „Ergebnis“ der JSON-Antwort aufgenommen und auf individueller Eintragsbasis mit dem Feld „Status“ und dem Array „Gründe“ angezeigt

### HTTP-Fehler

Unter normalen Betriebsbedingungen sollte Marketo nur zwei HTTP-Statusfehler zurückgeben: 413 Anfrageentität zu groß und 414 Anfrageentität zu lang. Beide können durch Abrufen des Fehlers, Ändern der Anfrage und erneutes Versuchen wiederhergestellt werden. Mit intelligenten Kodierungsverfahren sollten Sie jedoch nie auf diese stoßen. Marketo gibt 413 zurück, wenn die Payload der Anfrage 1 MB überschreitet, oder 10 MB im Fall von [Lead importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads). In den meisten Szenarien ist es unwahrscheinlich, dass diese Grenzwerte erreicht werden. Wenn Sie jedoch die Größe der Anfrage überprüfen und Datensätze, die zur Überschreitung des Grenzwerts führen, in eine neue Anfrage verschieben, sollten Sie alle Umstände verhindern, die zu diesem Fehler führen, indem Sie Endpunkte zurückgeben. 414 wird zurückgegeben, wenn der URI einer GET-Anfrage 8 KB überschreitet. Um dies zu vermeiden, überprüfen Sie die Länge Ihrer Abfragezeichenfolge, um festzustellen, ob sie diesen Grenzwert überschreitet. Wenn Ihre Anfrage jedoch in eine POST-Methode geändert wird, geben Sie Ihre Abfragezeichenfolge als Anfragetext mit dem zusätzlichen Parameter &#39;_method=GET&#39; ein. Dadurch wird die Einschränkung für URIs aufgegeben. In den meisten Fällen ist es selten, dieses Limit zu erreichen, aber es ist etwas üblich, wenn große Batches von Datensätzen mit langen individuellen Filterwerten wie einer GUID abgerufen werden.

### Fehler auf Antwortebene

Fehler auf Antwortebene sind vorhanden, wenn der Parameter „Erfolg“ der Antwort auf „false“ gesetzt ist. Jeder Eintrag in „Fehler“ hat zwei Mitglieder, „Code“ eine Nummer, entweder 6xx oder 7xx, und eine „Nachricht“, die den Klartext-Grund für den Fehler angibt. 6xx-Codes zeigen immer an, dass eine Anfrage vollständig fehlgeschlagen ist und nicht ausgeführt wurde. Ein Beispiel dafür ist ein 601-Zugriffstoken „Ungültig“, das durch erneute Authentifizierung und Übergabe des neuen Zugriffstokens mit der Anfrage wiederherstellbar ist. 7xx-Fehler zeigen an, dass die Anfrage fehlgeschlagen ist, entweder weil keine Daten zurückgegeben wurden oder weil die Anfrage falsch parametrisiert wurde, z. B. weil ein ungültiges Datum enthalten war oder ein erforderlicher Parameter fehlte.

### Fehler auf Datensatzebene

Fehler auf Datensatzebene zeigen an, dass ein Vorgang für einen einzelnen Datensatz nicht abgeschlossen werden konnte, die Anfrage selbst jedoch gültig war. Diese treten innerhalb einzelner Datensätze im „Ergebnis“-Array einer Antwort auf. Das Feld „Status“ dieser Datensätze wird „übersprungen“ und ein Array „Gründe“ ist vorhanden. Jeder Grund enthält ein Element des Typs „Code“ und ein Element des Typs „Nachricht“. Der Code ist immer 1xxx. In der Meldung wird angegeben, warum der Datensatz übersprungen wurde. Ein Beispiel wäre, wenn bei einer Anfrage zum Erstellen/Aktualisieren von Leads „Aktion“ auf „createOnly“ eingestellt ist, aber bereits ein Lead für einen der Schlüssel in den gesendeten Datensätzen vorhanden ist. In diesem Fall wird der Code 1005 zurückgegeben, und die Meldung „Lead existiert bereits.“ In den nächsten Beiträgen werden wir einen Blick auf behebbare Fehler werfen, und einige Beispiele dafür, wie diese in Ihrem Code gehandhabt werden.

Veröffentlicht am _2015-10-09_ von _Kenny_

## Gastbeitrag - Direkte SQL-Connectoren zur Marketo-Datenbank

**Dies ist ein Gastbeitrag von Sumit Sarkar von Progress, Inc.**

**SQL-Connectoren zur Marketo-Datenbank** verfügt Marketo über gut dokumentierte APIs für den Datenzugriff. Manchmal benötigen Unternehmen jedoch direkten SQL-Zugriff. Wir sehen, wie sich die Grenzen zwischen Marketing und IT in Marketo-Shops verwischen, was die Nachfrage nach standardbasierter SQL-Konnektivität erhöht. Direkte SQL-Connectoren für Marketo sind über [Progress DataDirect Cloud](https://www.progress.com/connectors/cloud-data-integration) verfügbar. DataDirect Cloud ist ein Datenkonnektivitätsdienst, der Marketo-Daten über offene Branchenstandards für den SQL-Zugriff über ODBC („Open Database Connectivity„) oder JDBC („Java Database Connectivity„) und REST mit OData („Open Data Protocol„) bereitstellt. Im Folgenden finden Sie einige beliebte Anwendungsfälle für die vorkonfigurierte Verbindung von Marketo-Daten mit DataDirect Cloud:

* Datenerkennung und -visualisierung (Qlik, Tableau, SAP Lumira)
* Enterprise Reporting (SAP Business Objects, Microstrategy, Cognos)
* Datenintegration (SQL Server Integration Services - SSIS, Oracle Data Integrator - ODI, Informatica)
* Data Federation (SQL Server Linked Server, SAP HANA SDA, Oracle Database Gateway)
* Ad-hoc-Abfrage (Microsoft Office, DB Visualizer, Aqua Data Studio)
* Datenvorbereitung (Alteryx, Trifecta, Paxata)

**Wie funktioniert der SQL-Zugriff auf Marketo?**

* DataDirect Cloud erstellt ein logisches Schema für Daten, die von Marketo-Integrations-APIs bereitgestellt werden.
* DataDirect Cloud verarbeitet SQL-Anfragen von einfachen ODBC- oder JDBC-Clients mithilfe einer elastischen Echtzeit-Abfrage-Engine auf Daten, die aus den Marketo-APIs abgerufen werden.
* DataDirect Cloud-Konnektivität ist direkt und in Echtzeit ohne Duplizierung von Daten.
* DataDirect Cloud Service abstrahiert die Marketo-API so, dass sich die Endbenutzer keine Gedanken über API-Versionsänderungen machen müssen oder SOAP im Vergleich zu REST verwenden.

DataDirect baut diese Art der Konnektivität zu SaaS-Datenquellen seit 2006 auf, beginnend mit dem ersten Salesforce ODBC-Treiber, der auf seinen Webservice-APIs aufbaut. **Erste Schritte beim Herstellen einer Verbindung mit Marketo:**

1. [Registrieren Sie sich für eine DataDirect Cloud-Anmeldung](https://pacific.progress.com/console/register?productName=d2c&ignoreCookie=true)
1. Klicken Sie auf „Datenquellen“ und dann auf die Schaltfläche &quot;+New Data Source&quot;
1. Wählen Sie &quot;Marketo&quot; aus und geben Sie die Verbindungsinformationen ein. Sie können sich bei Ihrem Marketo-Administrator erkundigen oder anmelden, um [Verbindungsinformationen für die SOAP-Integration](/help/soap-api/soap-api.md) zu finden.
1. Klicken Sie auf „Verbindung testen“. Beachten Sie, dass es eine OData-Registerkarte gibt, um OData aus Marketo zu erstellen, und wir werden dies in einem zukünftigen Blogpost diskutieren.
1. Klicken Sie auf „SQL-Test“, wenn Sie das angezeigte Marketo-Schema untersuchen oder einfache SQL-Abfragen über die Benutzeroberfläche ausführen möchten.
1. Klicken Sie auf der linken Seite auf „Downloads“ und wählen Sie den DataDirect Cloud ODBC- oder JDBC-Treiber für Ihre Anwendung und Plattform zur Installation aus.
1. Sobald der DataDirect Cloud ODBC- oder JDBC-Treiber installiert ist, können Sie alle standardbasierten Anwendungen mit Marketo verbinden.

Im Folgenden finden Sie ein Videobeispiel für [Verbinden mit dem DataDirect Cloud ODBC-Client](https://www.youtube.com/watch?v=H6PHra56Iig). Im Folgenden finden Sie weitere DataDirect Cloud-Tutorials, die für Marketo gelten:

* [SAP Data Analytics](http://scn.sap.com/community/lumira/blog/2015/08/05/connect-sap-lumira-to-eloqua-marketo-google-analytics)
* [Microstrategy Enterprise Reporting](https://community.microstrategy.com/t5/Tech-Corner/What-MSTR-developers-should-know-about-Cloud-Data-Sources/ba-p/253083)
* [Oracle-Datenintegration](https://www.ateam-oracle.com/post/a-universal-cloud-applications-adapter-for-odi/)

**Forschung und Entwicklung stehen vor der Herausforderung, SQL-Konnektivität über Cloud-Quellen wie Marketo hinweg aufzubauen**

Nicht alle SaaS-APIs stellen eine standardmäßige Abfragesprache bereit. In diesen Fällen betrachtet das Engineering-Team jedes Objekt einzeln. Jedes Objekt kann mit einer anderen API mit eindeutigen Regeln für das Aufrufen, Suchen, Filtern usw. verfügbar gemacht werden. Es war ein erheblicher Aufwand erforderlich, ein Standarderlebnis für die Abfrage des gesamten Datenmodells bereitzustellen.

Handhabung vollständiger Join-Funktionen. Wenn die SaaS-APIs eine Abfragesprache mit JOIN-Funktion nicht unterstützen, muss das Engineering-Team diesen Vorgang ausführen. Dies erfordert eine Übersetzung aus SQL, um Marketo-APIs effizient aufzurufen und so vor der Durchführung des Joins die minimale Datenmenge zurückzugeben. Beim Verbinden zweier sehr großer Objekte kann die Datenzugriffsschicht erhebliche Ressourcen auf dem Anwendungsserver oder Desktop verbrauchen. Daher ist die Bereitstellung der Datenzugriffsschicht für einen elastischen Cloud-Service wie DataDirect Cloud aus zwei Gründen sehr sinnvoll:

1. Schnellere Leistung und weniger Arbeitsspeicher-/CPU-Ressourcen auf dem Client-Anwendungsserver oder Desktop
1. Nutzen Sie die überlegene Bandbreite zwischen DataDirect Cloud und Marketo, wo vorab verbundene Datensätze ausgetauscht werden.

Wie werden Datenmodelle gehandhabt? Ist sie statisch oder dynamisch? Wie werden Änderungen erkannt und an den Client übermittelt? Jede SaaS-Datenquelle ist anders. Im Fall von Marketo werden bestimmte Objekte besser über Ansichten und andere über Tabellen abgefragt. Der Umgang mit dieser Matrix von Datenmodellen und -objekten über alle SaaS-Quellen hinweg war sicherlich eine Herausforderung.

**Marketo- und DataDirect-Cloud-Referenz für Entwickler:**

* Marketo-Verbindungseigenschaften ([Link zu Dokumenten](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Unterstützte SQL-Abfragen und Erweiterungen mit Marketo ([Link zu Dokumenten](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html,-hubspot,-and-marketo.html))
* Verfügbare Marketo-Tabellen und -Ansichten ([Link zu Dokumenten](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Allgemeine von Marketo zurückgegebene Fehlermeldungen ([Link zu Dokumenten](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))

**Biografie:** Sumit Sarkar ist ein leitender Datenevangelist bei Progress und verfügt über mehr als 10 Jahre Erfahrung in der Datenkonnektivität. Als weltweit führender Berater für die Konnektivität offener Datenstandards mit Cloud-Daten hat Sumit unter anderem die Leistungsoptimierung der Datenzugriffsschicht, für die er eine zum Patent angemeldete Technologie für ihre Analyse entwickelt hat, Business Intelligence und Data Warehousing für SaaS-Plattformen sowie Datenkonnektivität für PaaS-Umgebungen mit Schwerpunkt auf Standards wie ODBC, JDBC, ADO.NET und ODATA. Er ist IBM Certified Consultant für IBM Cognos Business Intelligence und TDWI-Mitglied. Er hat Sessions über Datenkonnektivität auf verschiedenen Konferenzen präsentiert, darunter Dreamforce, Oracle OpenWorld, Strata Hadoop, MongoDB World und SAP Analytics and Business Objects Conference. Twitter: @SAsInSumit LinkedIn: [www.linkedin.com/in/meetsumit](http://www.linkedin.com/in/meetsumit)

Veröffentlicht am _2015-12-07_ von _Kenny_

## Erstellen eines Dashboards zur API-Nutzung und zur Fehlerzählung

Als Marketo-API-Kunde sind dies nützliche Informationen, die Sie im Auge behalten sollten. Was wäre, wenn Sie historische Nutzungsdaten erhalten könnten, mit deren Hilfe Trends im Zeitverlauf erkannt werden? Wie wäre es, wenn Sie eine Zusammenfassung der API-Fehler-Codes erhalten könnten, um den Zustand Ihrer Integration zu messen? Was wäre, wenn Sie als Marketo-Technologiepartner Nutzungs- und Fehlerdaten aus allen Ihren Kundenkonten in einem Dashboard abrufen könnten? Dieser Beitrag bietet einen Ansatz zur Beantwortung der oben genannten Fragen. Schnallen Sie sich an, los geht&#39;s!
**Geplanter Auftrag für den**: Erstellen wir eine App, die Nutzungs- und Fehlerdaten mit den Endpunkten [Tägliche Nutzung abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLast7DaysErrorsUsingGET) und [Tägliche Fehler abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyErrorsUsingGET) abruft. Die App ist so konzipiert, dass sie einmal pro Tag ausgeführt wird. Bei jeder Ausführung der App werden die Nutzungsdaten eines Tages an eine Datei und die Fehlerdaten eines Tages an eine andere Datei angehängt. Zu Beginn jedes Monats wird ein neues Dateipaar erstellt. Diese Dateien dienen als historische Aufzeichnung, auf die wir jederzeit zugreifen können. Hier ist die App-Logik …

* Lesen Sie Marketo-Kontoinformationen (Munchkin-ID und Client-Anmeldedaten) aus einer externen Quelle. Hinweis: Diese Quelle muss sicher sein, damit andere nicht auf Kontodaten zugreifen können.
* Iterieren Sie durch jedes Konto und…
   * Aufruf „Get Daily Usage“, um Nutzungsdaten für einen Tag abzurufen
   * Hängen Sie tägliche Nutzungsdaten an eine monatliche Nutzungsdatei an
   * Rufen Sie Tägliche Fehler abrufen auf, um Fehlerdaten für einen Tag abzurufen
   * Hängen Sie tägliche Fehlerdaten an eine monatliche Fehlerdatei an

Ausgabedateiformat Das Format für die Ausgabedateien ist JSON, das mit dem von den jeweiligen API-Aufrufen zurückgegebenen „Ergebnis“-Array übereinstimmt (Nutzung und Fehler). Jedes Element des Arrays „result“ ist ein JSON-Objekt, das Daten für einen Tag enthält. Benennung der Ausgabedatei Die Ausgabedateien erhalten folgende Namen:

`<type>_<yyyy>_<mm>_<account>.json`

Dabei gilt Folgendes:

`<type>` - Der Datentyp („Nutzung“ oder „Fehler„) `<yyyy>` - Das Jahr (4-stellig) `<mm>` - Der Monat (2-stellig) `<account>` - Die Konto-ID (Munchkin-ID)

Beispiele für Ausgabedateien usage_2015_10_111-AAA-222.json

```json
[
    { "date": "2015-10-15", "total": 0, "users" : [] },
    { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
    { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] },
]
```

`errors_2015_10_111-AAA-222.json:`

```json
[
    { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
    { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
    { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
]
```

Der Code für diese App wird am Ende dieses Posts (Stats.java) vorgestellt. **Stats Web Service** Jetzt brauchen wir also einen Weg, diese Daten in unseren Browser zu bekommen. Es wird vorgeschlagen, einen Webdienst zur Bereitstellung der Daten zu erstellen. Der Webservice nutzt die Ausgabedateien der App und gibt dann Daten in einem Formular, das leicht präsentiert werden kann, an den Browser zurück. Der Einfachheit halber und um die Richtlinienbeschränkungen gleichen Ursprungs zu umgehen, verwenden wir das [JSONP](https://en.wikipedia.org/wiki/JSONP)-Muster. Im Folgenden finden Sie die vorgeschlagene REST-Endpunktspezifikation für den externen Webservice: **URI**: /stats **Method**: GET

**Parameter**

**Beschreibung**

**Beispiel**

month

Daten für diesen Monat abrufen. Durch Kommata getrennte Liste der einzuschließenden Monate (2-stellige Darstellung). Standardwert ist alle Monate.

10,11

year

Abrufen von Daten für dieses Jahr. Durch Kommas getrennte Liste der einzuschließenden Jahre (4-stellige Darstellung). Standardwert ist alle Jahre.

2015

account

Daten für dieses Konto abrufen (Munchkin-ID).

111-AAA-222

Rückruf

Name der Funktion für den JSON-Inhalt.

processStats

Beispielanfrage

`GET //localhost:8080/stats?month=10&year=2015&account=111-AAA-222&callback=processStats`

Der Webdienst liest die Dateien „Nutzung“ und „Fehler“, kombiniert sie und gibt sie in diesem Format zurück:

```html
**<Name of Callback here>**

<Contents of Usage file here>,

<Contents of Error file here>
```

Dies ist ein JSONP-Callback mit zwei Argumenten. Das erste Argument ist das Nutzungs-„Ergebnis“-Array. Das zweite Argument ist das Fehlerergebnis-Array. Beispielantwort

```json
processStats(
    [
        { "date": "2015-10-15", "total": 0, "users" : [] },
        { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
        { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] }
    ],
    [
        { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
        { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
        { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
    ]
);
```

Wie Sie sehen können, hat der Webservice einfach den Inhalt der beiden Ausgabedateien aus unserer App eingeschlossen. Wir haben eine Pseudo-Webservice-Antwort mit &quot;[&quot; ](https://designer.mocky.io). Ein Beispiel für den Webservice, der als Pseudo verwendet wird[ finden Sie hier.](http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats) Erstellung dieses Web-Services bleibt als Übung für den Leser: **Dashboard Web Page** Jetzt brauchen wir nur noch eine Web-Seite, die unseren Web-Service aufruft und die Daten formatiert. Um das JSONP-Muster zu verwenden, müssen wir nur ein `<script>`-Tag hinzufügen, das den Webservice aufruft:

`<script src="http: //<hostname>/stats?month=10&year=2015&account=284-RPR-133&callback=processStats"></script>`

Dadurch wird der Antworttext des Webservices direkt in die HTML-Seite eingefügt. Anschließend fügen wir die JSONP-Rückruffunktion hinzu:

```javascript
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
;
```

Diese Funktion wird nach dem Aufruf des Webservices automatisch aufgerufen. In diesem Beispiel nennen wir für jedes Array einen einfachen JavaScript-„Variablen[Dumper“ namens „prettyPrint.js](https://github.com/padolsey-archive/prettyprint.js/tree/master). Die Funktion prettyPrint erzeugt einfach eine HTML-Tabelle mit dem Inhalt des -Arrays. Im Folgenden finden Sie einen Screenshot der HTML-Tabellen. Voilà, das ist unser Armaturenbrett! Zugegeben, das ist nicht sehr elegant, aber es sollte Ihnen eine Vorstellung davon geben, was möglich ist. Es gibt nichts, was Sie davon abhält, die Daten auf irgendeine Weise zu transformieren, sodass Sie Ihre eigenen auffälligen Visualisierungen erstellen können. Die HTML-Seite finden Sie unten (index.html). Sie können die obigen Tabellen in Ihrem Browser live anzeigen, indem Sie die folgenden Schritte ausführen:

1. Speichern einer lokalen Kopie von „index.html“
1. Speichern einer lokalen Kopie von prettyPrint.js
1. Öffnen Sie index.html in Ihrem Browser

Das war&#39;s also. Ich hoffe, dieser Beitrag hat Ihnen einige Ideen zur Überwachung Ihrer Marketo-API-Statistiken gegeben. Alles Gute zum Programmieren! **Stats.java**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import java.io.\*;
import java.lang.reflect.Array;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

import java.nio.file.Files;
import java.nio.file.Paths;

import javax.net.ssl.HttpsURLConnection;

public class Stats {
    //
    // Define Marketo instance meta data here. Each row contains three elements: Account Id, Client Id, Client Secret.
    //
    // Note: that this information would typically be stored read from an external source (i.e. database)
    //
    // For example:
    //
    // private static String final CUSTOM_SERVICE_DATA[][] = {
    // {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    // };
    //
    private static final String CUSTOM_SERVICE_DATA[][] = {
    };

    // Output directory for stats files
    private static final String OUTPUT_DIR = "C:stats";

public static void main(String[] args) {

    // Loop through each Marketo instance
    for (int i = 0; i < CUSTOM_SERVICE_DATA.length; i++) {

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA[i][0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
            baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[i][1], CUSTOM_SERVICE_DATA[i][2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose Get Last 7 Days Usage URL
        String usageUrl = String.format("%s/rest/v1/stats/usage/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Compose Get Last 7 Days Errors URL
        String errorsUrl = String.format("%s/rest/v1/stats/errors/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Process usage data
        JsonObject usageObj = JsonObject.readFrom(httpRequest("GET", usageUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (usageObj.get("result") != null) {
                updateFile(usageObj, "usage", CUSTOM_SERVICE_DATA[i][0]);
            }
        }

        // Process errors data
        JsonObject errorsObj = JsonObject.readFrom(httpRequest("GET", errorsUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (errorsObj.get("result") != null) {
                updateFile(errorsObj, "errors", CUSTOM_SERVICE_DATA[i][0]);
            }
        }
    }
    System.exit(0);
}

// Write yesterday's data to output file
private static void updateFile(JsonObject usageObj, String statsType, String account){

    JsonArray usageResultAry = usageObj.get("result").asArray();
    JsonObject yesterdayUsageResultObj = usageResultAry.get(1).asObject();

    String yesterdayDate = yesterdayUsageResultObj.get("date").asString();
    String[] yesterdayDateAry = yesterdayDate.split("[-]+");

    // "C:statsstats_yyyy_mm_account.json"
    String statsFile = String.format("%s%s_%s_%s_%s.json",
        OUTPUT_DIR, statsType, yesterdayDateAry[0], yesterdayDateAry[1], account);

    // Create file
    File file = new File(statsFile);
    try {
        if (file.createNewFile()) {
            // created new file, seed with empty array
            FileWriter fw = new FileWriter(file.getAbsoluteFile());
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[n]");
            bw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Read file
    String content = null;
    try {
        content = new String(Files.readAllBytes(Paths.get(statsFile)));
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Remove trailing "]", append new record, append trailing "]"
    content = content.substring(0, content.length() - 1);
    content += yesterdayUsageResultObj.toString();
    content += "n]";

    // Write file
    FileWriter fw = null;
    try {
        fw = new FileWriter(file.getAbsoluteFile());
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(content);
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// Perform HTTP request
private static String httpRequest(String method, String endpoint, String body) {
    String data = "";
    try {
        URL url = new URL(endpoint);
        HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
        urlConn.setDoOutput(true);
        urlConn.setRequestMethod(method);
        switch (method) {
            case "GET":
                break;
            case "POST":
                urlConn.setRequestProperty("Content-type", "application/json");
                urlConn.setRequestProperty("accept", "text/json");
                OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                wr.write(body);
                wr.flush();
                break;
            default:
                System.out.println("Error: Invalid method.");
                return data;
        }
        int responseCode = urlConn.getResponseCode();
        if (responseCode == 200) {
            InputStream inStream = urlConn.getInputStream();
            data = convertStreamToString(inStream);
        } else {
            System.out.println(responseCode);
            data = "Status:" + responseCode;
        }
    } catch (MalformedURLException e) {
        System.out.println("URL not valid.");
    } catch (IOException e) {
        System.out.println("IOException: " + e.getMessage());
        e.printStackTrace();
    }
    return data;
}

private static String convertStreamToString(InputStream inputStream) {
    try {
        return new Scanner(inputStream).useDelimiter("A").next();
    } catch (NoSuchElementException e) {
        return "";
    }
}
}
```

**index.html**

```html
<html>
<head>
<title>Marketo API Stats</title>
<!-- Browser JavaScript variable dumper                  -->
<!-- https://github.com/padolsey-archive/prettyprint.js  -->
<script src="prettyPrint.js"></script>
</head>
<body>

<h1>Marketo API Stats</h1>

<script>
// JSONP callback that uses prettyPrint to format API stats
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
};
</script>

<!-- Web service for you to implement as an exercise                                                        -->
<!-- <script src="http://localhost:8080/stats?month=10&account=111-AAA-222&callback=processStats"></script> -->

<!-- Mock web service that returns sample payload -->
<!-- http://www.mocky.io/                         -->
<script src="http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats"></script>"
</body>
</html>
```

Veröffentlicht am _22.10.2015_ von _David_

## Ausnahme- und Fehlerbehandlung bei der Marketo REST-API: Teil 3

Meistens sind Fehler, die von der Marketo-REST-API zurückerhalten wurden, nicht automatisch wiederherstellbar. Es gibt jedoch einige Fälle, in denen Sie automatisch eine Wiederherstellung durchführen oder sicherstellen können, dass Sie nie eine bestimmte Fehlerart sehen.

### Fehler bei der Anfragengröße

Wie wir im letzten Beitrag dieser Reihe gesehen haben, gibt Marketo den HTTP-Status-Code 414 aus, wenn Ihr URI 8 KB überschreitet, oder 413, wenn Ihr Anfragetext 1 MB überschreitet, oder 10 MB für Import-Lead. Obwohl 414-Einträge selten sind, werden sie möglicherweise angezeigt, wenn Sie den Filtertyp Leads abrufen nach verwenden, um Datensätze basierend auf 300 separaten GUIDs oder ähnlichen Kriterien anzufordern. Angenommen, Sie haben die folgende Anfrage: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?filterType=customGUID&fields=email`, company…firstName, lastName> Wenn Sie die Anfrage senden, gibt Marketo den Status 414 zurück, da der URI 8 KB überschreitet.

Um dies zu handhaben, müssen wir das Muster dieser Anfrage ändern und einen POST anstelle eines GETS senden, „_method=GET&quot; an den URI anhängen und die Abfragezeichenfolge im Anfragetext als eine x-www-form-urlencoded-Anfrage übergeben: URI: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?_method=GET>` Anfragetext: filterType=customGUID&amp;fields=email,company…firstName, lastName Anstatt diese Ausnahme aus der HTTP-Antwort abzufangen, können wir jedoch nur die Gesamtlänge der Anfrage zur Laufzeit überprüfen und dieses alternative Muster bereitstellen, wenn der URI 8 KB überschreitet. Alternativ können Sie die POST-Methode in allen Fällen für Batch-Abrufen von Datensätzen verwenden. Bei 413s können wir einem ähnlichen Muster folgen, indem wir die Länge des Anfragetexts beim Hinzufügen von Datensätzen während des Serialisierungsschritts überprüfen und die Anfrage in mehrere Teile aufteilen, wenn diese Grenze überschritten würde.

### Authentifizierungsfehler

Die nächste Klasse behebbarer Fehler bezieht sich auf die Authentifizierung. Wenn ein zuvor gültiges Zugriffstoken nach Ablauf des Ablaufzeitraums verwendet wird, gibt die erste Verwendung den Fehlercode 602 „Zugriffstoken abgelaufen“ zurück. Wenn Sie danach dasselbe Token verwenden, wird ein 601-Zeichen zurückgegeben: „Zugriffstoken ungültig“. Jede andere Verwendung einer Zeichenfolge, die kein gültiges Zugriffstoken für das Zielabonnement ist, führt zu einer 601. In beiden Fällen kann dieser Fehler aus wiederhergestellt werden, indem das neue Zugriffstoken erneut authentifiziert und mit einer Wiederholung der fehlgeschlagenen Anfrage übergeben wird.

### Zeitüberschreitungen

In sehr seltenen Fällen kann ein Aufruf nach Ablauf des 30-Sekunden-Timeout-Zeitraums die Meldung 604, „Zeitüberschreitung bei Anfrage“, zurückgeben. Bei Batch-Anfragen, wie Leads erstellen/aktualisieren, kann die Anfrage in kleinere Batches aufgeteilt und erneut versucht werden, bis der Erfolg zurückgegeben wird (wenn der Batch auf weniger als 100 Datensätze aufgeteilt wird und die Anfrage weiterhin eine Zeitüberschreitung aufweist, sollten Sie wahrscheinlich einen Support-Fall einreichen). Der häufigste andere Fall besteht bei Asset-Genehmigungsaufrufen, bei denen der aktuelle genehmigte Datensatz von einem anderen Benutzer oder Service gesperrt werden kann, z. B. bei einer [E-Mail](https://developer.adobe.com/marketo-apis/api/asset/approve-email-by-id/) oder [E-Mail-Vorlage](https://developer.adobe.com/marketo-apis/api/asset/approve-email-template-by-id/). In diesen Fällen sollte [exponentieller Backoff](https://en.wikipedia.org/wiki/Exponential_backoff) für weitere Versuche verwendet werden, damit vorhandene Sperren aufgelöst werden können. Schauen Sie in den nächsten Wochen noch einmal vorbei, um den letzten Teil der Serie zu sehen, in dem wir uns einige spezifische, nicht behebbare Fehler genauer ansehen werden.

Veröffentlicht am _2015-10-30_ von _Kenny_

## Marketo-Sicherheitsverbesserungen

Bei Marketo nehmen wir Sicherheit ernst. Im Rahmen einer **[branchenweiten Initiative](https://security.googleblog.com/2014/09/gradually-sunsetting-sha-1.html)** aktualisiert Marketo unsere Web-Authentifizierung und -Verschlüsselung, um unsere Sicherheitsmaßnahmen zu verbessern. Der Rollout ist für den 12. Dezember 2011 geplant. **Wer ist betroffen?** Nur eine geringe Anzahl von Benutzern ist betroffen, nur diejenigen, die über eine Integration mit Marketo aus einem System verfügen, das über 10 Jahre alt ist und vor kurzem nicht aktualisiert wurde. In **[dieser Liste](https://www.digicert.com/faq/sha2/sha-2-compatibility)** finden Sie weitere Informationen darüber, welche Systeme und Versionen unterstützt werden. **Die folgenden Benutzer sind nicht betroffen:**

* Endbenutzer, die über moderne Browser auf Marketo.com zugreifen ([ Liste](https://www.digicert.com/faq/sha2/sha-2-compatibility))
* Kunden, die Integrationspartner wie Informatica, Dell Boomi und Scribe verwenden.
* Kunden, die Launchpoint-Partner verwenden.

Alle anderen Marketo-Domains verwenden bereits SHA2-Zertifikate.

Veröffentlicht am _2015-11-18_ von _Kenny_

## Abrufen von Aktivitäten mit der REST-API

Aktivitäten sind ein Hauptobjekt in der Marketo-Plattform. Aktivitäten sind die Verhaltensdaten, die über jeden Web-Seitenbesuch, jede geöffnete E-Mail, jeden Besuch eines Webinars oder jede Teilnahme an einer Messe gespeichert werden. Ein gängiges Anwendungsbeispiel ist die Kombination von Aktivitätsdaten mit Daten aus anderen Teilen einer Organisation. Dieses Beispielprogramm enthält 6 Schritte:

1. Rufen Sie [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) auf, um eine Liste aller Aktivitätsdatensätze zu generieren, die zu einem bestimmten Datum/zu einer bestimmten Uhrzeit erstellt wurden. Wir verwenden einen Filter, um die Art der zurückgegebenen Aktivitätsdatensätze zu begrenzen.
1. Extrahieren Sie interessante Felder aus jedem Aktivitätsdatensatz.
1. Rufen Sie [Abrufen mehrerer Leads nach Filtertyp](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) auf, um eine Liste von Lead-Datensätzen zu generieren, die den Aktivitäten aus Schritt 1 entsprechen. Wir verwenden das in Schritt 2 aus dem Aktivitätsdatensatz extrahierte Feld „leadId“ als Filter, um anzugeben, welche Leads zurückgegeben werden.
1. Extrahieren Sie interessante Felder aus jedem Lead-Datensatz.
1. Verbinden Sie die Aktivitätsdaten aus Schritt 2 mit den Lead-Daten aus Schritt 4.
1. Wandeln Sie die Daten aus Schritt 5 in ein Format um, das von einem externen System genutzt werden kann.

Wie das folgende Diagramm zeigt, haben wir uns für dieses Beispiel entschieden, E-Mail-bezogene Aktivitäten zu erfassen.

**Programmeingabe** Standardmäßig geht das Programm einen Tag nach dem aktuellen Datum in der Zeit zurück, um nach Änderungen zu suchen. Sie können dieses Programm beispielsweise jeden Tag zur gleichen Zeit ausführen. Um einen längeren Zeitraum zu verwenden, können Sie die Anzahl der Tage als Befehlszeilenargument angeben, wodurch das Zeitfenster effektiv verlängert wird. Das Programm enthält mehrere Variablen, die Sie ändern können: CUSTOM_SERVICE_DATA - Dieses enthält Ihre benutzerdefinierten Marketo-Service-Daten (Konto-ID, Client-ID, Client-Geheimnis). READ_BATCH_SIZE - Anzahl der gleichzeitig abzurufenden Datensätze. Hier können Sie die Reaktion auf die Körpergröße einstellen. LEAD_FIELDS - Enthält eine Liste der Lead-Felder, die wir erfassen möchten. ACTIVITY_TYPES - Enthält eine Liste der Aktivitätstypen, die erfasst werden sollen.

**Programmlogik** Zunächst richten wir unser Zeitfenster ein, erstellen unsere REST-Endpunkt-URLs und erhalten unser Authentifizierungs-Zugriffstoken. Als Nächstes starten wir die Schleife „Paging-Token abrufen/Lead-Aktivitäten abrufen“, die ausgeführt wird, bis wir das Angebot an Aktivitäten erschöpft haben. Diese Schleife dient dem Abrufen von Aktivitätsdatensätzen und dem Extrahieren von relevanten Feldern aus diesen Datensätzen. Wir weisen Get Lead-Aktivitäten an, nur nach den folgenden Aktivitätstypen zu suchen:

* E-Mail übermittelt
* E-Mail abbestellen
* E-Mail öffnen
* Klicken Sie auf E-Mail.

Wir extrahieren die folgenden Interessenfelder aus jedem Aktivitätsdatensatz:

* leadId
* activityType
* activityDate
* primaryAttributeValue

Sie können eine beliebige Kombination von Aktivitätstypen und Aktivitätsfeldern auswählen, die Ihrem Zweck entspricht. Als Nächstes starten wir eine Schleife „Mehrere Leads abrufen nach Filtertyp“, die so lange läuft, bis der Vorrat an Leads erschöpft ist. Beachten Sie, dass wir den Parameter „filterType=id“ in Kombination mit einer Reihe von „filterValues“-Parametern verwenden, um die abgerufenen Lead-Datensätze auf die Leads zu beschränken, die mit den zuvor abgerufenen Aktivitäten verbunden sind. Wir extrahieren die folgenden Interessenfelder aus jedem Lead-Datensatz:

* firstName
* lastName
* E-Mail

Auch hier haben Sie das Gefühl, alle Lead-Felder auszuwählen, die Ihnen gefallen. Als Nächstes verbinden wir die Lead-Felder mit den Aktivitätsfeldern, indem wir die Lead-ID verwenden, um sie miteinander zu verknüpfen. Schließlich durchlaufen wir alle Daten, wandeln sie in das JSON-Format um und drucken sie in der Konsole aus. **Programmausgabe** Im Folgenden finden Sie ein Beispiel für die Ausgabe aus dem Beispielprogramm. Hier werden die Aktivitätsfelder und die Lead-Felder zusammen als JSON-„Ergebnis“-Objekte angezeigt. Der Grundgedanke hier ist, dass Sie diese JSON-Datei als Anfrage-Payload an einen externen Webservice übergeben können.

```json
{
    "result": [
        {
            "leadId": 318581,
            "activityType": "Email Delivered",
            "activityDate": "2015-03-17T20:00:06Z",
            "primaryAttributeValue": "My Email Program",
            "firstName": "David",
            "lastName": "Everly",
            "email": "everlyd@marketo.com"
        },
        {
            "leadId":318581,
            "activityType":"Open Email",
            "activityDate":"2015-03-17T23:23:12Z",
            "primaryAttributeValue":"My Email Program - Auto Response",
            "firstName":"David",
            "lastName":"Everly",
            "email":"everlyd@marketo.com"
       },
        ... more result objects here...
    ]
}
```

**Programm-Code**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadActivities {
//
// Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    private static Map<String, String> CUSTOM_SERVICE_DATA;
    static {
        CUSTOM_SERVICE_DATA = new HashMap<String, String>();
//        CUSTOM_SERVICE_DATA.put("accountId", "111-AAA-222");
//        CUSTOM_SERVICE_DATA.put("clientId", "2f4a4435-f6fa-4bd9-3248-098754982345");
//        CUSTOM_SERVICE_DATA.put("clientSecret", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W");
    }

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Lookup lead records using lead id as primary key
    private static final String LEAD_FILTER_TYPE = "id";

    // Lead record lookup returns these fields
    private static final String LEAD_FIELDS = "firstName,lastName,email";

    // Lookup activity records for these activity types
    private static Map<Integer, String> ACTIVITY_TYPES;
    static {
        ACTIVITY_TYPES = new HashMap<Integer, String>();
        ACTIVITY_TYPES.put(7, "Email Delivered");
        ACTIVITY_TYPES.put(9, "Unsubscribe Email");
        ACTIVITY_TYPES.put(10, "Open Email");
        ACTIVITY_TYPES.put(11, "Click Email");
    }

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA.get("accountId"));

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA.get("clientId"), CUSTOM_SERVICE_DATA.get("clientSecret"));

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String activityTypesUrl = String.format("%s/rest/v1/activities/types.json?access_token=%s",
                baseUrl, accessToken);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        // Use activity ids to create filter parameter
        String activityTypeIds = "";
        for (Integer id : ACTIVITY_TYPES.keySet()) {
            activityTypeIds += "&activityTypeIds=" + id.toString();
        }

        // Compose URL for Get Lead Activities
        // Only retrieve activities that match our defined activity types
        String leadActivitiesUrl = String.format("%s/rest/v1/activities.json?access_token=%s%s&batchSize=%s",
                baseUrl, accessToken, activityTypeIds, READ_BATCH_SIZE);

        Map<Integer, List> activitiesMap = new HashMap<Integer, List>();
        Set leadsSet = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {
            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;
            do {
                moreResult = false;

                // Call Get Lead Activities API to retrieve activity data
                JsonObject leadActivitiesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadActivitiesUrl, nextPageToken)));
                if (leadActivitiesObj.get("success").asBoolean()) {
                    moreResult = leadActivitiesObj.get("moreResult").asBoolean();
                    nextPageToken = leadActivitiesObj.get("nextPageToken").asString();

                    if (leadActivitiesObj.get("result") != null) {
                        JsonArray activitiesResultAry = leadActivitiesObj.get("result").asArray();
                        for (JsonValue activitiesResultObj : activitiesResultAry) {

                            // Extract activity fields of interest (leadID, activityType, activityDate, primaryAttributeValue)
                            JsonObject leadObj = new JsonObject();
                            int leadId = activitiesResultObj.asObject().get("leadId").asInt();
                            leadObj.add("activityType", ACTIVITY_TYPES.get(activitiesResultObj.asObject().get("activityTypeId").asInt()));
                            leadObj.add("activityDate", activitiesResultObj.asObject().get("activityDate").asString());
                            leadObj.add("primaryAttributeValue", activitiesResultObj.asObject().get("primaryAttributeValue").asString());

                            // Store JSON containing activity fields in a map using lead id as key
                            List leadLst = activitiesMap.get(leadId);
                            if (leadLst == null) {
                                activitiesMap.put(leadId, new ArrayList());
                                leadLst = activitiesMap.get(leadId);
                            }
                            leadLst.add(leadObj);

                            // Store unique lead ids for use as lead filter value below
                            leadsSet.add(leadId);
                        }
                    }
                }
            } while (moreResult);
        }

        // Use unique lead id values to create filter parameter
        String filterValues = "";
        for (Object object : leadsSet) {
            if (filterValues.length() > 0) {
                filterValues += ",";
            }
            filterValues += String.format("%s", object);
        }

        // Compose Get Multiple Leads by Filter Type URL
        // Only retrieve leads that match the list of lead ids that was accumulated during activity query
        String getMultipleLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=%s&fields=%s&filterValues=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_FILTER_TYPE, LEAD_FIELDS, filterValues, READ_BATCH_SIZE);
        String nextPageToken = "";

        do {
            String gmlUrl = getMultipleLeadsUrl;

            // Append paging token to Get Multiple Leads by Filter Type URL
            if (nextPageToken.length() > 0) {
                gmlUrl = String.format("%s&nextPageToken=%s", getMultipleLeadsUrl, nextPageToken);
            }

            // Call Get Multiple Leads by Filter Type API to retrieve lead data
            JsonObject multipleLeadsObj = JsonObject.readFrom(getData(gmlUrl));
            if (multipleLeadsObj.get("success").asBoolean()) {
                if (multipleLeadsObj.get("result") != null) {
                    JsonArray multipleLeadsResultAry = multipleLeadsObj.get("result").asArray();

                    // Iterate through lead data
                    for (JsonValue leadResultObj : multipleLeadsResultAry) {
                        int leadId = leadResultObj.asObject().get("id").asInt();

                        // Join activity data with lead fields of interest (firstName, lastName, email)
                        List leadLst = activitiesMap.get(leadId);
                        for (JsonObject leadObj : leadLst) {
                            leadObj.add("firstName", leadResultObj.asObject().get("firstName").asString());
                            leadObj.add("lastName", leadResultObj.asObject().get("lastName").asString());
                            leadObj.add("email", leadResultObj.asObject().get("email").asString());
                        }
                    }
                }
            }

            nextPageToken = "";
            if (multipleLeadsObj.asObject().get("nextPageToken") != null) {
                nextPageToken = multipleLeadsObj.get("nextPageToken").asString();
            }
        } while (nextPageToken.length() > 0);

        // Now place activity data into an array of JSON objects
        JsonArray activitiesAry = new JsonArray();
        for (Map.Entry<Integer, List> activity : activitiesMap.entrySet()) {
            int leadId = activity.getKey();
            for (JsonObject leadObj : activity.getValue()) {
                // do something with leadId and each leadObj
                leadObj.add("leadId", leadId);
                activitiesAry.add(leadObj);
            }
        }

        // Print out result objects
        JsonObject result = new JsonObject();
        result.add("result", activitiesAry);
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Das war&#39;s. Alles Gute zum Programmieren!

Veröffentlicht am _2015-11-_) von _David_

## Integrieren des SoundCloud-Players in die Munchkin-API

SoundCloud bietet eine unglaubliche Audio-Hosting-Plattform mit umfangreichen Analysen und Funktionen für alles, von aufstrebenden Indie-Rock-Acts über EDM-Künstler an der Spitze der Musikindustrie bis hin zu erzählerischen Podcasts. Zusammen mit der unglaublichen nativen Funktionalität der Plattform kommt ein erstklassiges API-Programm, um Ihre Daten zu verschieben und das Hörverhalten zu verfolgen. Dies ist besonders für Podcaster nützlich und ermöglicht es Ihnen, bestimmte Höraktionen wie Wiedergaben, Pausen und Freigaben mit bestimmten Inhalten im Skript und im Audio zu korrelieren. Heute werfen wir einen Blick auf die Nutzung der [Widget-API von SoundCloud](https://developers.soundcloud.com/docs/api/html5-widget) um diese Aktivitäten in Marketo zu senden und zu verfolgen. Als Erstes sehen wir uns an, wie eine Munchkin-Aktivität erzeugt wird, die in der Marketo-Aktivität eines Leads aufgezeichnet wird. In seiner grundlegendsten Form rufen wir Munchkin.munchkinFunction auf und übergeben „visitWebPage“ als erstes Argument. Dadurch wird eine Besuchs-Web-Seitenaktivität mit Marketo protokolliert und alle beliebigen URL- und Abfragezeichenfolgendaten aufgezeichnet, die wir an die -Methode übergeben. Das zweite Argument akzeptiert ein JavaScript-Objekt mit unseren Daten, das zwei Member, „url“ und „params“, beide Zeichenfolgen hat. Das URL-Element entspricht der Web-Seite der Aktivität in Marketo, während die Parameter der Abfragezeichenfolge entsprechen. Für unsere Zwecke verwenden wir die URL als Kennung für SoundCloud-bezogene Aktionen, „soundCloudInteraction“, während Parameter zusätzliche Daten über die bestimmte Aktivität enthalten. Hier ist die Funktion, mit der wir jede Aktion verfolgen:

```javascript
var trackActivity = function(action){

    //set action param to be the string passed to the function
    var qs = "action=" + action;

    //use getCurrentSound callback to get the name of the current track
    soundCloudMunchkin.widget.getCurrentSound(function(currentSound){

        //add it to our querystring
        qs = qs + "&sound=" + currentSound.title;

        //use the getPosition callback to get the position of the track in ms
        soundCloudMunchkin.widget.getPosition(function(position){

            //add it to the querystring
            qs = qs + "&position=" + position;

            //assemble our data object for the munchkin activity
            var dataObject = {
                "url": "soundCloudInteraction",
                "params": qs
            }

            //call the munchkinFunction to submit the activity
            Munchkin.munchkinFunction("visitWebPage", dataObject);
        });
    });
}
```

Da das Standard-SoundCloud-Widget in einen iframe eingebettet ist, verwendet das Widget POST-Nachrichten zur Kommunikation, und es müssen Rückrufe verwendet werden, um Daten abzurufen, wie Sie mit der CurrentSound-Methode und der getPosition-Methode sehen können. Die SoundCloud-Widget-API bietet eine Reihe von JavaScript-Callbacks, mit denen wir auf einzelne Ereignisse im Player reagieren und sie an Marketo senden können. Die Ereignisse, an denen wir interessiert sind, sind, was der Benutzer hört, wie lange der Benutzer zuhört und Interaktionen, die der Benutzer mit dem Player macht. Daher sehen wir uns die folgenden Ereignisse an:

* PLAY
* PAUSIEREN
* BEENDEN
* SUCHEN
* CLICK_DOWNLOAD
* CLICK_BUY
* OPEN_SHARE_PANEL

Wir müssen auch die bind()-Methode aus dem Widget verwenden, um Callbacks zu jedem dieser Ereignisse hinzuzufügen. Sehen wir uns ein Beispiel an:

```javascript
widget.bind(SC.Widget.Events.PLAY, function(){
    soundCloudMunchkin.trackPlay();
});
```

Dadurch wird sichergestellt, dass bei jeder Wiedergabe eines Tracks die trackPlay-Methode ausgelöst wird, um ein Ereignis mit Daten zum aktuellen Track an Marketo zu senden. Das vollständige Script finden Sie [hier](https://gist.github.com/kelkingtron/6750bb07c1397d93d9c7#file-soundcloudmunchkin-js). Das SoundCloudMunchkin-Objekt verfügt über eine init-Methode, die ein SoundCloud-Widget-Objekt als einziges Argument akzeptiert, das die Tracking-Methoden an die entsprechenden Callbacks bindet, und Ihr Widget so einrichtet, dass es Aktivitäten bis zu Marketo verfolgt. Auf Ihrer Seite müssen Ihr [Munchkin-Code](/help/javascript-api/lead-tracking.md) sowie die [SoundCloud-API-Bibliothek) geladen ](https://w.soundcloud.com/player/api.js). Zusätzlich zum Einbetten des eigentlichen SoundCloud-Widgets müssen Sie auch alles initialisieren:

```javascript
window.onload=function(){
var iframe = document.getElementById(iframeId);
    if(iframe) {
        widget = SC.Widget(iframe);
        soundCloudMunchkin.init(widget);
    };
};
```

Veröffentlicht am _21.12.2015_ von _Kenny_

## RTP und die EU-Datenschutzrichtlinie für elektronische Kommunikation

In diesem Beitrag wird erläutert, wie Sie mit RTP Website-Besuchern mitteilen können, dass sie verfolgt werden, oder das Tracking für europäische Besucher automatisch deaktivieren können. Seit 2012 muss jede Website, die europäischen Besuchern zur Verfügung steht, der EU-Datenschutzrichtlinie für elektronische Kommunikation entsprechen. Im Jahr 2011 traten neue Gesetze in Kraft, die verhindern, dass identifizierende Informationen ohne deren Wissen und Zustimmung auf dem Computer eines Benutzers gespeichert werden. Wenn Sie Cookies oder andere Technologien für nicht erforderliches Tracking verwenden, müssen Sie:

1. Benutzern mitteilen, dass Tracking-Technologien verwendet werden.
1. Erläutern Sie die Gründe für die Verwendung dieser Technologien.
1. Einholung der Einwilligung des Benutzers vor der Nutzung dieser Technologie und Erlaubnis zum Widerruf der Erlaubnis jederzeit.

Veröffentlicht am _1970-01-01_ von _Yanir_

## Aktualisieren von Kunden- und Interessenteninformationen in Marketo mithilfe der -API

Es gibt Szenarien, in denen proprietäre Systeme verwendet werden, um Kunden- und Interessenteninformationen zu aktualisieren. Das Marketing-Team möchte, dass diese Aktualisierungen in Marketo wiedergegeben werden, damit es über das genaueste Aufzeichnungssystem verfügt, das es für seine Marketing-Kampagnen verwenden kann. Mit dem folgenden Ansatz können Sie regelmäßige Uploads in Marketo einrichten, um Ihre Marketo-Kontaktinformationen mit den in diesem proprietären System geänderten Daten auf dem neuesten Stand zu halten. Das folgende Diagramm zeigt die API-Aufrufe, die an einem festgelegten periodischen Timer durchgeführt werden. Da der periodische Zeitgeber ausgelöst wird, ruft die Client-Logik zunächst aktualisierte Kontakte aus dem proprietären System ab. Die Vorgehensweise unterscheidet sich von System zu System bei Verwendung von APIs oder Datenexporten aus dem proprietären System. Wir werden die Marketo-APIs detailliert beschreiben, die ausgeführt werden, sobald die aktualisierten Kontaktinformationen abgerufen wurden. SOAP-Anfrage für syncMultipleLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadRecordList>
    <leadRecord>
      <Email>henry@superstar.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Henry</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Adams</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>Director of Demand Generation</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
    <leadRecord>
      <Email>ssmith@gmail.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Suzie</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Smith</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>VP Marketing</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
  </leadRecordList>
  <dedupEnabled>true</dedupEnabled>
</ns2:paramsSyncMultipleLeads>
```

SOAP-Antwort von syncMultipleLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
    <syncStatusList>
      <syncStatus>
        <leadId>1094593</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
      <syncStatus>
        <leadId>1094594</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
    </syncStatusList>
  </result>
</ns2:successSyncMultipleLeads>
```

`syncMultipleLeads` führt einen UPSERT-Vorgang aus. Wenn in Marketo bereits ein Kontakt basierend auf der gesendeten E-Mail-Adresse vorhanden ist, werden die Attribute aktualisiert. Wenn kein Kontakt vorhanden ist, wird er erstellt. Die Antwort von `syncMultipleLeads` gibt den Status für jeden gesendeten Kontakt zurück. Die `<attrName/>` Werte im `<leadAttributeList/>` müssen mit dem SOAP-API-Namen übereinstimmen, der für dieses Marketo-Abonnement definiert ist. Sie können die SOAP-API-Namen im Abschnitt Feldverwaltung im Admin-Bedienfeld von Marketo ermitteln, indem Sie die Feldnamen exportieren.

Siehe folgendes Java-Beispielprogramm, das das oben beschriebene Szenario ausführt:

```java
 import com.marketo.mktows.\*;
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
import java.util.\*;

public class SyncMultipleLeadsExample {

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
      ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

      ObjectFactory objectFactory = new ObjectFactory();

      JAXBElement dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
      request.setDedupEnabled(dedup);

      // The list of contacts defined here would be retrieved from the proprietary system
      Contact contact = new Contact("Henry","Adams","henry@superstar.com", "Director of Demand Generation");
      Contact contact2 = new Contact("Suzie","Smith","ssmith@gmail.com", "VP Marketing");

      ArrayList updatedContacts = new ArrayList();
      updatedContacts.add(contact);
      updatedContacts.add(contact2);

      ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

      Iterator it = updatedContacts.iterator();
      while(it.hasNext())
      {
        Contact c = it.next();

        LeadRecord leadRec = new LeadRecord();

        JAXBElement email = objectFactory.createLeadRecordEmail(c.email);
        leadRec.setEmail(email);

        Attribute attr1 = new Attribute();
        attr1.setAttrName("FirstName");
        attr1.setAttrValue(c.fname);

        Attribute attr2 = new Attribute();
        attr2.setAttrName("LastName");
        attr2.setAttrValue(c.lname);

        Attribute attr3 = new Attribute();
        attr3.setAttrName("Title");
        attr3.setAttrValue(c.title);

        ArrayOfAttribute aoa = new ArrayOfAttribute();
        aoa.getAttributes().add(attr1);
        aoa.getAttributes().add(attr2);
        aoa.getAttributes().add(attr3);

        QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
        JAXBElement attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

        leadRec.setLeadAttributeList(attrList);
        arrayOfLeadRecords.getLeadRecords().add(leadRec);

      }

      request.setLeadRecordList(arrayOfLeadRecords);

      JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
      Marshaller m = context.createMarshaller();
      m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
      m.marshal(request, System.out);

      SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

      m.marshal(result, System.out);
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }

  public static class Contact {
    public String fname, lname, email, title;

      public Contact(String fname, String lname, String email, String title) {
          this.fname = fname;
          this.lname = lname;
          this.email = email;
          this.title = title;
      }
  }
}
```

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-03-24_ von _Travis Kaufman_

## Senden einer Transaktions-E-Mail aus Marketo mithilfe der API

Dazu muss eine bestehende Smart Campaign über die Benutzeroberfläche von Marketo erstellt werden. Außerdem muss der E-Mail-Empfänger in Marketo vorhanden sein. Bevor Sie also die requestCampaign-API aufrufen, verwenden Sie die [getLead-API]&#x200B;(/help/soap-api/getlead.md), um zu überprüfen, ob die E-Mail in Marketo vorhanden ist. Nachdem Sie einen Aufruf über die requestCampaign-API durchgeführt haben, können Sie ihn bestätigen, indem Sie überprüfen, ob die Smart-Kampagne in Marketo ausgeführt wurde. Wir zeigen Ihnen zuerst, wie Sie eine Smart Campaign erstellen, zweitens, wie Sie einen Trigger einrichten, um eine Kampagne über die API zu senden, drittens, wie Sie eine E-Mail als Teil einer Flussaktion definieren, und viertens, welche Codebeispiele verwendet werden, um diese Kampagne auszuführen.
**Erstellen einer neuen Smart-Kampagne in Marketo** Smart-Kampagnen in Marketo führen alle Ihre Marketing-Aktivitäten aus. Sie können eine Reihe automatisierter Aktionen einrichten, die für eine Smart-Liste von Kontakten ausgeführt werden. Für den Versand von Transaktions-E-Mails richten Sie in der Kampagne einen Trigger ein, wie unten dargestellt, um E-Mails über die API zu senden. Richten wir zunächst die Smart Campaign ein. 1. Wählen Sie unter „Marketing-Aktivitäten“ ein Programm aus und klicken Sie dann in der Dropdown-Liste „Neu“ auf „Neues lokales Asset“.

1. Klicken Sie auf Smart Campaign
1. Geben Sie einen Smart-Kampagnennamen ein und klicken Sie auf Erstellen

**Trigger zu einer Smart-Kampagne hinzufügen** Durch das Hinzufügen von Triggern zu einer Smart-Kampagne können Sie eine Smart-Kampagne auf der Grundlage eines Live-Ereignisses jeweils nur für eine Person ausführen. In diesem Fall handelt es sich um eine Anfrage über die [requestCampaign-API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST). 1. Suchen Sie nach dem Trigger „Kampagne ist angefordert“ und ziehen Sie ihn dann per Drag-and-Drop auf die Arbeitsfläche.

1. Wählen Sie im Trigger „is“ und „Web Service API“ aus.

**Erstellen einer E-Mail-Flussaktion für eine Kampagne** Die Verknüpfung einer E-Mail mit einer Smart-Kampagne ermöglicht es Marketing-Experten, zu verwalten, wie eine E-Mail aussehen soll, und ermöglicht dem Drittanbieterprogramm zu bestimmen, wer sie wann erhält. Nachdem Sie eine E-Mail als neues lokales Asset erstellt haben, können Sie sie als Flussaktion in einer Kampagne festlegen.  Suchen Sie die E-Mail, die Sie senden möchten, und wählen Sie sie aus.

**Codebeispiel zum Aufrufen der requestCampaign-API** Nachdem Sie die Kampagne und die Trigger in der Marketo-Benutzeroberfläche eingerichtet haben, zeigen wir Ihnen, wie Sie die API zum Senden einer E-Mail verwenden. Das erste Beispiel ist eine XML-Anfrage, das zweite eine XML-Antwort und das letzte Beispiel ist ein Java-Codebeispiel, das zum Generieren der XML-Anfrage verwendet werden kann. Außerdem erfahren Sie, wie Sie die Kampagnen-ID finden, die beim Aufruf der `requestCampaign`-API verwendet wird.
Für den API-Aufruf müssen Sie außerdem die ID der Marketo-Kampagne im Voraus kennen. Sie können die Kampagnen-ID mit einer der folgenden Methoden ermitteln: 1. Verwenden Sie die [getCampaignForSource](/help/soap-api/getcampaignsforsource.md) API 1. Öffnen Sie die Marketo-Kampagne in einem Browser und sehen Sie sich die URL-Adressleiste an. Die Kampagnen-ID (dargestellt als 4-stellige Ganzzahl) ist unmittelbar nach „SC“ zu finden. Beispiel: `<https://app-stage.marketo.com/#SC**1025**A1>`. Der fett gedruckte Teil ist die Kampagnen-ID „1025“. SOAP-Anfrage für `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-Antwort für requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Unten finden Sie ein Java-Beispielprogramm, das das oben beschriebene Szenario ausführt.

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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-03-27_ von _Murta_

## Senden einer E-Mail mit dynamischen Inhalten von Marketo mithilfe des

Angenommen, Sie möchten Ihre Callcenter-Follow-up-E-Mails automatisieren. Nachdem Ihr Support-Mitarbeiter mit einem Kunden gesprochen hat, möchten Sie ihm automatisch eine E-Mail senden, in der er sich für die Kontaktaufnahme mit Ihrem Unternehmen bedankt. Gehen wir einen Schritt weiter und sagen, Sie möchten das spezifische Gesprächsthema einbeziehen, das mit dem Kunden besprochen wurde und das Sie in Ihrem CRM nachverfolgen. Dies können Sie über Marketo mithilfe der requestCampaign-SOAP-API tun, um eine E-Mail mit dynamischen Inhalten zu senden. Mit der requestCampaign-API können Sie einen Lead oder Leads übergeben. Außerdem können Sie Programm-Token übergeben, die mit einer vorhandenen Kampagne zum Senden dynamischer Inhalte verwendet werden können. Für die SOAP-API von requestCampaign muss der E-Mail-Empfänger in Marketo vorhanden sein. Bevor Sie also die requestCampaign-API aufrufen, verwenden Sie die [getLead-API](/help/soap-api/getlead.md), um zu überprüfen, ob die E-Mail in Marketo vorhanden ist. Wir zeigen Ihnen zuerst, wie Sie eine Smart Campaign erstellen, zweitens, wie Sie einen Trigger einrichten, um eine Kampagne über die API zu senden, drittens, wie Sie eine E-Mail erstellen, die dynamische Inhalte über Programm-Token akzeptiert, viertens, wie Sie eine E-Mail als Teil einer Flussaktion definieren, und fünftes Code-Beispiel, das zur Ausführung dieser Kampagne verwendet würde. **Erstellen einer neuen Smart-Kampagne in Marketo** Smart-Kampagnen in Marketo führen alle Ihre Marketing-Aktivitäten aus. Sie können eine Reihe automatisierter Aktionen einrichten, die für eine Smart-Liste von Kontakten ausgeführt werden. Für den Versand von Transaktions-E-Mails richten Sie in der Kampagne einen Trigger ein, wie unten dargestellt, um E-Mails über die API zu senden. Richten wir zunächst die Smart Campaign ein. 1. Wählen Sie unter „Marketing-Aktivitäten“ ein Programm aus und klicken Sie dann in der Dropdown-Liste „Neu“ auf „Neues lokales Asset“

1. Klicken Sie auf Smart Campaign
1. Geben Sie den Namen der Smart-Kampagne ein und klicken Sie auf Erstellen **Trigger zu einer Smart-Kampagne hinzufügen** Wenn Sie Trigger zu einer Smart-Kampagne hinzufügen, können Sie eine Smart-Kampagne auf der Grundlage eines Live-Ereignisses einzeln ausführen. In diesem Fall handelt es sich um eine Anfrage über die [requestCampaign-API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST).
1. Suchen Sie nach dem Trigger „Kampagne ist angefordert“ und ziehen Sie ihn dann per Drag-and-Drop auf die Arbeitsfläche.
1. Wählen Sie im Trigger „is“ und „Web Service API“ aus.

**Übergeben dynamischer Inhalte mithilfe der API** In Marketo sind „Meine Token“ Variablen, die Sie in Ihrem Programm verwenden können. Mit „Meine Token“ können Sie Informationen zu Ihrem Programm an einer Stelle eingeben, durch einen von Ihnen angegebenen Wert ersetzen und diese Informationen in anderen Teilen des Programms abrufen, z. B. in einer E-Mail-Vorlage. Mit der requestCampaign SOAP-API können Sie ein Array von Programm-Token übergeben, mit denen vorhandene Token überschrieben werden. Nach der Ausführung der Kampagne werden die Token verworfen. Meine Token werden entweder auf Kampagnenordnerebene oder auf Programmebene erstellt. Meine Token auf der Kampagnenordnerebene übernehmen alle Programme, die im Kampagnenordner enthalten sind. Wenn Sie Meine Token auf der Kampagnenordnerebene erstellen, können Sie den übernommenen Wert auf Programmebene überschreiben. Wenn Sie beispielsweise auf der Kampagnenordnerebene Token für das Programmdatum und die Programmbeschreibung definieren, können Sie diese Werte auf der einzelnen Programmebene überschreiben.

Gehen Sie wie folgt vor: 1. Wählen Sie im Baum der Marketing-Aktivitäten den Kampagnenordner oder das Programm aus, in dem Sie die Token erstellen möchten. Wählen Sie in der oberen Menüleiste Meine Token aus. Dann wird die Arbeitsfläche Meine Token angezeigt. Ziehen Sie in der Struktur auf der rechten Seite einen Token-Typ auf die Arbeitsfläche, in diesem Fall „Text“. Markieren Sie im Feld „Token-Name“ Mein Token und geben Sie einen eindeutigen Token-Namen ein, in diesem Fall „my.conversionTopic“. Geben Sie im Feld Wert einen relevanten Wert für das Token ein, in diesem Fall „Vielen Dank, dass Sie uns heute angerufen haben“. Beachten Sie, dass wir durch Verwendung der API den Standardwert für „Mein Token“ überschreiben. Klicken Sie auf „Speichern“, um das benutzerdefinierte Token zu speichern.  1. Erstellen Sie eine neue E-Mail durch Klicken auf Neu. Klicken Sie dann auf Neue lokale Assets und wählen Sie E-Mail aus. Füllen Sie anschließend die entsprechenden Felder aus, um Ihre E-Mail zu benennen. Klicken Sie beim Entwerfen Ihrer E-Mail auf das Token-Symbol, um Token in Ihre E-Mail aufzunehmen. Nachdem Sie nun Ihre E-Mail-Vorlage mit Token erstellt haben, fügen wir im nachfolgenden Schritt die E-Mail als Flussaktion für die Kampagne hinzu. Wenn Sie die Kampagne also über die API aufrufen, wird die E-Mail gesendet.
**Erstellen einer E-Mail-Flussaktion für eine Kampagne** Die Verknüpfung einer E-Mail mit einer Smart-Kampagne ermöglicht es Marketing-Experten, zu verwalten, wie eine E-Mail aussehen soll, und ermöglicht dem Drittanbieterprogramm zu bestimmen, wer sie wann erhält. Nachdem Sie eine E-Mail als neues lokales Asset erstellt haben, können Sie sie als Flussaktion in einer Kampagne festlegen. Suchen Sie die E-Mail, die Sie senden möchten, und wählen Sie sie aus.
**Codebeispiel zum Aufrufen der requestCampaign-API** Nachdem Sie die Kampagne und die Trigger in der Marketo-Benutzeroberfläche eingerichtet haben, zeigen wir Ihnen, wie Sie die API zum Senden einer E-Mail verwenden. Das erste Beispiel ist eine XML-Anfrage, das zweite eine XML-Antwort und das letzte Beispiel ist ein Java-Codebeispiel, das zum Generieren der XML-Anfrage verwendet werden kann. Außerdem erfahren Sie, wie Sie die Kampagnen-ID finden, die beim Aufruf der requestCampaign-API verwendet wird. Für den API-Aufruf müssen Sie außerdem die ID der Marketo-Kampagne im Voraus kennen. Sie können die Kampagnen-ID mit einer der folgenden Methoden ermitteln: 1. Verwenden Sie die [getCampaignForSource](/help/soap-api/getcampaignsforsource.md) API 1. Öffnen Sie die Marketo-Kampagne in einem Browser und sehen Sie sich die URL-Adressleiste an. Die Kampagnen-ID (dargestellt als 4-stellige Ganzzahl) ist unmittelbar nach „SC“ zu finden. Beispiel: `<https://app-stage.marketo.com/#SC&#x200B;**1025**&#x200B;A1>`. Der fett gedruckte Teil ist die Kampagnen-ID „1025“. SOAP-Anfrage für requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
      </leadList>
      <programTokenList>
        <attrib>
          <name>{{my.conversationtopic}}</name>
          <value>Thank you for calling about adding a line of service to your current plan.</value>
        </attrib>
      </programTokenList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-Antwort für requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Unten finden Sie ein Java-Beispielprogramm, das das oben beschriebene Szenario ausführt.

```java
import com.marketo.mktows.\*;
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


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
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
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            leadKeyList.getLeadKeies().add(key);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            ArrayOfAttrib aoa = new ArrayOfAttrib();

            Attrib attrib = new Attrib();
            attrib.setName("{{my.conversationtopic}}");
            attrib.setValue("Thank you for calling about adding a line of service to your current plan.");

            aoa.getAttribs().add(attrib);

            JAXBElement<ArrayOfAttrib> arrayOfAttrib = objectFactory.createParamsRequestCampaignProgramTokenList(aoa);
            request.setProgramTokenList(arrayOfAttrib);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
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

Nachdem Sie einen Aufruf über die requestCampaign-API durchgeführt haben, können Sie ihn bestätigen, indem Sie überprüfen, ob die Smart-Kampagne in Marketo ausgeführt wurde.

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-04-03_ von _Murta_

## Erfassung anonymer Besucheraktivitäten basierend auf Geschäftslogik

Angenommen, Sie möchten Benutzer tracken, die einen bestimmten Beitrag auf Ihrem Unternehmensblog besuchen. Nehmen wir an, dass Sie anhand der Gesamtzahl der Benutzer, die einen Beitrag besuchen, nur diejenigen verfolgen möchten, die ihr Interesse signalisieren, indem Sie mindestens 5 Sekunden damit verbringen und die Seite nach unten scrollen. Für anonyme Benutzer möchten Sie mit diesem Ereignis einen neuen Lead in Marketo erstellen. Für bekannte Benutzer möchten Sie ihre Lead-Aktivität mit diesem Ereignis aktualisieren. Verwenden Sie dazu den [Munchkin-Trackingcode](/help/javascript-api/lead-tracking.md) auf Ihrer Website. Wenn ein nicht-Cookie-Benutzer auf eine Seite mit dem Munchkin-Trackingcode wechselt, wird ein neues Cookie im Browser des Benutzers erstellt und in Marketo wird ein neuer anonymer Lead erstellt. Wenn der Benutzer bereits Cookies hat und er bereits ein Lead in Marketo ist, wird der Besuch der Seite im Aktivitätsprotokoll des Benutzers in Marketo aufgezeichnet. Wir zeigen Ihnen zunächst, wie Sie Munchkin-Trackingcode in Marketo generieren, zweitens, wie Sie Ihren Munchkin-Beispielcode so ändern, dass er nur dann auf Trigger verweist, wenn bestimmte Bedingungen erfüllt sind, und drittens, wie Sie überprüfen können, ob ein Seitenbesuch eines anonymen Benutzers in Marketo aufgezeichnet wurde.

**Generieren von Munchkin-Trackingcode** Mit dem Munchkin-Trackingcode können Sie Besuche auf Ihrer Website verfolgen. Im Folgenden werden drei Typen von Munchkin-Code beschrieben. In diesem Beispiel verwenden wir jedoch den asynchronen Munchkin-Trackingcode. A) Einfach: Verfügt über die wenigsten Codezeilen, wird aber nicht für die Ladezeit der Web-Seite optimiert. Dieser Code lädt die jQuery-Bibliothek jedes Mal, wenn eine Web-Seite geladen wird. B) Asynchron: Verringert die Ladezeit der Web-Seite. Dieser Code prüft, ob die jQuery-Bibliothek bereits vorhanden ist, lädt sie, falls sie fehlt, und verwendet sie für die Ausführung von Trackingcode, sobald der Rest der Web-Seite geladen wurde. C) Asynchrone jQuery: verringert die Ladezeit der Web-Seite und verbessert auch die Systemleistung. In diesem Code wird davon ausgegangen, dass Sie bereits über jQuery verfügen, und es wird nicht geprüft, ob es geladen wird. 1. Klicken Sie oben rechts in der App auf Admin .  1. Klicken Sie in der Baumstruktur links auf Munchkin .  1. Wählen Sie „Asynchron“ für den Trackingcode-Typ. 1. Klicken Sie auf den Trackingcode von JavaScript und kopieren Sie ihn auf Ihre Website.
**Code-Beispiel für Cookie-Benutzer und Tracking-**: Platzieren Sie den Tracking-Code auf Ihren Web-Seiten direkt vor dem `</body>`-Tag. In Marketo erstellte Landingpages enthalten automatisch Trackingcode, sodass Sie diesen Code nicht auf sie anwenden müssen. Dieses Codebeispiel ruft die Munchkin-API auf, nachdem das Skript geladen wurde:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Dieses Codebeispiel ruft die Munchkin-API auf, nachdem der/die Benutzende 5 Sekunden lang auf der Seite war und auch 500 Pixel nach unten gescrollt hat:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Überprüfen, ob der Seitenbesuch eines anonymen Benutzers in Marketo aufgezeichnet wurde**

1. Klicken Sie im oberen Menü auf Analytics und dann auf Neuer Bericht . Wählen Sie Webseitenaktivität als Berichtstyp aus und benennen Sie dann Ihren Bericht.
1. Nachdem Sie einen Bericht erstellt haben, klicken Sie auf Smart-Liste. Wählen Sie dann im Feld rechts den Filter Besuchte Web-Seite aus. Geben Sie die Web-Seite ein, auf der Sie den Munchkin-Trackingcode ablegen.
1. Klicken Sie auf Setup. Wählen Sie anonyme Besucher von ISPs aus und ändern Sie die Option in Angezeigt.
1. Klicken Sie auf Bericht. Die Aktivität wird nun auf der ausgewählten Web-Seite verfolgt.
1. Doppelklicken Sie auf den Lead-Eintrag, der dann das Aktivitätsprotokoll anzeigt, in dem die spezifische Seite angezeigt wird, die der anonyme Benutzer besucht hat.

Dieser Artikel enthält Code zum Implementieren benutzerdefinierter Integrationen. Aufgrund seiner Anpassung kann das technische Supportteam von Marketo keine Fehlerbehebung bei benutzerdefinierten Arbeiten durchführen. Bitte versuchen Sie nicht, das folgende Codebeispiel ohne entsprechende technische Erfahrung oder Zugang zu einem erfahrenen Entwickler zu implementieren.

Veröffentlicht am _2014-04-17_ von _Murta_

## Dynamisches Ändern der lokalen Telefonnummer mithilfe von RTP

Personalization ist alles - das haben wir schon vor langer Zeit herausgefunden. Es überrascht mich noch immer, dass es immer so schwierig ist, auf einer Webseite die richtigen örtlichen Telefonnummern zu finden, wenn ich sofortige Hilfe brauche. Gut, dass wir [Marketo Real-Time Personalization](https://business.adobe.com/products/marketo/content-personalization.html) (RTP) auf <https://business.adobe.com/products/marketo/adobe-marketo.html> installiert haben. Wir können die [RTP Visitor API](/help/javascript-api/web-personalization.md) nutzen, um die Telefonnummer, die ein Web-Besucher in verschiedenen Bereichen der Website sieht, dynamisch zu ändern. Wow! Kannst du das glauben? Wie funktioniert diese Magie? Zunächst müssen Sie RTP wie [ beschrieben auf Ihrer Website ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript). Folgen Sie anschließend den unten stehenden Anweisungen und implementieren Sie den JavaScript-Code auf Ihrer Website:

1. Geben Sie Ihre internationale Telefonnummer in die Konfiguration **defaultPhone** ein
1. Fügen Sie die HTML-Element-ID(s) in die Konfiguration **divIds** ein
1. Wenn Sie die Telefonnummer zu einem Klick-zum-Anruf-Link für mobile Browser machen möchten, setzen Sie die Konfiguration **mobileLink** auf **true**.
1. Ordnen Sie die verschiedenen Standorte ihren Telefonnummern in den Konfigurationen **cityPhone**, **statePhone** und **countryPhone** zu

Hier finden Sie Beispielwerte für Konfigurationseinstellungen:

```json
  defaultPhone:"+1.503.608.4679", // Optional
  divIds:["phoneId1","phoneId2"],
  mobileLink: true,
  cityPhone: {
    "<a href="#">yanir</a>": ["San Mateo", "San Francisco"],
    "+353.1.242.3000": ["tel-aviv"]
  },
  statePhone: {
    "+1.650.376.2300": ["CA"],
    "+1.650.376.2302": ["OR"]
  },
  countryPhone: {
    "+1.650.376.2300": ["United States"],
    "+353.1.242.3000": ["Israel"]
  }
```

Fügen Sie abschließend ein HTML-Anker-Tag ein, das eine ID enthält, die einer der IDs in **divIds** entspricht (aus Schritt 2 oben). Wenn Sie beispielsweise „phoneId1“ in &quot;**divIds** angegeben haben, würde Ihr HTML-Anker-Tag wie folgt aussehen:

`<a href="tel:+1800229933" id="phoneId1">+1800229933</a>`

Das Skript prüft, ob eine Übereinstimmung in dieser Reihenfolge vorliegt: cityPhone > statePhone > countryPhone > defaultPhone Sie können die Telefonnummern auch durch Text ersetzen (Beispiel: „Mitglied in unserer San Francisco User Group!„) oder HTML-Code erstellen und den Inhalt je nach Standort dynamisch ändern. Viel Spaß!

```html
<a href="tel:+1800229933" id="phoneId1">+1800229933</a>
<script>
(function(a){
    rtp('get','visitor',function(yc){
        var location = yc.results.location;
        var loop = true;
        var phoneChanged = false;
        console.log(yc.results);
        function checkObj(obj){
            return Object.getOwnPropertyNames(obj).length >0;
        }
        function changePhone(p){
            d=a.divIds;
            for(i=0;i<d.length;i++){
                if(document.getElementById(d[i]) !== null){
                    document.getElementById(d[i]).innerHTML = p;
                    if(a.mobileLink){
                        document.getElementById(d[i]).href= "tel:" + p;
                    }
                    console.log(p);
                }
            }
            loop = false;
            phoneChanged = true;
        }
        function matchLocation(loc,objc){
            for (var key in objc) {
                for(i=0;i<objc[key].length && loop;i++){
                    if (!loop) { return true;};
                    val = objc[key][i];
                    //console.log(loc + location[loc] + " ? " + val);
                    if(location[loc].toLowerCase() === val.toLowerCase()){
                        changePhone(key);
                    }
                }
            }
        }
        if(checkObj(a.cityPhone)){
            matchLocation("city",a.cityPhone);
        }else if(checkObj(a.statePhone)){
            matchLocation("state",a.statePhone);
        }else if(checkObj(a.countryPhone)){
            matchLocation("country", a.countryPhone);
        }else if(!phoneChanged && a.defaultPhone.length > 0 ){
            changePhone(a.divId,a.defaultPhone);
        }
    });
})({
    defaultPhone:"",  //  [Optional] the number to show if visitor does not match the mapping below
    divIds:["phoneId1","Floater"],  //the phone HTML element ID, can be <div>, <a>, <span>, <p> etc.
    mobileLink: true,  //if you use click to call link (with href="tel:") you can also change its number

    cityPhone: {
        "<a href='#'>yanir</a>": ["San Mateo", "San Francisco"],
        "+353.1.242.3000": ["tel-aviv"]
    },
    statePhone: {
        "+1.650.376.2300": ["CA"],
        "+1.650.376.2302": ["OR"]
    },
    countryPhone: {
        "+1.650.376.2300": ["United States"],
        "+353.1.242.3000": ["Israel"]
    }
});
</script>
```

Veröffentlicht am _2016-02-02_ von _Yanir_

## Updates Winter 2016

### Benutzerdefinierte Objekte

* [Benutzerdefinierte Objekte:N Beziehungen werden jetzt unterstützt](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)
   * Lead- oder Konto-Datensätze können jetzt über benutzerdefinierte Objekte über die Definition von Zwischenobjekten Viele-zu-Viele-Beziehungen aufweisen. Nach dem Erstellen eines eigenständigen benutzerdefinierten Objekttyps können ein Zwischenobjekttyp mit Verknüpfungsfeldern sowohl zum eigenständigen Objekt als auch zu Leads oder Konten erstellt werden.
   * Es gibt keine neuen API-Aufrufe für diese Funktion, aber die Objektdefinitionen müssen korrekt konfiguriert sein, um diese Beziehungen über die API nutzen zu können.
* `getLeadActivities` und `getLeadChanges` geben keine Aktivitäten von anonymen Leads mehr zurück. Weitere Informationen finden Sie in den [Häufig gestellte Fragen zum Munchkin](https://experienceleague.adobe.com/de/docs/marketo/using/home)Tracking der nächsten Generation)

Veröffentlicht am _2016-02-05_ von _Kenny_

## Abrufen von Aktivitäten für einen einzelnen Lead mithilfe der REST-API

Hier ist eine Frage, die uns unsere Entwickler-Community immer wieder stellt:

„Wie erhalte ich eine Liste vergangener Aktivitäten für einen einzelnen Lead?“

Bis vor kurzem gab es keine einfache Möglichkeit, dies mithilfe der REST-API zu erreichen. Aber jetzt gibt es sie! Die Winter-Version 2016 unserer REST-API enthält eine nette kleine Verbesserung. [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) akzeptiert jetzt den Parameter **leadIds**, mit dem eine Lead-ID angegeben werden kann. Wenn der Parameter **leadIds** angegeben wird, werden nur Aktivitäten für diese Lead-ID zurückgegeben. Sie können sich dies als Lead-ID-Filter vorstellen. Der Parameter **leadIds** kann eine kommagetrennte Liste von Lead-IDs annehmen, wenn Sie die Ergebnisse nach mehr als einem Lead (bis zu 30) filtern möchten. Dies kann beispielsweise nützlich sein, wenn Aktivitäten auf Leads für ein bestimmtes Unternehmen beschränkt werden. **Beispiel** Nachfolgend finden Sie eine Beispielanfrage zum Abrufen [ Lead-Aktivitäten](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) die den Parameter **leadIds** enthält. Ich habe für den Parameter „leadIds **den Wert „50** angegeben, was einem beliebigen Lead in meiner Marketo-Instanz entspricht. Für den Parameter activityTypeIds habe ich den Wert „129“ angegeben, der der Aktivität „Mobile-App-Sitzung“ in meiner Marketo-Instanz entspricht.

`<https://123-abc-456.mktorest.com/rest/v1/activities.json?leadIds=50&activityTypeIds=129&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3J4SMAZRQO4RKIXCEMLFCM2APRSQ====>`

Nachstehend finden Sie einen Auszug aus der Antwort dieser Anfrage oben. Wie zu sehen ist, enthält es nur Ergebnisobjekte mit „leadId“: 50 und „activityTypeId“: 129.

```json
{
    "id": 846,
    "leadId": 50,
    "activityDate": "2015-04-06T21:58:59Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 7
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 879,
    "leadId": 50,
    "activityDate": "2015-04-07T00:45:11Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 5
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1114,
    "leadId": 50,
    "activityDate": "2015-04-08T00:02:41Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 241
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1551,
    "leadId": 50,
    "activityDate": "2015-04-09T23:31:56Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1716,
    "leadId": 50,
    "activityDate": "2015-04-15T22:44:19Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
```

## Nahtlose Integration mit Marketo und über 500 Apps mit Zapier

Dies ist ein Beitrag von Philippe Delle Case, Principal Solutions Consultant, Marketo.

### Ziele

In diesem Artikel wird detailliert erläutert, wie Sie Marketo mithilfe von Zapier mit potenziell über 500 Cloud-Apps integrieren können. Dazu werden wir einen Zapier-Connector für Marketo von Grund auf neu erstellen und zwei praktische Integrationsanwendungsfälle implementieren: Anwendungsfall 1: Unidirektionale Lead-Integration von FullContact Card Reader zu Marketo

* Scannen Sie die Visitenkarte eines Kontakts mit der FullContact Mobile Card Reader-App und erstellen Sie automatisch einen Lead in Marketo.
* Fügen Sie einen bestehenden Lead zu einer statischen Liste in Marketo hinzu und suchen Sie den Lead, der automatisch zu Ihrem Google-Blatt hinzugefügt wird.
* Ändern Sie einen beliebigen Lead in Ihrem Google-Blatt und finden Sie die Änderung wieder in Marketo.

### Voraussetzungen

**Registrieren Sie sich für ein kostenloses Konto bei Zapier** [Zapier](https://zapier.com/) ist ein Service zur Automatisierung von Web-Apps, mit dem Sie mühelos Aufgaben zwischen anderen Online-Apps automatisieren können, ohne dass Programmierer oder IT-Ressourcen erforderlich sind. Eine kurze Einführung finden Sie [hier](https://zapier.com/api/v4/helpdocs/category/redirect/what-is-zapier). Heute unterstützt Zapier mehr als 500 Apps in vielen verschiedenen Bereichen wie Marketing, CRM, CMS, Kundensupport, elektronische Signatur, Forms usw. Eine einzelne Integration zwischen einer App und einer anderen wird als Zap bezeichnet. In Zapier&#39;s [zapbook](https://zapier.com/apps) finden Sie eine umfassende Liste der unterstützten Web-Apps.  Registrieren Sie sich für ein kostenloses Konto [hier](https://zapier.com/sign-up/). Sie erhalten Zugriff auf bis zu 100 Aufgaben pro Monat, 5 Zips und Zaps, die alle 15 Minuten ausgeführt werden. Sie können natürlich viel mehr bekommen, indem Sie Zapier&#39;s bezahlte Pläne abonnieren (Basic, Business, Business Plus, etc.)

**Zugriff auf eine Marketo-Instanz als Administrator oder mit einem bereitgestellten API-Benutzerkonto** Unser Zapier-Connector verwendet die Marketo-REST-API, um Lead-Daten an Marketo zu senden. Um diese API verwenden zu können, benötigen Sie einen API-Benutzer und einen benutzerdefinierten Service, den Sie selbst erstellen können, wenn Sie Administrator Ihrer Marketo-Instanz sind. Ist dies nicht der Fall, muss ein Administrator Ihnen diese bereitstellen. Es gibt auch einen zu erstellenden Webhook, der nur für Marketo-Admins zugänglich ist. Eine schrittweise Erklärung zum Erstellen des Marketo-API-Benutzers und des benutzerdefinierten Service finden Sie [hier](http://rest-api/custom-services/). Sobald Sie fertig sind, sollten Sie über die folgenden Anmeldeinformationen verfügen, um die Marketo-REST-API aufzurufen: Client-ID, Client-Geheimnis, Munchkin-Konto-ID, Munchkin-Konto-ID

Sie können die Munchkin-Konto-ID über die Munchkin- oder Web-Services-Admin-Bildschirme abrufen. Das Muster sieht so aus: `000-XXX-000`.  Es muss kein Zugriffs-Token abgerufen werden, da es nur eine einzige Stunde lang gültig wäre. Der Connector generiert automatisch Token für Sie.
**Mit der kostenlosen Anmeldung bei Google Docs, Sheets und Slides können Sie verschiedene Arten von Online-Dokumenten erstellen, mit anderen Personen in Echtzeit bearbeiten und online in Ihrem Google Drive speichern. Unser Anwendungsfall erfordert ein Google-Blatt. Verschiedene Funktionen von Google Docs und die Erstellung eines Kontos bei Google finden Sie [hier](https://workspace.google.com/products/docs/).
**Registrieren Sie sich für ein kostenloses Konto bei FullContact** FullContact sorgt dafür, dass Sie mit den wichtigsten Personen in Verbindung bleiben, indem Sie alle Ihre Kontakte abrufen und sie kontinuierlich mit Änderungen an Social-Media-Profilen, Fotos, E-Mail-Signaturen, Unternehmensinformationen und mehr synchronisieren. Sie bieten einen mobilen Visitenkartenleser, der Karten in über 250 Web-Apps scannen kann, darunter Zapier. Sie können sich für ein kostenloses Konto [hier](https://app.fullcontact.com/login). Sie können auch ein kostenpflichtiges Premium-Konto mit mehr Funktionen und Kapazität abonnieren. Die Mobile App kann vom [Apple AppStore](https://apps.apple.com/us/app/fullcontact-business-card/id576780807) oder von [Google Play heruntergeladen ](https://play.google.com/store/apps/details?id=com.fullcontact.cardreader). Die FullContact Zaps sind [hier](https://zapier.com/apps/contacts-plus/integrations) dokumentiert.

### Implementierung des Marketo Connectors für Zapier

**Erstellen der Marketo-App** Wechseln Sie über die Zapier-Web-Oberfläche zum Entwicklerportal. Klicken Sie **Neue App hinzufügen** und füllen Sie mindestens den Titel (z. B. &quot;Marketo„) und die Beschreibung aus. Das Logo ist optional, aber nett zu haben.\ **Authentifizierung** In diesem Abschnitt deklarieren wir die verschiedenen Felder, die für die Marketo REST-API-Authentifizierung und die Authentifizierungseinstellungen verwendet werden. Erstellen Sie zunächst die folgenden Felder:

Bearbeiten Sie die „Authentifizierungseinstellungen“:

* Authentifizierungstyp: Sitzungsauthentifizierung
* Auth-Zuordnung:

  `{"access_token":"{{access_token}}"}`

* Zugriffstoken-Platzierung&#x200B;**:** Token in Abfragezeichenfolge

Nachdem ein benutzerdefinierter Marketo-Service erstellt wurde, werden die Client-ID und das Client-Geheimnis verfügbar. Wir verwenden die Client-ID und den geheimen Client-Schlüssel zum Generieren eines Zugriffstokens über den REST-API-Endpunkt [Authentifizierung](/help/rest-api/authentication.md). Wir können dieses Zugriffstoken dann verwenden, um nachfolgende Anfragen an die REST-API zu stellen. Das Token läuft nach einer Stunde ab und muss erneut generiert werden, um mit dem Aufruf der REST-API fortzufahren. Wir haben Authentifizierungstyp = „Sitzungsauthentifizierung“ gewählt, da wir damit jedes Mal, wenn unser Sitzungs-Token abgelaufen ist, ein benutzerdefiniertes Authentifizierungsskript ausführen können. Im Abschnitt „Skripterstellungs-API“ erfahren Sie, wie Sie diesen Mechanismus implementieren, der nur mit dieser Art von Authentifizierung funktioniert.
**Trigger** Zapier-Trigger sind da, um Daten nach Zapier zu bringen. Wir benötigen keinen für unsere Anwendungsfälle, da wir stattdessen einen Marketo-Webhook nutzen werden. Wir müssen jedoch weiterhin einen Platzhalter-Trigger als obligatorischen Test für unseren Marketo-Connector schreiben. Wir erstellen einen Test-Trigger, der den Endpunkt Marketo REST-API ([ tägliche Nutzung abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyUsageUsingGET) aufruft. Klicken Sie **Neuen Trigger hinzufügen**, um den Assistenten zu starten und die folgenden Felder auszufüllen (die nicht erwähnten Felder können leer gelassen werden): Name und Beschreibung

* Name: Test-Trigger
* Schlüssel: test_Trigger
* Beschreibung: Der Test-Trigger der Marketo-App
* Wichtig? Nicht aktiviert
* Ausblenden? Aktiviert

Trigger Fields

* Keine

Woher die Daten kommen

* Data Source: Abfrage
* Abruf-URL: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/stats/usage.json`

Beispielergebnis

* Leer lassen

Klicken Sie **Benutzereinstellungen verwalten** und legen Sie unseren Test-Trigger auf den Trigger fest, den wir zum Überprüfen der Benutzeranmeldeinformationen verwenden. **Aktionen** Zapier-Aktionen senden Daten von Zapier aus. Wir implementieren die Lead-Aktion „Create_Update“ und rufen die Marketo-REST-API auf. Mit dieser Aktion können wir einen neuen Lead in Marketo erstellen. Wenn der Lead bereits vorhanden ist, wird er mit den gesendeten Werten aktualisiert. Zur Deduplizierung verwenden wir das Feld „E-Mail“. Klicken Sie **Neue Aktion hinzufügen**, um den Assistenten zu starten und die folgenden Felder auszufüllen (die nicht genannten Felder können leer gelassen werden): Name und Beschreibung

* Name: create_update Lead
* Substantiv: Lead
* Schlüssel: create-update-lead
* Beschreibung: Erstellen Sie einen neuen Lead in Marketo oder aktualisieren Sie ihn mit den gesendeten Werten, wenn der Lead bereits vorhanden ist
* Wichtig? Aktiviert
* Ausblenden? Nicht aktiviert

Aktionsfelder Aktionsfelder sind die Felder, denen Benutzer Daten zuordnen. Wählen Sie sie sorgfältig entsprechend Ihren eigenen Anforderungen aus, da sie alle Daten darstellen, die Sie in Marketo aktualisieren können. In Zapier gibt es eine Option, dem Endbenutzer alle in Marketo verfügbaren Felder anzubieten. Dies würde jedoch zu mehr Code und Komplexität führen, die für einen Einweg-Connector nicht erforderlich sind. Als Beispiel haben wir die folgenden Felder ausgewählt.

Der Partitionsname ist in unserem Fall obligatorisch, da unsere Marketo-Instanz Lead-Partitionen im Dienst hat. Andernfalls könnte er weggelassen werden. Wir haben sie von der Gruppe „Eingabe“ getrennt, sodass der Endbenutzer versteht, dass es sich nicht um ein zu synchronisierendes Feld handelt. Das Feld „Anmerkungen“ stammt aus einer Synchronisation zwischen Marketo und Salesforce. Verwenden Sie es nicht, wenn Sie es nicht in Ihrer Marketo-Instanz haben. Das Feld „Aufgerufen“ wurde in unserer Marketo-Instanz erstellt. Verwenden Sie es nicht, wenn es nicht in Ihrer Marketo-Instanz vorhanden ist. Natürlich besteht das Ziel darin, Sie die Felder auswählen zu lassen, die Sie von Marketo benötigen. Es wird empfohlen, klein zu beginnen und die zusätzlichen Felder später hinzuzufügen. Senden von Daten

* Aktionsendpunkt-URL: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/leads.json`

Beispielergebnis

* Leer lassen

### Zapier Scripting API

Mit der Skripterstellungsfunktion von Zapier können Sie die Anfragen und Antworten bearbeiten, die zwischen der API Ihrer App und Zapier ausgetauscht werden. Sie können HTTP-Anfragen ändern, bevor sie gesendet werden, und Antworten analysieren, bevor Zapier etwas mit ihnen macht. Wir benötigen sie, um unsere benutzerdefinierte „Sitzungsauthentifizierung“ abzuschließen, damit sie mit Marketo funktioniert. Weitere Informationen finden Sie [hier](https://zapier.com/developer/documentation/scripting/). Kopieren Sie den folgenden Code, und wir werden später einige Erklärungen durchgehen:

```javascript
var Zap = {

    get_session_info: function(bundle) {

       console.log('Entering get_session_info method ...');

         var access_token,
            access_token_request_payload,
            access_token_response;


        // Assemble the meta data for our Access Token swap request
         console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
        access_token_request_payload = {
            method: 'POST',
            url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
            params: {
                'grant_type' : 'client_credentials',
                'client_id' : bundle.auth_fields.client_id,
                'client_secret' : bundle.auth_fields.client_secret
            },
            headers: {
                'Content-Type': 'application/json',  // Could be anything.
                Accept: 'application/json'
            }
        };

        // Fire off the Access Token request.
        access_token_response = z.request(access_token_request_payload);

        // Extract the Access Token from returned JSON.
        access_token = JSON.parse(access_token_response.content).access_token;
        console.log('New Access_Token=' + access_token);

        // This will be mixed into bundle.auth_fields in future calls.
        //bundle.auth_fields.access_token=access_token;
        return {'access_token': access_token};
    },


    test_trigger_pre_poll: function(bundle) {

         console.log('Entering test_trigger_pre_poll method ...');

         bundle.request.params = {
         'access_token':bundle.auth_fields.access_token
         };

         return bundle.request;

    },


    test_trigger_post_poll: function(bundle) {

        console.log('Entering test_trigger_post_poll method ...');

        var data = JSON.parse(bundle.response.content);
        if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

           throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }

        return JSON.parse(bundle.response.content);
    },

    create_update_lead_pre_write: function(bundle) {

       bundle.request.params = {'access_token':bundle.auth_fields.access_token};
       return bundle.request;
    },

    create_update_lead_post_write: function(bundle) {

         var data = JSON.parse(bundle.response.content);
         if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }
        return JSON.parse(bundle.response.content);
    }
};
```

**Methoden** **get_session_info**

* Diese Methode ist für das Generieren oder erneute Generieren eines Zugriffstokens verantwortlich, das den Marketo-REST-API-Endpunkt [Authentifizierung) ](/help/rest-api/authentication.md).
* Sie wird jedes Mal aufgerufen, wenn bei einer „post_poll“-Methode ein „Zugriffstoken abgelaufen“-Fehler auftritt. Ein Zugriffs-Token läuft planmäßig alle 1 Stunde ab. Dies ist also zu erwarten.
* Aktionsendpunkt-URL: https://{{munchkin_account_id}}.mktorest.com/identity/oauth/token

**pre_poll, pre_write**

* Wir müssen für jeden von uns erstellten Trigger eine „Pre-poll“-Methode erstellen, um die HTTP-Anfrage direkt vor dem Senden zu ändern, damit wir das Marketo-Zugriffs-Token in den Parametern hinzufügen können.
* Aus demselben Grund müssen wir für jede von uns erstellte Aktion eine „Pre-write“-Methode erstellen.

**post_poll, post_write**

* Wir müssen für jeden von uns erstellten Trigger eine „Post-Poll“-Methode erstellen, um die Antworten zu analysieren, bevor Zapier etwas mit ihnen macht, und schließlich den Fehler „Access Token Expired“ abzufangen.
* Aus demselben Grund müssen wir für jede von uns erstellte Aktion eine Post-Write-Methode erstellen.
* Wenn ein solcher Fehler aufgetreten ist, lösen wir eine InvalidSessionException aus, die Zapier anweist, die Authentifizierung erneut zu wiederholen und die get_session_info-Methode erneut auszuführen.

Beachten Sie, dass Sie über die Skripterstellungs-API über das Menü „Schnelllinks“ oben rechts auf dem Bildschirm auf die Bundle-Protokolle zugreifen können. Dies ist wirklich nützlich, um die Skripte zu debuggen.

Anwendungsfall 1: Integration von Marketo mit FullContact Card Reader

Für diese Integration erstellen wir eine einzige Zap von FullContact zu Marketo. Mit diesem Zap können Sie Visitenkarten mit der FullContact Mobile Card Reader scannen und die Leads an Marketo pushen.   **Zap FullContact -> Marketo** Klicken Sie im Zapier-Dashboard auf die Schaltfläche „Neuen Zap erstellen“.

**Trigger in Zapier**

* Auswahl der App FullContact
* FullContact-Trigger auswählen &#39;Neue Visitenkarte&#39;
* Verbinden mit Ihrem FullContact-Konto
* Testen der FullContact-App

**Aktion in Zapier**

* Wählen Sie die soeben erstellte App Marketo aus. Sie sollte in Beta angezeigt werden.
* Marketo-Aktion &#39;Create_Update Lead&#39; auswählen
* Stellen Sie eine Verbindung zu Ihrem Marketo-Konto her und füllen Sie die Authentifizierungsparameter (Munchkin-Konto-ID, Client-ID, Client-Geheimnis) aus
* Zuordnen der Felder von FullContact zu Marketo
* Füllen Sie schließlich einen Partitionsnamen aus, in den Ihre neuen Leads gehen sollen (nur wenn Partitionen in Ihrer Marketo-Instanz vorhanden sind)
* Testen der Marketo-App
* ZAP aktivieren

Hinweis: Stellen Sie sicher, dass Sie die Visitenkarten Reader von FullContact herunterladen und die Zapier Integration direkt von Ihrem Mobilgerät aus aktivieren.

Anwendungsfall 2: Integration von Marketo mit Google Sheets -

Für diese Integration erstellen wir zwei Zaps. Eine von Marketo zu Google Sheets und eine andere von Google Sheets zu Marketo. Mit dieser Zap-Funktion können Sie einige Ihrer Leads oder Kontakte zwischen Marketo und einer Google-Tabelle synchronisieren.   **ZAP Marketo Webhook -> Google Sheets**
Für den ersten Zap verwenden wir keinen benutzerdefinierten Connector für Marketo, sondern nutzen Marketos Webhooks und den Trigger „Webhooks by Zapier“. Klicken Sie im Zapier-Dashboard auf die Schaltfläche „Neuen Zap erstellen“. **Trigger Teil 1 in Zapier**

* Trigger-App „Webhooks von Zapier“ auswählen
* Markieren Sie „Catch Hook“, damit auf einen POST gewartet werden kann, oder GET auf eine Zapier-URL
* Kein Grund, einen untergeordneten Schlüssel auszuwählen
* Zapier hat eine benutzerdefinierte Webhook **URL generiert** an die Sie Anfragen senden und in die Zwischenablage kopieren können

**Webhook in Marketo (Schritte, die von einem Administrator auszuführen sind)**

* Gehen Sie zu Admin > Webhooks
* Erstellen Sie einen neuen Webhook mit dem Namen „Lead an Zapier pushen“ und bearbeiten Sie das Webhook-Formular.

Deklarieren Sie im Feld der Vorlage alle Felder des Leads, die Sie an Zapier übertragen möchten, und nutzen Sie die Token der Marketo. Für unsere Anwendungsfälle verwenden wir dieselben Felder, die wir für den benutzerdefinierten Zapier-Connector definiert haben, der Leads an Marketo sendet:

```json
{
"firstName":"{{lead.First Name}}",
"lastName":"{{lead.Last Name}}",
"email":"{{lead.Email Address}}",
"phone":"{{lead.Phone Number}}",
"leadOwner":"{{lead.Lead Owner First Name}} {{lead.Lead Owner Last Name}}",
"leadOwnerEmail":"{{lead.Lead Owner Email Address}}",
"leadNotes":"{{lead.Lead Notes:default=edit me}}",
"called":"{{lead.Called}}"
}
```

* Formular speichern
* Es ist kein Antwort-Mapping erforderlich, sodass Sie mit dem Webhook fertig sind

**Testen einer Kampagne in Marketo (Schritte, die von einem Marketer oder Administrator durchgeführt werden müssen)**

* Erstellen Sie aus den Marketing-Aktivitäten eine neue intelligente Kampagne

Zu Testzwecken erstellen wir eine Kampagne, die unseren Webhook jedes Mal Trigger, wenn sich der Lead-Status in MQL ändert. Natürlich können Sie den Webhook auch für andere Geschäftszwecke verwenden.

* Bearbeiten der Smart-Liste
* Aufrufen des Webhooks im Fluss
* Planen der Kampagne
* Stellen Sie sicher, dass jeder Lead jedes Mal durch den Fluss laufen kann
* Aktivieren der Smart-Kampagne

**Trigger Teil 2 in Zapier**

* Um die Trigger-App „Webhooks von Zapier“ abzuschließen, müssen wir die Marketo Smart Campaign einmal auslösen und den Webhook in Zapier abfangen
* In unserem Testfall müssen wir nur zur Marketo Lead-Datenbank gehen, einen Lead öffnen und seinen Status in „MQL“ ändern

**Erstellen des Arbeitsblatts in Google Sheets**

* Neue Tabelle erstellen
* Erstellen eines Arbeitsblatts oder Verwenden des Standardarbeitsblatts
* Fügen Sie für jedes Feld, das Sie aus Marketo synchronisieren möchten, eine Spalte hinzu (die im Marketo-Webhook deklarierten)

  **Aktion in Zapier**

* App-Google-Arbeitsblätter auswählen
* Aktivieren Sie die Option „Tabellenzeile erstellen“.
* Verbinden mit Ihrem Google Sheets-Konto
* Google Sheets-Tabelle auswählen
* Arbeitsblatt auswählen
* Ordnen Sie alle Felder zwischen der Trigger-App „Webhooks by Zapier“ und den Google Sheets zu.

* Testen der Google Sheets-App
* ZAP aktivieren

**Zap Google Sheets -> Marketo**

Klicken Sie im Zapier-Dashboard auf die Schaltfläche „Neuen Zap erstellen“.

**Trigger in Zapier**

* Trigger-App &quot;Google Sheets“ auswählen
* Aktivieren Sie das Kontrollkästchen &#39;Aktualisierte Tabellenzeile&#39;, das Trigger verursacht, wenn eine neue Zeile in einer Tabelle hinzugefügt oder geändert wird
* Herstellen einer Verbindung zu Ihrem Google-Konto
* Wählen Sie die Tabelle, aus der Sie einen Trigger erstellen möchten (es sollte die gleiche sein, die in der vorherigen ZIP-Datei verwendet wurde), und das Arbeitsblatt aus
* Trigger-Spalte auf &#39;any_column&#39; setzen
* Testen der Google Sheets-App

**Aktion in Zapier**

* Wählen Sie die soeben erstellte App Marketo aus. Sie sollte in Beta angezeigt werden.
* Marketo-Aktion &#39;Create_Update Lead&#39; auswählen
* Stellen Sie eine Verbindung zu Ihrem Marketo-Konto her und füllen Sie die Authentifizierungsparameter (Munchkin-Konto-ID, Client-ID, Client-Geheimnis) aus
* Zuordnen der Felder aus Google Sheets zu Marketo
* Füllen Sie schließlich einen Partitionsnamen aus, in den Ihre neuen Leads gehen sollen (nur wenn Partitionen in Ihrer Marketo-Instanz vorhanden sind)
* Testen der Marketo-App
* ZAP aktivieren

### Zusammenfassung

Hier sind einige Verbesserungsvorschläge für unseren Marketo-Connector für Zapier:

* Hinzufügen anderer Trigger und Aktionen im Zusammenhang mit verschiedenen Marketo-Objekten (Listen, benutzerdefinierte Objekte usw.)
* Anstatt die Felder aus Marketo fest zu codieren, ist es möglich, die Felder aus Marketo dynamisch abzurufen, aber das würde einige technische Übersetzungsarbeiten zwischen Marketo und Zapier erfordern.
* Freigabe des Connectors für das Entwicklungs-Team und schließlich allgemeine Verfügbarkeit.

Es ist möglich, dass Zapier einen Premium-Marketo-Adapter bereitstellt, was die Implementierung unserer Anwendungsfälle erheblich erleichtern würde. In jedem Fall kann dieser Artikel immer genutzt werden, um Marketo mit Zapier mit einem kostenlosen Zapier-Plan zu integrieren und auch um benutzerdefinierte Anwendungsfälle zu erstellen, die möglicherweise nicht von einem Premium-Adapter unterstützt werden. Wir hoffen, dass Ihnen dieser Artikel gefallen hat und dass er Ihnen helfen wird, mit Marketo und Zapier noch erfolgreicher zu sein. Vielen Dank!

Veröffentlicht am _2016-04-17_ von _David_

## Updates Frühjahr 2016

**REST-API**

* Asset-API - Web-Seiten
   * **Landingpages** werden jetzt über fünfzehn neue Endpunkte verfügbar gemacht, was das Erstellen, Aktualisieren, Löschen, Klonen und Entwurfsmanagement für Landingpages ermöglicht. Für Landingpage-Vorlagen werden jetzt auch Entwurfs-Management-Endpunkte bereitgestellt
      * Landingpages abrufen
      * Landingpage nach ID abrufen
      * Landingpage nach Namen abrufen
      * Landing Page erstellen
      * Aktualisieren von Landingpage-Metadaten
      * Abrufen von Landingpage-Inhalten
      * Abschnitt zum Hinzufügen von Landingpage-Inhalten
      * Abschnitt zum Aktualisieren des Landingpage-Inhalts
      * Abschnitt zum Löschen des Landingpage-Inhalts
      * Abschnitt „Dynamischen Inhalt abrufen“
      * Abschnitt „Dynamischen Inhalt aktualisieren“
      * Entwurf der Landingpage verwerfen
      * Landing Page genehmigen
      * Genehmigung des Landingpage-Entwurfs aufheben
      * Landingpage löschen
   * **Landingpage-Vorlagen**
      * Entwurf der Landingpage-Vorlage verwerfen
      * Landing Page-Vorlage genehmigen
      * Genehmigung der Landingpage-Vorlage aufheben
      * Landing Page-Vorlage löschen
   * **Forms** hat 21 neue Endpunkte veröffentlicht, die vollständige Erstellungs-, Bearbeitungs- und Verwaltungsfunktionen über die API bieten. Die APIs unterstützen keine Änderungen an Forms 1.0-Formularen.
      * Forms abrufen
      * Formular nach ID abrufen
      * Formular nach Namen abrufen
      * Formularfeldliste abrufen
      * Formularfeldliste aktualisieren
      * Formular erstellen
      * Abruf der Dankeseite
      * Aktualisierungsformular - Dankesseite
      * Formular aktualisieren
      * Formularentwurf verwerfen
      * Formular genehmigen
      * Genehmigung für Formular aufheben
      * Formular klonen
      * Formular löschen
      * Formularfeld aktualisieren
      * Formularfeld entfernen
      * Aktualisieren der Sichtbarkeitsregeln für Formularfelder
      * Rich-Text-Formularfeld hinzufügen
      * Feldsatz hinzufügen
      * Feld aus Feldgruppe entfernen
      * Verfügbare Formularfelder abrufen
      * Ändern der Formularfeldpositionen
      * Schaltfläche „Senden aktualisieren“
   * Bei Verwendung von **Programme abrufen oder durchsuchen** wird die SFDC-Kampagnen-ID für Programme zurückgegeben, die mit einer SFDC-Kampagne verknüpft sind

**Benutzerdefinierte Objekte** Benutzerdefinierte Objekte unterstützen jetzt Textbereich-Datentypen, sodass in benutzerdefinierten Objektfeldern dieses Typs Zeichenfolgenfelder mit bis zu 2.000 Zeichen gespeichert werden können. **IP-Adressen-Whitelisting** Admin-Benutzer können jetzt eine Whitelist von IP-Adressen verwalten, um den unbefugten Zugriff über die APIs zu verhindern. [Weitere Informationen zu dieser Funktion finden Sie hier](https://experienceleague.adobe.com/de/docs/marketo/using/home). **Benutzeroberfläche für benutzerdefinierte Aktivitäten** Admin-Benutzer können jetzt benutzerdefinierte Aktivitätstypen in ihrem Admin-Menü definieren und über die API [Benutzerdefinierte Aktivitäten hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST) Datensätze zu Leads hinzufügen. [ Informationen zur Definition benutzerdefinierter Aktivitätstypen finden Sie hier](https://experienceleague.adobe.com/de/docs/marketo/using/home).

Veröffentlicht am _2016-06-01_ von _Kenny_

## Updates Sommer 2016

Für die Version vom Sommer 2016 am 23. September werden drei entwicklerorientierte Funktionen veröffentlicht.

### E-Mail 2.0-Unterstützung in der REST-API

Alle [bereits vorhandenen Asset-APIs](/help/rest-api/assets.md) die nur mit E-Mails und Vorlagen der Version 1.0 kompatibel waren, sind jetzt für die Verwendung mit E-Mail-Assets der Version 2.0 aktiviert.

### Lead zu Marketo pushen

[Lead per Push übertragen](/help/rest-api/leads.md) ist eine alternative Methode zur Lead-Synchronisation, die zur einfacheren Auslösung in Smart-Kampagnen entwickelt wurde. Sie können ein einzelnes Aktivitätsprotokollelement erstellen, einen Lead verknüpfen und den Lead-Datensatz in einem Aufruf aktualisieren. Dies funktioniert ähnlich wie ein einzelnes Formular, das von einem Lead ausgefüllt wird, und kann einfacher als Proxy-Methode für die Formularübermittlung anstelle der vorhandenen Methode Lead synchronisieren verwendet werden.

### HTTP-Komprimierung

Die REST-API kann jetzt Antworten mit dem durch die HTTP 1.1-Spezifikation definierten Standard komprimieren. Dadurch kann die Größe der Antwort reduziert werden, was die Übertragungsgeschwindigkeit erhöht und die Bandbreitenauslastung minimiert.  

Veröffentlicht am _2016-09-23_ von _Kenny_

## Verwenden von swagger-codegen mit Marketo

[Swagger-codegen](https://github.com/swagger-api/swagger-codegen) ist eine leistungsstarke Java-Bibliothek, die sowohl Server-Stubs als auch API-Clients aus Swagger-Definitionen generieren kann. Dies kann die Schwierigkeit und die Kosten für die Generierung von Clients für eine bestimmte Sprache erheblich verringern. Um zu beginnen und Ihren ersten Client zu generieren, sollten Sie sich zunächst eine instanzspezifische Kopie einer der Swagger-Definitionen von Marketo sichern. Geben Sie die Munchkin-ID aus der Instanz ein, mit der Sie testen möchten. Beginnen Sie mit der Identitätsdefinition. Nachdem Sie nun über eine für Ihre Instanz spezifische Definition verfügen, müssen Sie swagger-codegen herunterladen und installieren. Der Prozess ist spezifisch für Ihr Betriebssystem und Sie können die Anweisungen [hier](https://github.com/swagger-api/swagger-codegen#prerequisites) mit den Standardeinstellungen erhalten. Codegen gibt einen Client aus, der alle bereitgestellten Endpunkte und Modelle abdeckt. Diese werden in der Regel über eine Klasse mit der Bezeichnung DefaultAPI verwaltet, die Methoden zum Aufrufen der verfügbaren Endpunkte mit Beispielen in einem „docs“-Ordner enthält (nicht alle Sprachen enthalten standardmäßig Vorlagen dafür). Jetzt bauen wir den ersten Client. Erstellen Sie einen Ordner, in dem Ihr Code gespeichert werden soll, und wechseln Sie in Ihre Terminal-Sitzung und verwenden Sie den Generate-Befehl, um einen Client in der gewünschten Sprache zu erstellen (wir gehen davon aus, dass Sie die homebrew-Installationsmethode verwendet haben):

swagger-codegen generate -i $definitionLocation -l $yourLanguage -o $yourLocation

Dadurch wird der Clientcode an den gewünschten Speicherort ausgegeben. Sehen wir uns nun an, wie wir dies verwenden, um den Identitätsendpunkt aufzurufen und ein Zugriffstoken abzurufen:

```java
 public static void main(String[] args){
  String client_id = args[0];
  String client_secret = args[1];
  ApiClient client = new ApiClient();
  DefaultApi id = new DefaultApi();
  try {
   String token = id.identityOauthTokenGet(client_id, client_secret, "client_credentials").getAccessToken();
   System.out.println(token);
  } catch (ApiException e) {
   e.printStackTrace();
  }
 }
```

```php
<?php
require_once(_DIR_ . '/vendor/autoload.php');

$api_instance = new Swagger\\Client\\Api\\DefaultApi();
$client_id = "client_id_example";
$client_secret = "client_secret_example";
$grant_type = "grant_type_example";

try {
    $result = $api_instance->identityOauthTokenGet($client_id, $client_secret, $grant_type);
    print_r($result->getAccessToken);
} catch (Exception $e) {
    echo 'Exception when calling DefaultApi->identityOauthTokenGet: ', $e->getMessage(), "\\n";
}
?>
```

```c#
  public static void Main(string[] args)
  {
      string clientId = "CHANGE ME";
      string clientSecret = "CHANGE ME";

      IdentityApi instance = new IdentityApi();
      ResponseOfIdentity response = instance.IdentityUsingGET(clientId, clientSecret, "client_credentials");

      string message = string.Format("Access Token: {0}, Expires In: {1}, Scope: {2}, Token Type: {3}",
          response.AccessToken, response.ExpiresIn, response.Scope, response.TokenType);
      Console.WriteLine(message);
  }
```

Da wir nun wissen, wie wir autorisiert werden können, werden wir uns in den kommenden Wochen mit weiterentwickelten Anwendungsfällen von automatisch generierten Clients befassen.

Veröffentlicht am _2016-10-10_ von _Kenny_

## Excel-Integration Teil 1: Extrahieren und Formen von Marketo-Daten mithilfe von Power Query

Dies ist der erste aus einer Reihe von Artikeln, in denen erläutert wird, wie Sie die in Microsoft Excel integrierte Power BI-Technologie nutzen können, um mit Marketo ein echtes Self-Service-Geschäftsanalyseerlebnis zu schaffen. Mit den in diesen Artikeln behandelten Konzepten können Sie:

* Importieren von Daten aus Marketo in Excel
* Importieren und Kombinieren von Daten aus anderen Quellen (SaaS-Anwendungen, Datenbanken, flache Dateien usw.)
* Daten für geschäftliche Anforderungen und Analysezwecke formen
* Daten aus Excel bei Bedarf aktualisieren
* Erstellen von berechneten Spalten und Kennzahlen mithilfe von Formeln
* Erstellen von Beziehungen zwischen heterogenen Daten
* Daten analysieren und erweiterte Berichte mit Pivot-Tabellen und Pivot-Diagrammen erstellen
* Hervorragende Datenvisualisierungen erstellen

### Power Query für Excel

Dieser erste Artikel behandelt den Datenimport und -formungsprozess mithilfe der Power Query-Technologie. Power Query ist eine Datenverbindungstechnologie, mit der Sie Datenquellen identifizieren, verbinden, kombinieren und verfeinern können, um Ihre Analyseanforderungen zu erfüllen. Funktionen in Power Query sind in Excel und Power BI Desktop verfügbar. Power Query kann eine Verbindung zu vielen Datenquellen wie Datenbanken, Facebook, Salesforce, MS Dynamics CRM usw. herstellen. Marketo wird nicht vorkonfiguriert unterstützt, aber zum Glück können wir Marketo-REST-APIs für die Remote-Ausführung vieler Systemfunktionen verwenden. Power Query verfügt über einen umfangreichen Satz von Formeln (informell als „M“ bezeichnet), mit denen Sie eine benutzerdefinierte Datenquelle erstellen können.

### Benutzerdefinierter Connector

Die Skripterstellung für einen einzelnen REST-API-Aufruf ist bei Power Query trivial, aber es wird schwieriger, die folgenden Anforderungen zu erfüllen:

* Verwaltung von Zugriffstoken einschließlich Authentifizierungsmechanismus und regelmäßiger Token-Aktualisierung
* Paginierungsmechanismus für große Datenmengen
* Umgang mit Fehlern

In diesem Artikel wird erläutert, wie Sie einen robusten benutzerdefinierten Connector erstellen, der die REST-APIs von Marketo verwenden kann, um alle Arten von Daten (Leads, Aktivitäten, benutzerdefinierte Objekte, Programme usw.) abzurufen. Ihre einzige Einschränkung betrifft das Limit Ihrer täglichen Marketo-API-Anfrage. Die hier erläuterten Konzepte konzentrieren sich auf Marketo, können aber auch zur Integration anderer SaaS-Lösungen verwendet werden, die eine REST-API bereitstellen.

### Voraussetzungen

#### Power Query

Vor der Veröffentlichung von Excel 2016 fungierte Microsoft Power Query for Excel als Excel-Add-in, das auf Excel 2010 oder Excel 2011 heruntergeladen und installiert wurde. Ab Excel 2016 ist diese Technologie eine native Funktion, die in das „Daten“-Band unter dem Abschnitt „Abrufen und Transformieren“ integriert ist. Alle für diesen Artikel erstellten Skripte wurden auf Excel 2016 für Windows getestet. Die Konzepte sollten für Excel 2013 oder Excel 2010 identisch sein, es könnten jedoch einige Anpassungen erforderlich sein.  Power Query ist derzeit nur in der Microsoft Windows-Version von Excel verfügbar; die Mac-Version wird leider nicht unterstützt.

#### Marketo

Power Query verwendet die Marketo REST-APIs für den Zugriff auf Daten aus Marketo. Um diese APIs verwenden zu können, benötigen Sie einen API-Benutzer und einen benutzerdefinierten Service, den Sie selbst erstellen können, wenn Sie Administrator Ihrer Marketo-Instanz sind. Ist dies nicht der Fall, muss ein Administrator Ihnen diese bereitstellen. Eine schrittweise Erklärung zum Erstellen des Marketo-API-Benutzers und des benutzerdefinierten Service finden Sie [hier](/help/rest-api/custom-services.md). Sobald Sie fertig sind, sollten Sie über die folgenden Anmeldeinformationen verfügen, um die Marketo-REST-APIs aufzurufen: **Client-ID** und **Client-Geheimnis**. Der **REST-API** Endpunkt) befindet sich im REST-API-Abschnitt des Web Services Admin in Marketo und sollte das folgende Muster aufweisen:

`<https://XXX-XXX-XXX.mktorest.com/rest>`

Marketo verfügt über ein tägliches Anforderungslimit für seine API. Dieses Limit finden Sie in der Web-Services-Verwaltung zusammen mit einem Verbrauchsbericht. **Achten Sie beim Entwerfen Ihrer Abfragen darauf, nie Ihr tägliches Limit zu überschreiten, da Sie möglicherweise einige Daten in Ihren Berichten verpassen**.

### Erstellung der Power Query-Arbeitsmappe

Beginnen wir mit einer neuen Excel-Arbeitsmappe. Wir erstellen ein spezifisches Konfigurationsarbeitsblatt zum Deklarieren aller Marketo REST API-Einstellungen. In diesem Arbeitsblatt erstellen wir drei Tabellen:

Tabelle &#39;**REST_API_Authentication**&#39; mit den Spalten: **URL**: Ihr Marketo REST-API-Endpunkt. **Client-ID**: von Ihren Marketo REST API OAuth2.0-Anmeldeinformationen. **Client-Geheimnis**: von Ihren Marketo REST API OAuth2.0-Anmeldeinformationen.
Tabelle &#39;**Scoping**&#39; mit den Spalten: **Paging-Token SinceDatetime**: ein Datum, das der ISO 8601-Standarddatumsnotation folgt (z. B. sind „2016-10-06T13:22:17-08:00“, „2016-10-06“ ein gültiges Datum/eine gültige Uhrzeit), die verwendet wird, um Marketo-Aktivitäten seit einem bestimmten Zeitraum mithilfe eines anfänglichen &#39;datumsbasierten&#39; Paging-Tokens abzurufen. Dieses Datum wird hauptsächlich verwendet, um die Datenmenge zu begrenzen, die in die Arbeitsmappe importiert werden soll. **Listen-ID**: die ID einer statischen Liste in Marketo, die auf alle Leads/Kontakte verweist, mit denen wir es zu tun haben. Diese statische Liste kann in Marketo frei verwaltet werden (z. B. kann eine Smart-Kampagne sie regelmäßig oder in Echtzeit mit Leads und Kontakten füttern).
Um die ID einer statischen Liste abzurufen, öffnen Sie sie in Marketo und rufen Sie die numerische ID aus der URL ab, z. B. `<https://myorg.marketo.com/#ST3517A1LA1>`, Listen-ID=3511. **Max Records Pages**: Dies wird für unsere pseudo-rekursiven Algorithmen verwendet, die die Marketo-Ausgabedaten mithilfe von „positionsbasierten“ Paging-Token mit einer Kapazität von maximal 300 Datensätzen pro Seite durchlaufen. Da es in unserem Interesse ist, so viele Datensätze pro Seite wie möglich zu erhalten, bleiben wir bei 300. Wenn also die maximale Anzahl an Datensätzen auf 33,333 festgelegt ist, bedeutet dies in der Regel eine Kapazität von 33,333 x 300 = 9,9999 Millionen Datensätze; in Bezug auf Ihr Marketo API-Limit für tägliche Anfragen bedeutet dies jedoch auch 33,333 KB. Die Algorithmen stoppen trotzdem, sobald alle Daten aus den Abfragen abgerufen werden, sodass dieser Parameter nur eine Sicherheitsgrenze für eine Schleife ist.

Tabelle `Leads` mit der Spalte: **Lead-Felder**: Kommagetrennte Lead-Felder, die bei der Abfrage der Leads und Kontakte aus Marketo gesammelt werden sollen. Eine Tabelle in Excel zu deklarieren ist einfach. Geben Sie zwei Zeilen in der Tabelle mit den Spaltennamen und -werten ein, markieren Sie mit der Maus den Umfang der Tabelle, wählen Sie das Symbol Tabelle im Menü „Einfügen“ aus und geben Sie ihr dann einen Namen. Die Namen der Tabellen und ihrer Spalten sind wichtig, da sie direkt von unseren Skripten aufgerufen werden.

## Authentifizierungs- und Zugriffstoken

### Über die Marketo REST-API-Authentifizierung

Marketos REST-APIs werden mit 2-beinigem OAuth 2.0 authentifiziert. Client-IDs und Client-Geheimnisse werden von benutzerdefinierten Services bereitgestellt, die Sie definieren. Jeder benutzerdefinierte Dienst ist im Besitz eines Benutzers, der nur über eine API verfügt und der mit einer Reihe von Rollen und Berechtigungen ausgestattet ist, die den Dienst zum Ausführen bestimmter Aktionen autorisieren. Ein Zugriffs-Token ist einem einzelnen benutzerdefinierten Service zugeordnet.
Der vollständige Authentifizierungsmechanismus wird ([) ](/help/rest-api/authentication.md) der Marketo Developer-Site dokumentiert. Wenn ein Zugriffs-Token ursprünglich erstellt wurde, beträgt seine Lebensdauer 3.600 Sekunden oder 1 Stunde. Bei jedem aufeinander folgenden Authentifizierungsaufruf für denselben benutzerdefinierten Service wird das aktuelle Zugriffstoken mit der verbleibenden Lebensdauer zurückgegeben. Sobald das Token abgelaufen ist, gibt die Authentifizierung ein brandneues Zugriffs-Token zurück. Die Verwaltung des Ablaufs von Zugriffstoken ist wichtig, um sicherzustellen, dass Ihre Integration reibungslos funktioniert und um zu verhindern, dass während des normalen Betriebs unerwartete Authentifizierungsfehler auftreten.

#### Abfrage erstellen

Erstellen Sie eine neue Abfrage, indem Sie auf das Symbol „Neue Abfrage“ im Abschnitt „Get&amp;Transform“ des Menüs „Daten“ klicken. Wählen Sie eine leere Abfrage, mit der Sie beginnen möchten, und geben Sie ihr einen Namen wie „AutoAccessToken“. Starten Sie den erweiterten Editor aus dem Abfrage-Editor, damit Sie manuell Skripte für einige Power Query-Formeln erstellen können. Geben Sie den folgenden Code im erweiterten Editor ein:

```
let
    // Get url and credentials from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
    clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

    // Calling Marketo API Get Access Token
    getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
    TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

    // Parsing access token
    accessTokenStr = TokenJson [access_token]

in
    accessTokenStr
```

Die im Quell-Code eingebetteten Kommentare, denen &quot;//&quot; vorangestellt ist, machen den Code selbsterklärend. Wenn Sie Funktionsreferenzen benötigen, lesen Sie bitte die Links im Referenzabschnitt dieses Artikels. Klicken Sie auf die Schaltfläche „Fertig“. Überprüfen Sie, ob das Zugriffs-Token erfolgreich in der Ausgabe für den letzten angewendeten Schritt „accessTokenStr“ angezeigt wird.  Ein kurzer Kommentar zur Sicherheit in Excel. Sie werden gelegentlich aufgefordert, externe Datenverbindungen über das gelbe Banner zu aktivieren. Dies ist erforderlich, damit die Abfragen ordnungsgemäß funktionieren.

#### Konvertieren einer Abfrage in eine Funktion

Kehren Sie zum erweiterten Editor zurück und schließen Sie den Code mit der folgenden Funktionsdeklaration ein:

```
let
    FnMktoGetAccessToken =()=>

        let
            // Get url and credentials from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
            clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
            clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

            // Calling Marketo API Get Access Token
           getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
           TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

            // Parsing access token from Json
           accessTokenStr = TokenJson [access_token]

        in
            accessTokenStr

in FnMktoGetAccessToken
```

Die Funktion akzeptiert keine Parameter in der Eingabe, ruft diese jedoch aus dem Konfigurationsarbeitsblatt ab. Es erzeugt das Zugriffstoken als Ausgabe. Benennen Sie Ihre `FnMktoGetAccessToken` um und speichern Sie sie. Beachten Sie, dass Sie alle Ihre Abfragen jederzeit in Excel anzeigen können, indem Sie im Abschnitt „Abrufen und Transformieren“ des Menüs Daten auf die Schaltfläche „Abfragen anzeigen“ klicken. Ihre Funktion sollte nun mit dem Funktionssymbol &#39;Fx&#39; markiert sein.

### Member von statischer Liste laden

#### Leads abrufen

Die Marketo-Lead-API bietet einfache CRUD-Vorgänge für Lead-Datensätze, die Möglichkeit, die Zugehörigkeit eines Leads zu statischen Listen und Programmen zu ändern und die intelligente Kampagnenverarbeitung für Leads zu starten. Alle diese Funktionen sind ([) ](/help/rest-api/leads.md). Eine große Anzahl von Lead-Datensätzen kann basierend auf der Mitgliedschaft in einer statischen Liste oder einem Programm abgerufen werden. Mithilfe der ID einer statischen Liste können Sie alle Lead-Datensätze abrufen, die Mitglieder dieser statischen Liste sind. Die ID der Liste ist ein Pfadparameter im Aufruf. Weitere Informationen finden Sie im Kapitel „Liste und Programmmitgliedschaft“ in der Marketo Developers-Dokumentation. Die maximale Anzahl von Lead-Datensätzen, die wir pro API-Aufruf erhalten können, beträgt 300. Daher müssen wir Paging-Token nutzen, um die Datensätze pro Seite von 300 Datensätzen zu erfassen. Wir erhalten das Paging-Token in der JSON-Antwort nach dem ersten Aufruf und wissen, dass wir fertig sind, wenn das Paging-Token nicht mehr in der Ausgabe enthalten ist.

### Grundlegende Abfrage

Beginnen wir mit einer voll funktionsfähigen Abfrage, die darauf abzielt, alle Leads aus einer statischen Liste herunterzuladen. Erstellen Sie eine neue leere Abfrage mit dem Namen „MotoLeads“ und geben Sie den folgenden Code im erweiterten Editor ein:

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the number of iterations (pages of 300 records) - Table Scoping
    iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    pagingTokenParamStr = "",

    // Function iterating though the pages
    FnProcessOnePage =
    (accessTokenParamStr, pagingTokenParamStr) as record =>
        let

            // Send REST API Request
            content = Web.Contents(getMultipleLeadsByListIdUrl & accessTokenParamStr & pagingTokenParamStr),

            // Recover Json output and watch if token is expired, in that case, regenerate access token
            newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
            getMultipleLeadsByListIdJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(getMultipleLeadsByListIdUrl & newAccessTokenParamStr & pagingTokenParamStr)),

            // Parse Json outputs: data and next page token
            data = try getMultipleLeadsByListIdJson[result] otherwise null,
            next  = try  "&nextPageToken=" & getMultipleLeadsByListIdJson[nextPageToken] otherwise null,
            res = [Data=data, Next=next, Access=newAccessTokenParamStr]
        in
            res,

    // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
    GeneratedList =
        List.Generate(
            ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
            each [i]<iterationsNum and [res][Data]<>null,
            each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
            each [res][Data])
in
    GeneratedList
```

Power Query bietet keine herkömmlichen Schleifenfunktionen (z. B. for-loop, while-loop) und unterstützt keine Rekursion. Eine gute Problemumgehung ist die Implementierung einer For-Schleife mithilfe von List.Generate. Diese Funktion ist dokumentiert [hier](http://msdn.microsoft.com/query-bi/m/list-generate). Mit Liste. Generieren Es ist möglich, über die Seiten zu iterieren. Bei jedem Schritt der Iteration extrahieren wir eine Datenseite, wobei die URL, die das Paging-Token für die nächste Seite enthält, beibehalten wird, und die Ergebnisse im nächsten Element der generierten Liste gespeichert werden. Der Blog von [Datachant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) war eine großartige Ressource, um dieses Problem zu lösen. Unser Parameter &#39;Max Records Pages&#39; ist hier, um die Anzahl der Seiten zu begrenzen und sie auf einen realistischen Bereich zu beschränken, um eine Endlosschleife zu vermeiden. Eine weitere Herausforderung besteht darin, sicherzustellen, dass das Zugriffstoken nie abgelaufen ist. Die Verfolgung der verbleibenden Lebensdauer wäre mit Power Query zu komplex. Daher werden alle Aufrufe an die REST-API mit einer Fehlerprüfung gesichert. Wenn ein Fehler auftritt, gehen wir davon aus, dass das Token abgelaufen ist, und erneuern es zuerst und wiederholen den Aufruf dann erneut. Wenn der zweite Aufruf fehlschlägt, wird der zweite Fehler an Excel gemeldet (im schlimmsten Fall erhalten Sie dadurch keine Daten). Starten Sie die Abfrage nach dem Speichern oder jederzeit durch Klicken auf die Schaltfläche „Aktualisieren“.  In unserem Fall wurden 1364 Lead-Datensätze extrahiert, die in 5 Seiten Daten innerhalb von 5 Listen passten.

### Daten formen

Wir müssen die Daten so gestalten, dass alle diese Datensätze in einer einzigen flachen Liste von Datensätzen enthalten sind. Dazu gibt es zwei Möglichkeiten:

* Verwenden von mehr Code
* Verwenden der Power Query-Benutzeroberfläche

Klicken Sie mit der rechten Maustaste auf das Ausgaberaster und wählen Sie im Kontextmenü die Option „In Tabelle“ aus, um die Tabelle in Listen zu konvertieren.  Lassen Sie im Pop-up „An Tabelle“ die Standardwerte in den zwei Auswahllisten.  Erweitern Sie nun die resultierende Listentabelle. Jetzt haben wir alle Datensätze in einer Liste. Die im JSON-Format kodierten Datensätze enthalten die Felder und die zugehörigen Werte. Erneut erweitern. Aktivieren Sie im Popup-Fenster alle Felder, die Sie beibehalten möchten, und deaktivieren Sie das Kontrollkästchen „Ursprünglichen Spaltennamen als Präfix verwenden“.  Et voila! Alle Einträge werden schön in unserer Tabelle angezeigt. Wenn wir den erweiterten Editor erneut öffnen, können wir sehen, dass 3 Codezeilen hinzugefügt wurden, um unsere Daten zu formen:

```
# "Converted to Table" = Table.FromList(GeneratedList, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
# "Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
# "Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"}, {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"})
```

Sie können mit Power Query viel mehr tun, z. B. zusätzliche Spalten mit berechneten Werten erstellen. Später werden wir einige weitere Möglichkeiten sehen. Speichern und schließen wir diese Abfrage. Es kann jetzt jederzeit manuell oder automatisch über Hintergrundaktualisierungen aktualisiert werden.

### Umleiten der Ergebnisse

Die Frage ist nun, wohin die Ergebnisdaten gesendet werden sollen. Bewegen Sie den Mauszeiger über Ihre Abfrage und wählen Sie im Kontextmenü das Menü &#39;Laden, um…&#39;. Im Popup können Sie dann Folgendes auswählen:

* &#39;Tabelle&#39;, wenn Sie alle geformten Daten an ein Arbeitsblatt senden möchten (neues oder vorhandenes),
* &#39;Nur Verbindung erstellen&#39;, wenn Sie weitere Analysen im Power Pivot durchführen möchten.

Die Checkbox &#39;Dies zum Datenmodell hinzufügen&#39; ermöglicht es Ihnen, die Daten im Power Pivot zu nutzen. Dies möchten wir für den zweiten Teil dieses Artikels.

### Verwalten der Seitenumbrüche

Da das Ziel unseres Projekts darin besteht, viele weitere Abfragen zu erstellen, nehmen wir eine Umgestaltung vor und extrahieren eine wiederverwendbare Funktion, die die Paginierung verwalten würde. Erstellen Sie eine neue leere Abfrage **FnMktoGetPagedData** und geben Sie den folgenden Code im erweiterten Editor ein:

```
let
    FnMktoGetPagedData =(url, accessTokenParamStr, pagingTokenParamStr)=>

    let

        // Get the number of iterations (pages of 300 records) - Table Scoping
        iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],

        // Sub-function iterating though the REST API service result pages
        FnProcessOnePage =
        (accessTokenParamStr, pagingTokenParamStr) as record =>
            let

                // Send REST API Request
                content = Web.Contents(url& accessTokenParamStr & pagingTokenParamStr),

                // Recover Json output and watch if token is expired, in that case, regenerate access token
                newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
                contentJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(url & newAccessTokenParamStr & pagingTokenParamStr)),

                // Parse Json outputs: data and next page token
                data = try contentJson[result] otherwise null,
                next  = try  "&nextPageToken=" & contentJson[nextPageToken] otherwise null,
                res = [Data=data, Next=next, Access=newAccessTokenParamStr]
            in
                res,

        // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
        GeneratedList =
            List.Generate(
                ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
                each [i]<iterationsNum and [res][Data]<>null,
                each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
                each [res][Data])
    in
        GeneratedList

in FnMktoGetPagedData
```

Speichern Sie die Abfrage. Wir werden es als Nächstes verwenden.

### Vereinfachte Abfrage

Schreiben wir unsere Abfrage &#39;MktoLeads&#39; erneut um, die die Funktion **FnMktoGetPagedData** aufruft.

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    // No initial paging token required for this call
    pagingTokenParamStr = "",

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getMultipleLeadsByListIdUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Wie Sie sehen können, ist unsere Abfrage jetzt wirklich einfach zu lesen und zu pflegen. Wir werden die Funktion **FnMktoGetPagedData** erneut für die anderen Abfragen nutzen.

### Laden spezifischer Aktivitäten aus einem definierten Zeitraum

#### Abrufen von Aktivitäten mit Paginierung

Marketo ermöglicht eine Vielzahl von Aktivitätstypen im Zusammenhang mit Lead-Datensätzen. Fast jede Änderung, Aktion oder jeder Flussschritt wird im Aktivitätsprotokoll eines Leads aufgezeichnet und kann über die API abgerufen oder in Smart List- und Smart Campaign-Filtern und -Triggern genutzt werden. Aktivitäten sind immer über die Lead-ID mit dem Lead-Datensatz verknüpft, was der ID des Datensatzes entspricht, und verfügen außerdem über eine eigene eindeutige Ganzzahl-ID. Die vollständige REST-API-Dokumentation finden [hier](/help/rest-api/activities.md).

Es gibt eine sehr große Anzahl potenzieller Aktivitätstypen, die von Abonnement zu Abonnement variieren können und für jeden eine eindeutige Definition haben. Zwar hat jede Aktivität ihre eigene eindeutige ID, leadId und activityDate, aber primaryAttributeValueId und primaryAttributeValue variieren in ihrer Bedeutung. Wir werden uns auf die Interesting Moments konzentrieren, eine Art von Marketo verfolgten Aktivitäten mit der ID 41. Die neuen Herausforderungen, die wir lösen werden, sind:

* Wir müssen ein „datumsbasiertes“ Paging-Token initiieren, um den Zeitraum zu definieren, in dem die Aktivitäten stattfanden.
* Die Datengestaltung ist etwas komplizierter, da je nach Aktivitätstyp eine Liste aktivitätsspezifischer Attribute in JSON bereitgestellt wird und ausgewertet und abgeflacht werden muss, um die Analyse zu erleichtern.

#### Datumsbasiertes Paging-Token

Zunächst müssen wir diese Funktion erstellen, um das anfängliche „datumsbasierte“ Paging-Token zu generieren, das erforderlich ist, um den Zeitraum für unsere Aktivitätsabfragen abzudecken. Die Dokumentation zum Paging-Token finden Sie [hier](/help/rest-api/paging-tokens.md). Erstellen Sie eine neue leere Abfrage **FnMktoGetPagingToken** und geben Sie den folgenden Code im erweiterten Editor ein:

```
let
    FnMktoGetPagingToken =(accessTokenStr)=>

        let
            // Get url from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],

            // Get Paging Token SinceDatetime from config worksheet - Table Scoping
            mktoPTSinceDatetimeStr = DateTime.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Paging Token SinceDatetime], "yyyy-MM-ddThh:mm:ss"),

            // Building URL for API Call
            getPagingTokenUrl = mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & accessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr,

            // Calling Marketo API Get Paging Token
            content = Web.Contents(getPagingTokenUrl),

            // Recover Json output and watch if access token is expired, in that case, regenerate it
            newAccessTokenStr = if Json.Document(content)[success]=true then accessTokenStr else "?access_token=" & FnMktoGetAccessToken(),
            pagingTokenJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & newAccessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr)),

            // Parsing Paging Token
            pagingTokenStr = pagingTokenJson[nextPageToken]

        in
            pagingTokenStr

in FnMktoGetPagingToken
```

Speichern Sie die Funktion. Wir werden es als Nächstes verwenden.

#### Aktivitäten mit interessanten Momenten

Schreiben wir nun die Abfrage „MktoInterestingMomentsActivities“, die die Funktionen **FnMktoGetPagedData** und **FnMktoGetPagingToken** aufruft.

```
let

    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),

    // Build Get Activities URL
    getActivitiesUrl = mktoUrlStr & "/rest/v1/activities.json?ListId=" & listIdStr & "&activityTypeIds=46",

    // Build Marketo Access Token URL parameter
    accessTokenStr = FnMktoGetAccessToken(),
    accessTokenParamStr = "&access_token=" & accessTokenStr,

    // Obtain date-based paging token used to scope in time the activities
    pagingTokenParamStr = "&nextPageToken=" & FnMktoGetPagingToken(accessTokenStr),

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getActivitiesUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Das Ergebnis dieser Abfrage ist wiederum eine Liste von Listen, sodass eine weitere Datenverarbeitung erforderlich ist, um für die Analyse verwendet werden zu können.

### Daten formen

Nehmen wir die gleichen Gestaltungsvorgänge vor wie bei den Leads:

* Klicken Sie mit der rechten Maustaste auf das Ausgaberaster und wählen Sie im Kontextmenü die Option „In Tabelle“ aus, um die Tabelle in Listen zu konvertieren.
* die resultierende Listentabelle zu erweitern,
* Einmal erweitern und im Pop-up alle Felder auswählen, die beibehalten werden sollen (deaktivieren Sie das Kontrollkästchen &#39;Ursprünglichen Spaltennamen als Präfix verwenden&#39;).

Sie können die Spalten mit ihren Werten sehen, mit Ausnahme der Spalte „Attribute“, die noch eine Liste spezifischer Attribute enthält, die mit den interessanten Momenten verknüpft sind. Erweitern wir diese Attribute. Nachdem die Liste nun in Datensätze erweitert wurde, erweitern wir sie erneut, indem wir die gewünschten Felder auswählen (Name und Wert jedes Attributs) und deaktivieren das Kontrollkästchen „Ursprünglichen Spaltennamen als Präfix verwenden“.  Infolgedessen sind alle unsere Daten sichtbar, einschließlich der Attribute, aber jede interessante Momentaktivität wird über 3 Zeilen verteilt. Es wird schwierig sein, das für unsere Analyse zu verwenden.  Idealerweise sollte nur eine Zeile pro Aktivität enthalten sein, wobei alle Attribute als zusätzliche Spalten angezeigt werden. Wir können dies einfach tun, indem wir die drei Attribute aus unserer Tabelle drehen. Wählen Sie die beiden Spalten „Name“ und „Wert“ aus den Aktivitätsattributen aus und klicken Sie im Menü „Transformieren“ auf „Pivot-Spalte“. Fragen Sie nach den erweiterten Optionen im Popup und wählen Sie „Werte Spalte“ = Wert und die Wertefunktion „Nicht aggregieren“ aus.  Klicken Sie auf „OK“ und Sie müssen pro Aktivität eine einzige Zeile mit Daten ausgeben.  Die folgenden Code-Zeilen vom Typ „Datenformung“ sollten automatisch an das Skript Ihrer Abfrage angehängt worden sein:

```
  #"Converted to Table" = Table.FromList(result, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
    #"Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}, {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}),
    #"Expanded attributes" = Table.ExpandListColumn(#"Expanded Column2", "attributes"),
    #"Expanded attributes1" = Table.ExpandRecordColumn(#"Expanded attributes", "attributes", {"name", "value"}, {"name", "value"}),
    #"Pivoted Column" = Table.Pivot(#"Expanded attributes1", List.Distinct(#"Expanded attributes1"[name]), "name", "value")
in
    #"Pivoted Column"
```

### Nächste Schritte

Sie sollten jetzt in der Lage sein, alle Abfragen zu entwerfen, die Sie für den Zugriff auf bestimmte Marketo-Daten benötigen, die über die REST-APIs verfügbar sind. Wir hoffen, dass Ihnen dieser Artikel gefallen hat und dass er Ihnen geholfen hat, die großen Vorteile von Excel und Marketo zusammen zu nutzen. Eine Beispielarbeitsmappe mit allen Abfragen finden Sie auch im zweiten Artikel.

### Verweise

#### Power Query

* [Power Query - Übersicht und Lernen](https://support.microsoft.com/en-us/article/Power-Query-Overview-and-Learning-ed614c81-4b00-4291-bd3a-55d80767f81d)
* [Power Query-Formelreferenz](http://msdn.microsoft.com/query-bi/m/power-query-m-reference)
* [Matt Masson Blog](http://www.mattmasson.com/tag/power-query/) bietet einige gute Ressourcen über Power Query
* [DataChant Blog](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) ist sehr nützlich für die Implementierung des Paginierungsmechanismus

Veröffentlicht am _2016-10-18_ von _Philippe_

## Updates vom Herbst 2016

In der Version vom Herbst 2016 fügen wir CRUD-Unterstützung für E-Mail-v2-Variablen und -Module sowie CRUD-Unterstützung für benannte Konten hinzu. Sie können jetzt mithilfe von Marketo-REST-APIs den Inhalt lokal lesen und bearbeiten sowie verschieben, neu anordnen und löschen. Siehe die vollständige Liste der Aktualisierungen unten:

### Lead-Datenbank-APIs

* [**Benannte Konten**](/help/rest-api/named-accounts.md)
   * Neue Endpunkte zum Lesen, Aktualisieren und Löschen benannter Konten
   * Bekannte Probleme:
      * Ab der Version vom Herbst 2016 können Leads nicht mehr über die API mit benannten Konten verknüpft werden

### Asset-APIs

* [**E-Mail**](https://developer.adobe.com/marketo-apis/api/asset/#operation/describeUsingGET_5)
   * Neue Endpunkte zum Bearbeiten von E-Mail v2-Variablen
   * Neue Endpunkte zum Bearbeiten von E-Mail v2-Modulen
   * Bekannte Probleme:
      * Abfragen und Aktualisierungen für Abschnitte, die prädiktive Token enthalten, geben einen Fehler zurück
      * E-Mails mit Inhaltsabschnitten, die prädiktive Token enthalten, werden möglicherweise nicht mit der API genehmigt

Veröffentlicht am _2016-12-07_ von _Kenny_

## Warum wir uns @MarketoDev Twitter Handl3 zurückgezogen haben

Wir haben beschlossen, den @MarketoDev bei Twitter einzustellen. Das Konto wird am 9. Dezember 2011 deaktiviert. Neugierig warum? **Zurückspulen bis Anfang 2014…** Wir haben gerade die Marketo Developers-Site gestartet, um Entwicklern beim Erstellen von APIs zu helfen. Wir wollten das Bewusstsein für die Marketo-Plattform schärfen und die Zutrittsschranken für Entwickler verringern. In Verbindung damit haben wir @MarketoDev geschaffen, um mit unseren Technologiepartnern und Kunden zusammenzuarbeiten, die dazu neigten, integrierte Lösungen mit Marketo zu erstellen. Wie bei jeder schönen neuen Aufgabe waren wir uns nicht sicher, wie sich das entwickeln würde. Anfangs twitterten wir neue Blogposts und neue API-Releases. Die Dinge waren gut; der Traffic zur Entwickler-Site stieg! Wir haben auch eine Reihe von Fragen erhalten, die wir gebührend beantwortet haben. **Spulen Sie vor bis Ende 2016…** Als das Ökosystem [LaunchPoint Partner](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) wuchs, stieg die Anzahl der Tweets. Die Beantwortung der vielen an @MarketoDev gestellten Fragen war für unser kleines Team zu einer großen Aufgabe geworden. Wir erhielten immer mehr allgemeine Produkt- und Verkaufsfragen, die wir an @Marketo oder @MarketoCares weiterleiteten. Wir hatten auch herausgefunden, dass 140 Zeichen einfach nicht genug waren für entwicklungsbezogene Fragen. Die Beantwortung dieser Fragen führte oft zu langen und verwickelten Diskussionen - ein Ansatz, der sich nicht skalieren ließ. Und schließlich analysierten wir die Traffic-Quellen für Besucher der Entwickler-Site und fanden heraus, dass die meisten von der organischen Suche und der „Jetzt abonnieren“-Funktion auf unserem Blog kommen. Aus diesen Gründen haben wir beschlossen, den Stecker auf @MarketoDev zu ziehen. **Von hier an…** Wenn Sie ein Twitter-Fan sind (und wer nicht), haben Sie keine Angst! Unser Unternehmens-Twitter-Handle @Marketo lebt weiter; genau wie unser Kunden-Support @MarketoCares handhabt.

Veröffentlicht am _2016-12-02_ von _David_

## Excel-Integration Teil 2: Erstellen erweiterter Marketo-Berichte und Datenvisualisierungen mit Power Pivot und Power View-

Dies ist der zweite in einer Reihe von zwei Artikeln, in denen erläutert wird, wie Sie die in Microsoft Excel integrierte Power BI-Technologie nutzen können, um mit Marketo ein echtes Self-Service-Geschäftsanalyseerlebnis zu schaffen. Mit den in diesen Artikeln behandelten Konzepten können Sie:

* Importieren von Daten aus Marketo in Excel
* Importieren und Kombinieren von Daten aus anderen Quellen (SaaS-Anwendungen, Datenbanken, flache Dateien usw.)
* Gestalten von Daten für Geschäftsanforderungen und Analysezwecke
* Daten bei Bedarf in Excel aktualisieren
* Erstellen von berechneten Spalten und Kennzahlen mithilfe von Formeln
* Erstellen von Beziehungen zwischen heterogenen Daten
* Daten analysieren und erweiterte Berichte mit Pivot-Tabellen und Pivot-Diagrammen erstellen
* Hervorragende Datenvisualisierungen erstellen

### Power Pivot und Power View für Excel

In diesem Artikel zeigen wir Beispiele, wie Sie Folgendes erstellen können:

* Erweiterte Marketo-Berichte, die mithilfe von Power Pivot Beziehungen zwischen verschiedenen Sammlungen von Marketo-Daten nutzen
* Coole statische und animierte Visualisierungen mit Power View

**Power Pivot** ist ein Excel-Add-in, das bereits in Excel 2016 enthalten ist und mit dem Sie leistungsstarke Datenanalysen durchführen und ausgefeilte Datenmodelle erstellen können. Mit Power Pivot können Sie große Datenmengen aus verschiedenen Quellen zusammenführen, schnell Informationsanalysen durchführen und Erkenntnisse einfach teilen. Daten, die mit Power Query aus verschiedenen Datenquellen extrahiert wurden, können an das Datenmodell, die Excel-Tabelle oder an beide gesendet werden. Im ersten Artikel haben wir Daten aus Marketo importiert und geformt und an das Datenmodell gesendet, um eine komplexere Analyse durchzuführen, bevor sie in der Tabelle verfügbar gemacht werden. **Power View** ist eine Alternative zur Excel-Visualisierungsebene. Dabei handelt es sich um ein interaktives Erlebnis zur Datenexploration, -visualisierung und -präsentation, das zu intuitivem Ad-hoc-Reporting und -Dashboard ermutigt.

Alle in diesem Artikel beschriebenen Schritte wurden in Excel 2016 für Windows getestet. Die Konzepte sollten für Excel 2013 oder Excel 2010 (ohne Power View) identisch sein, aber einige Anpassungen könnten erforderlich sein. Power Pivot und Power View sind derzeit nur in der Microsoft Windows-Version von Excel 2016 verfügbar, die Office 365-Version von Excel 2016 unterstützt Power Pivot und Power View nicht vollständig.

### Die Marketo Power-Arbeitsmappe

#### Arbeitsmappe herunterladen

Im ersten Artikel haben wir den Datenimport und -formungsprozess mithilfe der Power Query-Technologie behandelt. Wir haben gelernt, wie Sie einige erweiterte Stromabfragen implementieren können, um Leads und Aktivitäten aus Marketo zu extrahieren. Denn einige von Ihnen möchten direkt zu dem Punkt springen, an dem sie Berichte und Visualisierungen erstellen, ohne zu codieren. Diese Arbeitsmappe enthält alle Abfragen, die im ersten Artikel beschrieben wurden, und einige weitere. Wir haben die Fehlerbehandlung verbessert und einige zusätzliche Parameter im Konfigurationsarbeitsblatt hinzugefügt. Wenn Sie den ersten Artikel durchgesehen haben, empfehlen wir Ihnen dennoch, die Marketo Power-Arbeitsmappe herunterzuladen und sich anzusehen, was hinzugefügt wurde.

Haftungsausschluss: Die Marketo Power Workbook ist kein offizielles Marketo-Produkt und wird daher von Marketo nicht unterstützt. Nutzen Sie die Funktionen und erweitern Sie sie für Ihre persönlichen Geschäftsanforderungen, tun Sie dies jedoch auf eigene Gefahr.

#### Arbeitsmappe konfigurieren

Füllen Sie alle erforderlichen Informationen aus dem Marketo-Konfigurationsarbeitsblatt aus:

* **Marketo REST-API-Authentifizierung:** erforderlich
* **Umfang:** Sie das Paging-Token SinceDateTime und die ID Ihrer statischen Marketo-Liste ein, die alle Leads enthält, die Sie analysieren möchten
* **Leads:** Für künftige Berichte müssen Sie mindestens die folgenden Lead-Felder angeben: `id`, `firstName`, `lastName`, `email`, `edAt`, `updatedAt`, `title`, `company`, `industry`, `inferredCountry`, `inferredCity`
   * Wenn die Stadtinformationen in einem Ihrer benutzerdefinierten Felder genauer sind, können Sie stattdessen Ihr eigenes Feld verwenden
* **Aktivitäten:** Aktivitätstypen, die aus der Marketo-Datenbank abgerufen werden sollen, sind hier für jeden Aktivitätssatz angegeben. Dies muss jetzt nicht geändert werden.
   * Beachten Sie, dass wir für die Arbeitsmappe eine Dienstprogrammabfrage bereitgestellt haben, die direkt in der Excel-Arbeitsmappe alle vorhandenen Aktivitätstypen auflistet, wenn Sie diese Informationen später anpassen möchten

Beachten Sie, dass möglicherweise einige sicherheitsbezogene Popup-Fenster angezeigt werden. Externen Verbindungen vertrauen und auf „Öffentlich“ setzen. Wenn das unten angezeigte Popup angezeigt wird, behalten Sie den Inhalt des Web-Zugriffs „Anonym“ bei. Die Authentifizierung bei Marketo wird direkt von unseren benutzerdefinierten Abfragen verwaltet, sodass Sie keinen anderen Zugriff aktivieren müssen.

#### Herunterladen von Marketo-Daten

Stellen Sie zunächst sicher, dass der Parameter, den Sie im Bereich Umfang des Marketo-Konfigurationsarbeitsblatts definieren, nicht dazu führt, dass zu viele Daten heruntergeladen werden, wodurch das tägliche Marketo-API-Anforderungslimit überschritten wird. Wenn Sie bereit sind, klicken Sie auf die Schaltfläche „Alle aktualisieren“ im Menü „Daten“ und warten Sie, bis alle Daten in die Arbeitsmappe heruntergeladen wurden. Wenn beim Herunterladen der Daten Formatierungsfehlermeldungen angezeigt werden, ähnlich wie „Column1 nicht gefunden“, bedeutet dies, dass eine oder mehrere Abfragen die Daten nicht abrufen, sodass die Formatierung ebenfalls fehlschlägt. Versuchen Sie es später erneut, wenn der Fehler weiterhin besteht, überprüfen Sie Ihre Excel-Version (verwenden Sie nicht Excel 2016 aus Office 365). Es ist auch wichtig, die Latenz der Marketo-Plattform zu respektieren. Wenn Sie Änderungen an einer statischen Liste oder an Ihren Lead-Daten vornehmen, sollten Sie warten, bevor Sie die Energieabfragen starten.

### Datenmodellierung in Power Pivot

Öffnen Sie Power Pivot, indem Sie auf die Schaltfläche „Verwalten“ im Power Pivot-Menü klicken, das in der oberen Menüleiste verfügbar ist (falls nicht verfügbar, überprüfen Sie Ihre Version von Excel, Power Pivot kann als Add-in in einigen Versionen von Excel installiert werden). Alle von Marketo heruntergeladenen und an das Datenmodell gesendeten Daten sollten über die verschiedenen Registerkarten am unteren Rand des Power Pivot-Fensters zugänglich sein.

### Ausdrücke für die Datenanalyse (DAX)

Für einige Berichte müssen wir die Daten anreichern oder neu formatieren. Verwenden wir Power Pivot-Datenanalyseausdrücke (DAX), um einige benutzerdefinierte Berechnungen als berechnete Spalten und Kennzahlen (auch als berechnete Felder bezeichnet) zu definieren. Weitere Informationen zu DAX finden Sie unter dem Link „DAX in Power Pivot“ im Abschnitt Referenzen . Stellen Sie sicher, dass der Berechnungsbereich im Power Pivot-Fenster angezeigt wird. Aktivieren Sie ihn andernfalls über das Power Pivot-Startmenü.  Wählen Sie die Registerkarte **MktoLeads** aus und fügen Sie die Kennzahl **Leads-Anzahl** an einer beliebigen Stelle im Berechnungsbereich für Leads hinzu: **Leads-Anzahl:=**&#x200B;**DISTINCTCOUNT**&#x200B;**([id])**. Diese Kennzahl zählt die in der Liste verfügbaren eindeutigen Leads basierend auf ihrer ID. Dabei würden auch die eventuellen Filter berücksichtigt, die im Rahmen eines Berichts vorhanden sind. Diese Maßnahme ist nicht wirklich notwendig, da die Berichte in der Lage sind, die Anzahl der Leads zu summieren, aber wir haben es getan, um eine Lead-Anzahl mit einem schöneren Namen als „Summe der MiktoLeads“ zu haben. Es ist auch ein einfaches Beispiel, mit dem Sie sich leicht einige komplexere Messwerte vorstellen können, die für einen bestimmten Typ von Dateneingabe Durchschnittswerte, Min. und Max. erreichen (z. B. alle Leads mit einem Score über 50, Durchschnittswert usw.). …)  Wählen wir nun die Registerkarte **MktoWebActivities** aus und erstellen wir drei berechnete Spalten. Fügen Sie die folgenden berechneten Spalten ein, indem Sie nach ganz rechts in der Tabelle scrollen und auf die Spalte „Spalte hinzufügen“ klicken. **Aktivität** Rufen Sie die benutzerfreundliche Aktivitäts-Kennzeichnung ab, indem Sie die Aktivitäts-ID in der Tabelle „MktoActivityTypes“ nachschlagen. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoActivityTypes[name],MktoActivityTypes[id],[activityTypeId]**)**Year-Month:** Formatieren Sie das Aktivitätsdatum mit dem Muster „JJJJJMM“, das für einige Berichte besser geeignet ist. **\=**&#x200B;**LEFT**&#x200B;**([activityDate],4)&amp;**&#x200B;**MID**&#x200B;**([activityDate]**,6,2)**Date:** Aktivitätsdatum ist nur eine Zeichenfolge aus unserer ursprünglichen Abfrage, wandeln Sie sie in ein ordnungsgemäßes Datum um. **\=**&#x200B;**DATE**&#x200B;**(**&#x200B;**LEFT**&#x200B;**([activityDate],4),**&#x200B;**MID**&#x200B;**([activityDate],6,2),**&#x200B;**MID**&#x200B;**([activityDate],9,2))** jetzt dieselben Kennzahlen für die Registerkarte **MktoEmailActivities** und zwei weitere Kennzahlen erstellen: **Campaign:** Rufen Sie den benutzerfreundlichen Kampagnennamen ab, indem Sie die Kampagnen-ID in der Tabelle MktoCampaign nachschlagen. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaign[name],MktoCampaign[id],[campaignId]**)**program:** Rufen Sie den benutzerfreundlichen Programmnamen ab, indem Sie die Kampagnen-ID in der Tabelle MktoCampaign nachschlagen. Die Tabelle MotoPrograms enthält weitere Informationen zum Programm, wie z. B. Ordner, Arbeitsbereich usw. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaign[programName],MktoCampaign[id],[campaignId])**

### Entitäts-Beziehungen

Wir haben zuvor eine Möglichkeit gesehen, Informationen aus einer anderen Tabelle innerhalb des Modells nachzuschlagen, um einige fehlende Informationen zu vervollständigen. Power Pivot bietet eine leistungsfähigere Option, um die Beziehungen zwischen einigen Tabellen des Datenmodells zu definieren, sodass wir diese Beziehungen direkt aus den Berichten nutzen können. Definieren wir die Schlüsselbeziehungen für unsere Berichte. Wählen Sie im Power Pivot-Fenster die Diagrammansicht aus. Verfolgen Sie die folgenden Beziehungen innerhalb des Datenmodelldiagramms:

* **MktoInterestingMomentActivities:leadId →** **MktoLeads:id**
* **MktoScoringActivities:leadId →** **MktoLeads:id**
* **MktoRevenueStageActivities:leadId →** **MktoLeads:id**
* **MktoWebActivities:leadId →** **MktoLeads:id**
* **MktoEmailActivities:leadId →** **MktoLeads: id**

Wir verwenden nicht alle diese Beziehungen und Objekte in unseren Berichten, sondern nur Leads, Web-Aktivitäten und E-Mail-Aktivitäten. Jetzt ist es an der Zeit, einige Berichte zu erstellen.

### E-Mail-Performance Pivot-Diagramm

Dieser erste Bericht zeigt KPIs der E-Mail-Leistung basierend auf einem standardmäßigen Excel-Pivot-Diagramm an. Damit können wir Daten nach Branche und/oder Kampagne filtern. Sie können ein Pivot-Diagramm direkt aus dem Power Pivot-Menü erstellen, indem Sie „Pivot-Diagramm“ aus der Auswahl „Pivot-Tabelle“ auswählen.  Eine Alternative besteht darin, ein Pivot-Diagramm direkt aus der Excel-Tabelle zu erstellen, indem die Option „Verwenden des Datenmodells dieser Arbeitsmappe“ angekreuzt wird.  Ziehen Sie die Felder aus den Tabellen **MktoEmailActivities** und **MktoLeads** wie in der folgenden Abbildung dargestellt: **MktoEmailActivities.Activity →** **Legend** (verwenden Sie dazu die berechnete Spalte DAX, die wir **MktoEmailActivities** zuvor implementiert haben) **MktoEmailActivities.Date →** Axis **(verwenden Sie dazu die berechnete Spalte DAX, die wir zuvor in** MktoEmailActivities **implementiert haben)** MktoEmailActivities.Id →**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;** IdId **Id Id Id ∑Id Werte** MKTOemailactivities.campaign →Filter mktoLeads.industry →filter

Sie können einen benutzerdefinierten Namen erstellen, indem Sie „Wertfeldeinstellungen“ für jedes abgelegte Feld auswählen. In diesem Fall haben wir das Feld E-Mail-Aktivitäts-ID im Abschnitt &quot;∑ Werte“ abgelegt und seinen benutzerdefinierten Namen als „Anzahl der Aktivitäten“ bearbeitet. Konfigurieren wir nun das Pivot-Diagramm. Klicken Sie mit der rechten Maustaste direkt auf das Diagramm und wählen Sie im Kontextmenü die Option „Diagrammtyp ändern“. Und so haben wir den verschiedenen Diagrammtyp für alle Datenreihen ausgewählt.

### Lead-Karte mit Power View

Der zweite Bericht zeigt Ihre Leads und Kontakte nach Geografie auf einer Weltkarte und nach Branche an. Für diesen Bericht benötigen wir Power View. Folgen Sie dem unten stehenden Link, um das Menü in Excel zu aktivieren. Oder Sie geben einfach „power view“ in das Excel-Suchfeld ein. Wählen Sie „Power View-Bericht einfügen“.  Wählen Sie im leeren Power View-Bericht die **MktoLeads**-Tabelle im rechten Bedienfeld aus und ziehen Sie das Feld für die Lead-Position per Drag-and-Drop (z. B **inferredCity**). Jetzt erscheint das Menü &#39;Design&#39; im Hauptmenü.

Wechseln Sie zur Kartenvisualisierung, indem Sie im Power View-Menü „Design“ die Option „Map“ auswählen. Ziehen Sie die Felder wie in der folgenden Abbildung dargestellt per Drag-and-Drop aus der **MktoLeads**-Tabelle: **MktoLeads.Industry →** **Color** **MktoLeads.inferredCity →**&#x200B;**Locations** **MktoLeads.Lead-Anzahl →**&#x200B;**∑Größe** (verwenden Sie dazu die DAX-Kennzahl, die wir **MktoLeads** zuvor implementiert haben). Ihre Lead-Zuordnung ist fertig! Sie müssen nur die Größe der Karte anpassen, den Titel und die Legenden anpassen. Mit Power View können Sie erweiterte Dashboards mit mehreren Diagrammen auf einer einzigen Tabelle erstellen. Sehen Sie sich das referenzierte Tutorial unter „Erstellen [ beeindruckenden Power View-](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)&quot; an, um zu erfahren, wie Sie mit weiteren Dashboard-Komponenten mit Power View fortfahren.

### Web-Aktivitäten auf einer 3D-Karte animiert

Dieser dritte Bericht zeigt Ihre Lead-Web-Aktivitäten nach Branchen auf einer 3D-Weltkarte an. Wir brauchen eine 3D-Karte für diesen Bericht. Geben Sie einfach „3D“ in das Excel-Suchfeld ein und wählen Sie „3D Map“. Erstellen Sie eine neue Tour aus dem Popup-Fenster.  Wählen Sie das Blasendiagramm im rechten Bedienfeld aus. Ziehen Sie die Felder aus den Tabellen **MktoLeads** und **MktoWebActivities** wie in der folgenden Abbildung dargestellt: **MktoLeads.Industry →** **Category** **&#x200B;**&#x200B;MktoLeads.inferredCity →**Location** **MktoWebActivities.Activity →**&#x200B;**Time** (verwenden Sie dazu die berechnete Spalte DAX, die wir zuvor unter **MktoWebActivities** haben. Das Feld ID kann auch zum Zählen von Aktivitäten verwendet werden.) **MktoWebActivities.Date →** **Time** (dieses verwendet die berechnete DAX-Spalte, die wir **MktoWebActivities** zuvor implementiert haben) **MktoWebActivities.Activity** kann auch als Filter zum Herausfiltern der verschiedenen Arten von Web-Aktivitäten verwendet werden.

Verwenden Sie die Schaltfläche „Designs“, um das Farbschema Ihrer 3D-Karte zu ändern. Öffnen Sie die „Szenenoptionen“, um Ihre Animationen anzupassen.
Und wenn man mit der 3D-Weltkarte fertig ist, kann man jetzt Spaß daran haben, die Welt zu animieren und daraus Videos zu machen.

### Nächste Schritte

Wir haben gerade erst an der Oberfläche der Möglichkeiten für die Excel Power BI-Tools gekratzt. Wir empfehlen Ihnen, im Internet nach anderen großartigen Artikeln und Tutorials zu suchen, um Ihre Excel-Kenntnisse zu erweitern und die Berichte zu entwerfen, die Sie benötigen, um Ihre Geschäftsziele zu erreichen. Wir hoffen, dass Ihnen diese Artikel gefallen haben und dass sie Ihnen geholfen haben, die großen Vorteile von Excel und Marketo zusammen zu nutzen.

### Verweise

#### Machtachse

* [Power Pivot: Leistungsstarke Datenanalyse und Datenmodellierung in Excel](https://support.microsoft.com/en-us/article/Power-Pivot-Powerful-data-analysis-and-data-modeling-in-Excel-d7b119ed-1b3b-4f23-b634-445ab141b59b)
* [Datenanalyseausdrücke (DAX) in Power Pivot](https://support.microsoft.com/en-us/article/Data-Analysis-Expressions-DAX-in-Power-Pivot-bab3fbe3-2385-485a-980b-5f64d3b0f730)

#### Power View

* [Power View einschalten in Excel 2016](https://support.microsoft.com/en-us/article/Turn-on-Power-View-in-Excel-2016-for-Windows-f8fc21a6-08fc-407a-8a91-643fa848729a)
* [Tutorial: Erstellen Sie beeindruckende Power View-Berichte](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)

Veröffentlicht am _2017-02-02_ von _Philippe_

## Wichtige Änderung an Aktivitätsdatensätzen in der Marketo-API

**Hinweis: Dieser Beitrag wird aktualisiert, um die Änderungen widerzuspiegeln, die an den von der API aufgrund der Migration auf eine neue Infrastruktur zurückgegebenen Aktivitätsdatensätzen vorgenommen wurden.** **Letzte Aktualisierung: 13. September 2018** Mit dem Rollout des Activity Service der nächsten Generation von Marketo ab September 2017 können wir die Eindeutigkeit oder das Vorhandensein des ganzzahligen Felds „id“ in Aktivitäten, Datenwertänderungen oder Lead-Löschdatensätzen, die von Marketo-APIs zurückgegeben werden, nicht erzwingen. Um Service-Unterbrechungen für Integrationen zu vermeiden, die Aktivitätsdatensätze abrufen, sollte das ID-Feld als optional behandelt werden. Die Umstellung dieser Änderung wird sich auf Abonnements und die bevorstehende Version auswirken. Diese Änderung betrifft die folgenden Endpunkte: REST-API

Die betroffenen SOAP-Typen sind `ActivityRecord` und `LeadChangeRecord`.

### Beispiele

Die folgenden Beispiele zeigen die betroffenen Datensatztypen. In beiden Beispielen wird das betroffene Feld „id“ genannt.

**Beispiel-REST-Feld: id**

```json
  {
      "id" : 102988,
      "leadId" : 1,
      "activityDate" : "2015-01-16T23:32:19Z",
      "activityTypeId" : 1,
      "primaryAttributeValueId" : 71,
      "primaryAttributeValue" : "localhost/munchkintest2.html",
      "attributes" : [
          {
              "name" : "Client IP Address",
              "value" : "10.0.19.252"
          },
          {
              "name" : "Query Parameters",
              "value" : ""
          },
          {
              "name" : "Referrer URL",
              "value" : ""
          },
          {
              "name" : "User Agent",
              "value" : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
          },
          {
              "name" : "Webpage URL",
              "value" : "/munchkintest2.html"
          }
      ]
  }
```

**Beispiel für ein SOAP-Feld: id**

```xml
  <activityRecord>
    <id>1030680</id>
    <activityDateTime>2013-07-09T16:44:28-05:00</activityDateTime>
    <activityType>Visit Webpage</activityType>
    <mktgAssetName>ClickDemo</mktgAssetName>
    <activityAttributes>
      <attribute>
        <attrName>Webpage ID</attrName>
        <attrType xsi:nil="true" />
        <attrValue>2547</attrValue>
      </attribute>
      <attribute>
        <attrName>Webpage URL</attrName>
        <attrType xsi:nil="true" />
        <attrValue>/ClickDemo.html</attrValue>
      </attribute>
    </activityAttributes>
    <campaign />
    <personName xsi:nil="true" />
    <mktPersonId>1089965</mktPersonId>
    <foreignSysId xsi:nil="true" />
    <orgName xsi:nil="true" />
    <foreignSysOrgId xsi:nil="true" />
  </activityRecord>
```

Veröffentlicht am _2017-03-01_ von _Kenny_

## Updates Winter 2017

In der Winterversion 2017 fügen wir die Möglichkeit hinzu, benutzerdefinierte Objektdaten asynchron per Massenimport zu importieren und Variablen in geführten Landingpages zu bearbeiten. Unten finden Sie eine vollständige Liste der Updates.

### Lead-Datenbank-APIs

#### Massenimport von benutzerdefinierten Objekten

Neue Endpunkte zur Unterstützung des Massenimports von benutzerdefinierten Objekten. Details finden Sie [hier](/help/rest-api/custom-objects.md).

#### Benachrichtigung über bevorstehende Änderungen an Aktivitäten

Beachten Sie eine wichtige Änderung, die eintritt, wenn Marketo seinen Aktivitäts-Service der nächsten Generation veröffentlicht. Diese Änderung wirkt sich auf die folgenden Endpunkte aus:

SOAP

[getLeadActivity](/help/soap-api/getleadactivity.md), [getLeadChanges](/help/soap-api/getleadchanges.md)

Das in den von diesen Endpunkten zurückgegebenen Datensätzen enthaltene ganzzahlige Feld „id“ ist nicht mehr eindeutig. Dies wirkt sich auf die Datensatztypen für Aktivitäten, Datenwertänderungen und Lead-Löschungen aus. Um Service-Unterbrechungen für Integrationen zu vermeiden, die diese Datensatztypen abrufen, sollte das ID-Feld als optional behandelt werden.

#### Entfernen von Push-Token

Es wurde die Möglichkeit hinzugefügt, Push-Token über die SDK-API zu entfernen. Details finden Sie hier: [iOS](/help/mobile/push-notifications.md), [Android](/help/mobile/push-notifications.md).

Veröffentlicht am _2017-03-01_ von _David_

## Updates Frühjahr 2017

In der Version vom Frühjahr 2017 haben wir die Möglichkeit hinzugefügt, Lead- und Aktivitätsobjektdaten asynchron zu extrahieren und benannte Kontolisten zu bearbeiten. Unten finden Sie eine vollständige Liste der Updates.

### Massenextraktion von Leads

Neue Endpunkte zur Unterstützung der Massenextraktion von Leads. Geben Sie die Datensatzauswahlkriterien mit einer Vielzahl von Optionen an. Details finden Sie [hier](/help/rest-api/bulk-lead-extract.md).

### Massenextraktion von Aktivitäten

Neue Endpunkte zur Unterstützung der Massenextraktion von Aktivitäten. Geben Sie die Auswahlkriterien für Datensätze mithilfe von Datumsbereich und Aktivitätsliste an. Details finden Sie [hier](/help/rest-api/bulk-extract.md).

### Spezifische Kontolisten

Neue Endpunkte zur Unterstützung von CRUD-Vorgängen für spezifische Kontolisten. Details finden Sie [hier](/help/rest-api/named-account-lists.md).

### Weitere Verbesserungen

* Der Endpunkt [Kampagnen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignByIdUsingGET) ermöglicht jetzt das Filtern „auslösbarer“ Kampagnen. Dies wird erreicht, indem „isTriggerable=true“ als Abfrageparameter übergeben wird.
* Der Endpunkt [Programm klonen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) unterstützt jetzt Programme, die alle Asset-Typen außer SMS-Nachrichten enthalten.

### ionisch

Sie können jetzt Marketo Mobile MME und mit dem [Ionic](https://ionicframework.com/)-Anwendungs-Framework verwenden. Details finden Sie [hier](/help/mobile/ionic.md).

### Push-Benachrichtigungs-Token abmelden

Der SDK wurde eine neue Methode hinzugefügt, mit der Sie die Registrierung des Push-Benachrichtigungs-Tokens bei Marketo aufheben können. Dies ist beispielsweise nützlich, wenn sich ein Benutzer von Ihrer App abmeldet. Weitere Informationen zur Verwendung finden Sie unter `unregisterPushDeviceToken` (iOS) oder `uninitializeMarketoPush` (Android[Methode](/help/mobile/push-notifications.md).

Veröffentlicht am _2017-06-16_ von _David_

## Internet der Dinge für Marketing-Experten mit IFTTT und Zapier

Das Internet der Dinge (IoT) ist die Vernetzung von vernetzten Geräten, Geräten, Wearables, Fahrzeugen usw. mit integrierter Elektronik, Software, Sensoren und Netzwerkkonnektivität, die es diesen Objekten ermöglicht, Daten zu sammeln und mit Cloud-Informationssystemen auszutauschen. Diese Technologien wachsen und entwickeln sich so schnell, dass sie sich in kürzester Zeit auf unsere Lebensweise, unsere Arbeitsweise und unsere Geschäftstätigkeit auswirken werden. Marketo ist die führende Marketing-Interaktionsplattform für das IoT mit ihren Funktionen zur Skalierung und Interaktion mit jeder Form von Kommunikationskanal. Marketo kann bereits über 70 Aktivitätstypen im Zusammenhang mit E-Mails, Web, Mobile, CRM usw. nachverfolgen und unterstützt auch [benutzerdefinierte Aktivitäten](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-activities/create-a-custom-activity.html?lang=de) die von jedem Drittanbietersystem gespeist werden können. Marketo [Benutzerdefinierte Objekte](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects.html?lang=de) ermöglichen es, alle Arten von Drittanbietermetriken in Bezug auf Ihr Unternehmen zu verfolgen, und ermöglichen es Marketing-Experten, diese Metriken direkt über Marketo Smart Campaign-Filter und -Trigger zu nutzen. Die Implementierung von IoT für Privatkunden würde einen zentralen Server für die Interaktion mit Privatkunden-Geräten erfordern. Dieser Server würde Daten mit der offenen Marketo-Plattform austauschen, einschließlich Funktionen wie REST-API, benutzerdefinierte Objekte, benutzerdefinierte Aktivitäten usw. - dokumentiert [hier](http://eto.com/). Es ist nicht einfach, das mit einem Blogpost zu demonstrieren. Stattdessen werden wir den IFTTT-Service mit Marketo integrieren, um einige coole IoT-Anwendungsfälle für Marketing-Experten zu implementieren, z. B.:

* Jedes Mal, wenn ein Lead zu einer Roadshow zugelassen wird, muss das Marketing-Team aufmuntert werden, indem im Büro ein farbiges Licht blinkt
* Jedes Mal, wenn ein Deal gewonnen wird, wird das Vertriebsteam durch die automatische Zündung einer Glocke aufgeheizt, die an einen angeschlossenen Netzstecker angeschlossen ist
* Erfolgsmeilensteine des Marketings automatisch in sozialen Netzwerken wie LinkedIn, Facebook, Slack usw. posten …
* Automatisches Starten einer Marketing-Kampagne basierend auf:
   * Wenn ein Wetteralarm auftritt (Wind, Temperatur, Regen usw.)
   * wenn ein neuer Artikel von einer Zeitung wie der New York Times veröffentlicht wird, die bestimmte Kriterien erfüllt
   * wenn der US-Senat oder das Repräsentantenhaus abstimmen
   * wenn die Internationale Weltraumstation über einen bestimmten Standort führt
   * usw. …

Sie mögen diese Szenarien zwar lustig, aber nutzlos finden, aber sie sind hier, um einen neuen konzeptionellen Weg zu demonstrieren, Marketing nicht nur mit Menschen, sondern auch mit Dingen in unserer vernetzten Welt zu tun. Ein weiterer interessanter Punkt, der in diesem Artikel behandelt wird, ist die Nutzung einer offenen Web-Integrationsplattform wie Zapier als „Serving Hatch“ zwischen einem Drittanbietersystem und Marketo, um z. B. die Authentifizierung zu verwalten.

### Der IFTTT-Dienst

IFTTT ist ein Akronym für „IF This Then That“. Es ist ein kostenloser webbasierter Dienst, den Menschen verwenden, um Ketten von einfachen bedingten Aussagen, so genannten Applets, zu erstellen. Ein Applet wird durch Änderungen ausgelöst, die in einigen Partner-Web-Services auftreten, und daher werden Aktionen an andere Partner-Web-Services gesendet. IFTTT wurde 2011 von Linden Tibbets, Jesse Tane, Scott Tong und Alexander Tibbets in San Francisco gestartet. Auf den ersten Blick ähnelt IFTTT einem Dienst wie [Zapier](https://zapier.com/) Beispielsweise konzentriert es sich viel stärker auf Verbraucher und IoT-Geräte (Fernbedienungen, Alarme, Lichter, Thermostate, Autos, Drucker, Mobiltelefone und so viel mehr).  Zunächst müssen Sie ein Konto für IFTTT auf der [IFTTT-Website](https://ifttt.com/explore) erstellen. Entdecken Sie alle coolen Applets, die bereits verfügbar sind, denn das wird Ihnen einige andere Szenarien Ideen geben!

### Der Maker-Kanal

Eine Web-Anwendung ohne Kanal, d. h. eine Partnerschaft mit IFTTT, muss den Maker-Kanal verwenden. Mit dem Maker-Kanal können Sie Applets erstellen, die mit jedem Gerät oder jeder App funktionieren, das bzw. die eine Web-Anfrage stellen oder empfangen kann. Es bietet die folgenden Integrationen:

* Eingehende Trigger, die eine Web-Anfrage von einem Drittanbietersystem erhalten, um eine Aktion Trigger
* Ausgehende Aktionen , um eine Web-Anfrage an ein Drittanbietersystem im Internet öffentlich zugänglich zu machen

Suchen Sie in IFTTT nach dem Service „Maker“ und klicken Sie darauf.  Beim ersten Mal müssen Sie es aktivieren, indem Sie auf die Schaltfläche „Verbinden“ klicken.  Jetzt ist der Maker-Kanal aktiv. Sie können Ihren geheimen Schlüssel erhalten, indem Sie auf die Schaltfläche Maker-Einstellungen klicken: Kopieren Sie die bereitgestellte URL und fügen Sie sie für weitere Details in Ihren Browser ein.

### Direktes Auslösen einer IFTT-Aktion vom Markt

Zunächst konzentrieren wir uns darauf, alle Arten von Webservice-Aktionen von Drittanbietern aus Marketo auszulösen. Dazu verwenden wir einen [Marketo-Webhook](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook.html?lang=de). Wir beginnen mit einer Push-Nachricht auf Ihrem Mobiltelefon oder Tablet über die IFTTT-Mobile-App, und dann implementieren wir ein IoT-Szenario, das eine Philips Hue-Lampe blinkt.

### Marketo Webhook

Es ist einfach, ein Ereignis aus Marketo als „if“ von IFTTT Trigger. Sie müssen lediglich eine POST-Web-Anfrage an IFTTT mit einem Ereignisnamen und Ihrem geheimen Schlüssel senden, wobei Sie dieser Muster-URL folgen:

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}>`

Der Maker ermöglicht auch die Kommunikation von bis zu 3 Parametern über die Web-Anfrage. Dies kann mithilfe von Abfrageparametern erfolgen,

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={value1}&value2={value2}&value3={value3}>`

oder unter Verwendung eines JSON-Bodys, der aus bis zu drei Werten besteht:

`{ "value1" : "{value1}", "value2" : "{value2}", "value3" : "{value3}" }`

Erstellen Sie in Marketo einen neuen Webhook über die Admin-Benutzeroberfläche.  Geben Sie die folgenden Informationen für Ihren neuen Webhook an:

**Webhook-Name:** Erfolg des IFTTT-Programms

**Beschreibung:** Trigger eines Ereignisses auf IFTTT von einer Smart Campaign for a Program Success

**URL:** `<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={{program.name}}&value2={{lead.Email> Address}}&value3={{lead.Full Name}}`

event_name, z. B. MarketoProgramSuccess

secret_key, verwenden Sie den geheimen Schlüssel aus Ihrem IFTT Maker-Service

Verwenden Sie einen statischen Text oder Marketo-Token für die drei verfügbaren Werte. Sie können interaktivere Nachrichten pushen, indem Sie Ihre eigenen Token auf Programmebene definieren und diese Werte übergeben.

**Anfragetyp:** POST

**template:** Leer lassen

**Request Token Encoding:** form/url

**Antworttyp:** keine

Es ist nicht erforderlich, ein Antwort-Mapping zu definieren.

### IFTTT-Applet

Wählen Sie im IFTTT-Webportal im Hauptmenü „Meine Applets“ aus.  Klicken Sie auf die Schaltfläche „Neues Applet“ und klicken Sie auf den Bereich **+this**.  Suchen Sie nach dem Maker-Service.  Erstellen Sie den Trigger , der jedes Mal ausgelöst wird, wenn der Maker-Service eine Web-Anfrage erhält, um ihn über ein Ereignis zu informieren. Verwenden Sie denselben Ereignisnamen wie in der URL Ihres Marketo-Webhooks, z. B. „MarketoProgramSuccess“, und klicken Sie auf die Schaltfläche &quot;Trigger erstellen“.  Jetzt ist es an der Zeit, den Aktionsdienst anzugeben, indem Sie auf den Abschnitt **+that** klicken.  Wir beginnen mit einem einfachen Aktionsdienst, den jeder testen kann, ohne in IoT-Geräte investieren zu müssen: den Benachrichtigungsdienst. Suchen Sie nach dem Benachrichtigungsdienst und wählen Sie ihn aus.
Wählen Sie die Aktion „Benachrichtigung senden“, die eine Benachrichtigung an Ihre Geräte sendet.  Sie können die drei von Marketo gesendeten Werte nutzen, indem Sie sie als Inhaltsstoffe hinzufügen, um eine aussagekräftige Benachrichtigung an die Benutzenden zu senden, wie im folgenden Beispiel … und dann auf die Schaltfläche „Aktion erstellen“ klicken. Überprüfen und beenden Sie das IFTTT-Applet. Stellen Sie sicher, dass sie aktiviert ist.

### Testen des IFTTT-Applets

Wenn Sie auf Ihrem Mobilgerät benachrichtigt werden möchten, müssen Sie zunächst die IFTTT-App für Ihr Gerät herunterladen.  Sie können einen Trigger zum Erfolgsereignis eines Marketo-Programms erstellen, indem Sie den Webhook in einem Marketo Smart Campaign-Fluss verwenden. Beachten Sie, dass Marketo-Webhooks ausschließlich mit ausgelösten Smart-Kampagnen funktionieren (z. B. Trigger, nachdem ein ausgefülltes Kontaktformular zu einer Liste hinzugefügt wurde usw.).  Und hier ist ein Beispiel für eine IFTT-Benachrichtigung auf Ihrem Mobiltelefon.

### Creative mit IFTTT

IFTTT bietet Applet-Aktionen mit über 300 Partnern, sodass Ihr Portfolio an Apps und Geräten zusammen mit Ihrer Vorstellungskraft die Grenzen sind … Nehmen wir ein Beispiel mit den [Hue-Leuchten von Philips](https://www.philips-hue.com/en-us), die Sie überall in Elektronikgeschäften oder online kaufen können. Die folgenden Applets würden eine Ihrer Lampen mit der aktuell zugewiesenen Farbe blinken lassen, wenn Marketo-Trigger einen Programmerfolg verzeichnen, der Ihr Marketing-Team im Büro stärken könnte. Wir erstellen ein neues Applet, indem wir den gleichen Schritten folgen wie zuvor, wobei Marketo mit einem Webhook ausgelöst wird, aber dieses Mal wählen wir die Aktion aus dem Philips Hue-Service aus.

Wählen wir die Aktion „Blinklicht“ aus. Die App fordert von Philips Hue alle verfügbaren Lichter an, damit Sie die auswählen können, die blinken soll. Sie müssen zuerst ein Konto bei Philips Hue, der Hue-Brücke und natürlich mindestens eine Hue-Glühbirne, Lichtleiste, Projektor oder Lampe einrichten.  Wir haben gerade ein neues Applet hinzugefügt, das jedes Mal, wenn ein Lead zu einer Roadshow oder einem Webinar registriert wird, ein farbiges Licht blinkt. Ihr Marketing-Team wird jeden Tag mit diesem Setup im Büro aufmuntern.

### Ausführen einer Marketo-Aktion vom IFTTT über Zapie

Jetzt werden wir den Trigger einer Marketo Smart Campaign über die IFTTT-Plattform starten. Dazu verwenden wir die [Marketo-REST-API](/help/rest-api/rest-api.md). Da diese API gesichert ist und eine OAuth2-Authentifizierung erfordert, bevor etwas aufgerufen wird, müssen wir diese Authentifizierung über eine andere Plattform wie Zapier durchführen, da IFTTT nicht zulässt, dass zwei aufeinander folgende Aufrufe einer API mit dem Maker-Kanal verkettet werden. Wir haben uns für [Zapier](https://zapier.com/) Web App Automation Service entschieden, da wir bereits die Einführung von Zapier veröffentlicht haben und Schritt für Schritt erklären, wie ein benutzerdefinierter Marketo-Connector für Zapier implementiert wird. Andere Plattformen wie [Workato](https://www.workato.com/integrations/marketo) könnten ebenfalls eine Lösung sein.

### Marketo Campaign

Erstellen Sie Ihr Marketo-Programm mit einer geplanten Smart-Kampagne. Zu Testzwecken können Sie beispielsweise die folgende Smart-Kampagne erstellen: **Smart-Liste** Verwenden Sie nur Filter, keine Trigger. Stellen Sie sicher, dass Sie sich zumindest qualifizieren. **Fluss** Senden Sie eine E-Mail oder eine andere Art von Benachrichtigung. **Zeitplan** Stellen Sie sicher, dass Sie jedes Mal den Fluss durchlaufen können, um mehrere Tests durchzuführen. Sie können die Smart Campaign-ID über die URL abrufen. Beispiel: _https://{{marketo_url}}/#SC4289A1_ - Die Smart-Kampagnen-ID lautet 4289. Der Trigger dieser Kampagne kann über die Marketo REST-API durchgeführt werden. Sie können beispielsweise das Plug-in [Postman](https://www.postman.com/) für Chrome verwenden und die beiden folgenden aufeinander folgenden HTTPS-Aufrufe senden: **Authentifizierungsschritt:**

`https://{{Your Munchkin_Account_id}}.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id={{Your_Client_Id}}&client_secret={{Your_Client_Secret}}`

Rufen Sie das Zugriffstoken aus der JSON-Antwort ab. **Kampagnenstart:**

`https://{{Your_Munchkin_Account_id}}.mktorest.com/rest/v1/campaigns/{{Campaign_Id}}/schedule.json?access_token={{access_token}}`

### Benutzerdefinierter Zwischenkonnektor von Zapier zum Starten der Marketo-Kampagne

Wir müssen einen benutzerdefinierten Zapier-Connector erstellen, der sich bei der Marketo REST-API authentifiziert und unsere Smart-Kampagne startet.

* Voraussetzungen
* Implementierung des Marketo Connectors für Zapier
* Anderen Titel verwenden, z. B. &quot;Marketo Campaign“
   * Führen Sie den Schritt „Authentifizierung“ aus
   * Führen Sie den Schritt &quot;Trigger&quot; aus (erforderlich für den Zapier-Testzweck).
   * Führen Sie den folgenden spezifischen Schritt „Aktionen“ aus, der für den Start einer Marketo-Kampagne verantwortlich ist:

Senden der Endpunkt-URL für die Datenaktion :

`https://{{munchkin_account_id}}.mktorest.com/rest/v1/campaigns/{{CampaignId}}/schedule.json`

Lassen Sie die anderen optionalen Felder leer.

#### Skript-API

Mit der Skripterstellungsfunktion von Zapier können Sie die Anfragen und Antworten bearbeiten, die zwischen der API Ihrer App und Zapier ausgetauscht werden. Sie können HTTP-Anfragen ändern, bevor sie gesendet werden, und Antworten analysieren, bevor Zapier etwas mit ihnen macht. Wir benötigen sie, um unsere benutzerdefinierte „Sitzungsauthentifizierung“ abzuschließen. Weitere Informationen finden Sie im ursprünglichen Artikel. Kopieren Sie den folgenden Code, der dem Original sehr ähnlich ist, wir haben gerade die Aktionsmethoden geändert:

```javascript
var Zap = {

 get_session_info: function(bundle) {

 console.log('Entering get_session_info method ...');

 var access_token,
 access_token_request_payload,
 access_token_response;


 // Assemble the meta data for our Access Token swap request
 console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
 access_token_request_payload = {
 method: 'POST',
 url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
 params: {
 'grant_type' : 'client_credentials',
 'client_id' : bundle.auth_fields.client_id,
 'client_secret' : bundle.auth_fields.client_secret
 },
 headers: {
 'Content-Type': 'application/json', // Could be anything.
 Accept: 'application/json'
 }
 };

 // Fire off the Access Token request.
 access_token_response = z.request(access_token_request_payload);

 // Extract the Access Token from returned JSON.
 access_token = JSON.parse(access_token_response.content).access_token;
 console.log('New Access_Token=' + access_token);

 // This will be mixed into bundle.auth_fields in future calls.
 //bundle.auth_fields.access_token=access_token;
 return {'access_token': access_token};
 },

 test_trigger_pre_poll: function(bundle) {

 console.log('Entering test_trigger_pre_poll method ...');

 bundle.request.params = {
 'access_token':bundle.auth_fields.access_token
 };

 return bundle.request;

 },

 test_trigger_post_poll: function(bundle) {

 console.log('Entering test_trigger_post_poll method ...');

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }

 return JSON.parse(bundle.response.content);
 },

 launch_campaign_pre_write: function(bundle) {

 bundle.request.params = {'access_token':bundle.auth_fields.access_token};
 return bundle.request;
 },

 launch_campaign_post_write: function(bundle) {

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }
 return JSON.parse(bundle.response.content);
 }

};
```

##### Neue ZAP

Klicken Sie im Zapier-Dashboard auf die Schaltfläche „Neuen Zap erstellen“. **Auslöser**

* Trigger-App „Webhooks von Zapier“ auswählen
* „Fanghaken“ markieren
* Kein Grund, einen untergeordneten Schlüssel auszuwählen
* Zapier hat eine benutzerdefinierte Webhook-URL generiert, an die Sie Anfragen senden können, um sie an einem sicheren Ort zu speichern
* Testen Sie die Webhook-URL, indem Sie das unten stehende Szenario „IFTTT Applet, das den Zapier-Webhook aufruft“ starten. Dadurch kann Zapier mehr über die Webhook-Payload erfahren und die Kampagnen-ID der Aktion zuweisen

**Aktion**

* Wählen Sie den zuvor erstellten Marketo Campaign-Connector aus
* Wählen Sie die einzige verfügbare Aktion aus: **Kampagne starten**
* Stellen Sie eine Verbindung zu Ihrem Marketo-Konto her und füllen Sie die Authentifizierungsparameter (Munchkin-Konto-ID, Client-ID, Client-Geheimnis) aus
* Bearbeiten Sie die Vorlage und verknüpfen Sie die Kampagnen-ID aus dem Trigger mit dem Kampagnen-ID-Parameter „Kampagne starten“
* Testen Sie den Schritt und überprüfen Sie, ob die Marketo-Kampagne gestartet wird.

### IFTTT-Applet, das den Zapier Webhook aufruft

Wir beginnen mit einem einfachen Szenario, das einfach zu testen ist. Wir wählen in IFTTT einen Datums- und Uhrzeitkampagnen-Trigger aus, der jede Stunde die Marketo-Kampagne startet. Die Aktion ist eine Web-Anfrage, die an die Zapier Webhook-URL gesendet und über die Smart Campaign ID übergeben wird. Stellen Sie sicher, dass Zapier Zap und das IFTTT Applet beide aktiv sind und testen Sie, ob alles wie erwartet funktioniert.

### Creative mit IFTTT

IFTTT bietet Applet-Trigger mit über 300 Partnern, sodass Ihr Portfolio an Apps und Appliances gemeinsam mit Ihrer Vorstellungskraft die Grenzen überschreitet … Nehmen wir ein Beispiel mit dem [Weather Underground](https://www.wunderground.com/)-Service, den wir verwenden werden, um unsere Marketo-Kampagne auf Wetterwarnung zu starten. Der folgende Trigger würde ausgelöst, wenn ein Regenzustand bekannt gegeben wird. Verknüpfen Sie dann den Trigger mit der Maker-Webhook-Aktion und füllen Sie wie zuvor die Zapier-Webhook-Parameter aus.  Et voila, man braucht jetzt nur einen guten Regen, um zu überprüfen, ob es tatsächlich funktioniert.

Wir hoffen, Sie haben viel Spaß bei der Anwendung der Konzepte in diesem Artikel. Vor allem aber denken wir, dass es dank der Schlüsselkonzepte dieses Artikels allen helfen wird, die Marketo mit anderen Drittanbietersystemen integrieren möchten:

* Marketo REST-API
* Marketo-Webhooks
* Nutzung einer offenen Web-Integrationsplattform wie Zapier als „Serving Hatch“ zwischen einem Drittanbietersystem und Marketo, um beispielsweise die Authentifizierung zu verwalten

Veröffentlicht am _2017-06-20_ von _Philippe_

## Updates Sommer 2017

In der Version vom Sommer 2017 veröffentlichen wir einige kleinere Verbesserungen an unseren Programm-APIs.

### Programme durchsuchen

Wir fügen unserem Endpunkt [Programme abrufen“ die Möglichkeit hinzu, Programme nach Datumsbereich abzurufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/browseProgramsUsingGET) indem die optionalen Parameter „frühestUpdatedAt“ und „neuesteUpdatedAt“ hinzugefügt werden. Sie können einen oder beide Parameter mit dem Datum/Uhrzeit-Wert Ihrer Wahl festlegen, um nur Programme zurückzugeben, die zwischen den beiden Datum/Uhrzeit-Werten erstellt oder aktualisiert wurden. Dies ist nützlich für den Abruf neuer und aktualisierter Marketingmaterialien, insbesondere für Anwendungsfälle der Übersetzung und Business Intelligence.

Veröffentlicht am _1970-01-01_ von _Kenny_

## Erweitern der Marketo-Geschäftslogik mit Google Cloud-Funktionen

In diesem Artikel wird eine Lösung vorgeschlagen, um Marketo mit einigen Geschäftslogikfunktionen mit Google Cloud Platform (GCP) zu erweitern, basierend auf dem folgenden einfachen Beispiel: 3 benutzerdefinierte Felder im Marketo-Lead-Datensatz:

* **OnLinePreference**: Ein inkrementeller Wert, der die Anwesenheit eines Interessenten/Kunden bei der Online-Kommunikation angibt.
* **OfflinePreference**: Ein inkrementeller Wert, der die Anwesenheit eines Interessenten/Kunden bei der Offline-Kommunikation angibt.
* **Präferenz**: Ein vom GCP berechnetes Feld, das „offline“ anzeigt, wenn die Offline-Bewertung höher als die Online-Bewertung ist, und „online“ umgekehrt

Diese Technologie ebnet den Weg für eine fortschrittlichere Geschäftslogik und schließlich für das Aufrufen externer Web-Services, die Transformation und Konsolidierung der Ergebnisse in Marketo.  

### Über Google Cloud Platform und Funktionen

[Google Cloud Platform](https://cloud.google.com/) (GCP) ist eine Suite von Cloud-Computing-Services, die auf derselben Infrastruktur ausgeführt wird, die Google intern für seine Endbenutzerprodukte verwendet, z. B. Google Search und YouTube. Neben einer Reihe von Management-Tools bietet es eine Reihe modularer Cloud-Services, einschließlich Computing, Datenspeicherung, Datenanalyse, maschinelles Lernen, Big Data und vieles mehr. Wir hätten für unseren Bedarf viele verschiedene GCP-Services nutzen können, wie Compute Engine, App Engine oder Kubernetes Engine, aber wir haben uns für die [Cloud-Funktionen](https://cloud.google.com/functions) (noch in Beta) für die folgenden Hauptvorteile entschieden:

* Beim Server-losen Cloud-Computing kann Logik bei Bedarf als Reaktion auf Ereignisse wie HTTP-Aufrufe erzeugt werden.
* Löst die meisten Probleme, die durch Serverwartung und -bereitstellungen verursacht werden.
* Kostengünstig, da Sie GCP nur für jeden Funktionsaufruf und nicht für die Aufrechterhaltung des Betriebs eines Servers bezahlen.
* Einfach und schnell zu implementieren, da Sie sich nur auf Ihre Anwendungslogik konzentrieren.
* Automatische Skalierung, bereit für sehr hohe Arbeitslasten.

Weitere Informationen zu dieser Technologie und [ Preis finden Sie ](https://cloud.google.com/) der GCP-Website. Normalerweise sollte dieses Tutorial keine wichtigen Kosten verursachen und passt perfekt in die kostenlose Gutschrift einer GCP-Testversion.  

### Vorbereitung der Google Cloud-Umgebung

Sie benötigen ein Google Cloud-Konto. Sie können GCP kostenlos mit einem Guthaben ausprobieren, das mehr als genug ist, um dieses Tutorial auszuführen, klicken Sie einfach auf die Schaltfläche „Probieren Sie es kostenlos“ auf der [GCP-Website](https://cloud.google.com/). Führen Sie alle Schritte aus dem Abschnitt „Bevor Sie beginnen“ im [HTTP-Tutorial](https://cloud.google.com/functions/docs/calling) von Google aus:

1. Erstellen eines Cloud Platform-Projekts: GEHEN SIE ZUR SEITE RESSOURCEN VERWALTEN .
1. Abrechnung für Ihr Projekt aktivieren: [ABRECHNUNG AKTIVIEREN](https://cloud.google.com/billing/docs/how-to/modify-project?visit_id=638816637273392093-1926929734&rd=1)
1. Aktivieren der Cloud-Funktionen-API: [AKTIVIEREN DER API](https://accounts.google.com/InteractiveLogin?continue=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&followup=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&osid=1&passive=1209600&service=cloudconsole&ifkv=ASKV5Mh81NGNsqcJqhx7hst0KFnyA0MJ-2zay8ovyluBfpvDoM820nF9Wq_SKbC1m_sjQvvRNoKSuA)
1. [Installieren und Initialisieren von Cloud SDK](https://cloud.google.com/sdk/docs/)
1. Aktualisieren und Installieren von Gcloud-Komponenten

   **gCloud-Komponenten aktualisieren und&amp;** **gCloud-Komponenten installieren die Beta-Version**

1. Ihre Umgebung für die Entwicklung von Node.js vorbereiten: [WECHSELN SIE ZUM SETUP-HANDBUCH](https://cloud.google.com/nodejs/docs/setup)

### Implementierung der ScoreCompare Cloud-Funktion

1. Erstellen Sie einen Cloud-Speicher-Bucket, um Ihre Cloud-Funktionsdateien zu staging. Sie können dies über die Befehlszeile tun:

   `gsutil mb gs://[YOUR_STAGING_BUCKET_NAME]`

   oder über die Google Cloud-Web-Oberfläche, indem Sie Ihr Projekt auswählen und auf das Speichermenü klicken:
   * Geben Sie Ihrem Speicher-Bucket einen eindeutigen Namen
   * Standard-Speicherklasse auswählen
   * Wählen Sie die am besten geeignete regionale Lage

1. Erstellen Sie auf Ihrem lokalen System ein Verzeichnis für den Anwendungs-Code.
1. Erstellen Sie in diesem Verzeichnis eine „index.js“-Datei mit folgendem JavaScript-Code: Der Code ist sehr einfach zu verstehen. Er analysiert die beiden Eingabeparameter aus dem HTTP-Anfragetext in JSON, führt die Verarbeitung durch und codiert die HTTP-Antwort in JSON.

```javascript
 /\*\*
     \* HTTP scoreCompare Cloud Function.
     \*
     \* @param {Object} req Cloud Function request context.
     \* @param {Object} res Cloud Function response context.
     \*/
    exports.scoreCompare = function scoreCompare (req, res) {
     var onlineScore=parseInt(req.body.onlineScore);
     var offlineScore=parseInt(req.body.offlineScore);
     console.log('/scoreCompare: got values onlineScore =' + onlineScore + ', offlineScore =' + offlineScore);
     var result;
     if (onlineScore>offlineScore) {result = 'online';} else {result = 'offline';}
     console.log('/scoreCompare: and result is ' + result);
     res.status(200).json({output: result}).end();
    };
```

Stellen Sie die Funktion scoreCompare mit einem HTTP-Trigger bereit. Führen Sie den folgenden Befehl in Ihrem Verzeichnis aus:

**gcloud Beta-Funktionen bereitstellen [FUNCTION] —stage-bucket [YOUR_STAGING_BUCKET_NAME] —Trigger-http**

Wobei [IHR_STAGING_BUCKET_NAME] der Name Ihres Staging-Cloud-Speicher-Buckets ist. In unserem Beispiel:

**gcloud Beta-Funktionen implementieren scoreCompare —stage-bucket mktostorage —Trigger-http**

1. Beachten Sie die Cloud-Funktions-URL (httpsTrigger-URL) aus der Konsolenausgabe, die wie folgt aussieht: `https://[YOUR_REGION]-[YOUR_PROJECT_ID].cloudfunctions.net/[FUNCTION]` wobei

   * [IHRE_REGION] ist die Region, in der Ihre Funktion bereitgestellt wird. Dies ist in Ihrem Terminal sichtbar, wenn die Bereitstellung Ihrer Funktion abgeschlossen ist.
   * [IHR_PROJEKT_ID] ist Ihre Cloud-Projekt-ID. Dies ist in Ihrem Terminal sichtbar, wenn die Bereitstellung Ihrer Funktion abgeschlossen ist.
   * [FUNCTION] ist Ihr Funktionsname.

   In unserem Beispiel: [**https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare**](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
1. Testen Sie Ihre Funktion mit einem Tool wie [Postman](https://www.postman.com/):
   * HTTP-Verb: POST
   * URL: [https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
   * Kopfzeilen: Content-Typ = application/json
   * Hauptteil: {„onlineScore“:110, „offlineScore“:200}Die Ausgabe sollte geben: {„output“: „offline“}.

### Rufen Sie die Cloud-Funktion über einen Webhook von Marketo auf

Die drei folgenden benutzerdefinierten Felder müssen für den Lead-Datensatz in Marketo erstellt werden:

* **OnlinePreference**: Integer
* **OfflinePreference**: Integer
* **preferences**: string

Erstellen Sie mithilfe Ihrer „scoreCompare“-Cloud-Funktions-URL und der Token des benutzerdefinierten Feldes den folgenden Webhook aus der Marketo-Admin-Oberfläche. Testen Sie den Webhook mit einer von Marketo ausgelösten Smart-Kampagne.

* **Marketo-Webhooks können nur von ausgelösten Smart-Kampagnen aufgerufen werden, nicht von Batch-Smart-Kampagnen.**
* **Wenn Sie Ihre Cloud-Funktion nicht verwenden, löschen Sie sie oder löschen Sie das gesamte Projekt, um Gebühren für Ihr Google Cloud Platform-Konto zu vermeiden.**

Wir hoffen, dass dieses Tutorial Ihre Zeit wert war und Sie über komplexere Szenarien mit komplexer Verarbeitung und Services von Drittanbietern nachdenken lässt. Ein gutes Beispiel wäre die Nutzung von Google Cloud AI, den maschinellen Lerndiensten von Google. Sie können beispielsweise einen freien Text aus einem Marketo-Formular analysieren und die Google Natural Language API bitten, die Struktur und Bedeutung des Textes zu enthüllen und dann diese Analyse in Marketo zurückzuspeichern; öffnen Sie einfach die Schleusentore für Ideen.

Veröffentlicht am _21.11.2017_ von _Philippe_

## Updates vom Herbst 2017

In der Version vom Herbst 2017 veröffentlichen wir mehrere Verbesserungen an unseren Asset-APIs. Unten finden Sie eine vollständige Liste der Updates.

### Programme nach Datumsbereich durchsuchen

Wir haben die Möglichkeit hinzugefügt, Programme nach Datumsbereich zu unserem Endpunkt [Programme abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST) zu erhalten. Dies geschieht mithilfe der Parameter `earliestUpdatedAt` und `latestUpdatedAt` . Sie können einen oder beide Parameter mit dem Datum/Uhrzeit-Wert Ihrer Wahl festlegen, um nur Programme zurückzugeben, die zwischen den beiden Datum/Uhrzeit-Werten erstellt oder aktualisiert wurden.

### E-Mail in der Vorschau anzeigen

Sie können jetzt eine E-Mail mit dem Endpunkt [Vollständiger Inhalt der E-Mail abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailFullContentUsingGET), der die serialisierte HTML-Version einer E-Mail zurückgibt, in der Vorschau anzeigen. Alle Token, Snippets, dynamischen Inhalte und eingebetteten Komponenten werden vollständig gerendert. Ein optionaler **leadId**-Parameter kann übergeben werden, um die Identität eines bestimmten Leads einzunehmen.

### HTML von E-Mail 2.0 ersetzen

Wir haben den Endpunkt [Vollständiger Inhalt der E-Mail aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailFullContentUsingPOST) hinzugefügt, damit Sie Blöcke von HTML-E-Mail-Inhalten ersetzen können. Wenn Sie den HTML-Code einer Marketo-E-Mail mit dem Marketo-E-Mail-2.0-Editor bearbeiten, funktioniert die Beziehung zwischen der E-Mail und ihrer Vorlage nicht mehr, mehr dazu [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html). Mit diesem Endpunkt können Sie den HTML-Inhalt einer E-Mail, deren Beziehung unterbrochen wurde, programmgesteuert aktualisieren. Darüber hinaus haben wir alle anderen Endpunkte zum E-Mail-Lebenszyklus so geändert, dass sie mit E-Mails kompatibel sind, bei denen die Beziehung unterbrochen wurde:

* E-Mail-Entwurf genehmigen
* Genehmigung für E-Mail aufheben
* E-Mail löschen
* E-Mail-Entwurf verwerfen
* E-Mail klonen
* E-Mail-Metadaten aktualisieren

### Weitere Verbesserungen

* Die maximale Zeitspanne für Datumsbereichsfilter wurde auf 31 Tage erhöht. Dies bezieht sich auf [Filter für die Lead-Massenextraktion](/help/rest-api/bulk-lead-extract.md) (createdAd oder updatedAt) und [Filter für die Massenaktivität-Extraktion](/help/rest-api/bulk-activity-extract.md) (createdAt).

Veröffentlicht am _2017-12-15_ von _David_

## TEST - Externer Video-Link auf der Community

[Tutorial-Video zum E-Mail-Zustellbarkeits-Power Pack](https://nation.marketo.com:443/t5/product-space-archive-videos/email-deliverability-power-pack-tutorial-video/m-p/283550)  

Veröffentlicht am _1970-01-01_ von _David_

## Updates Winter 2018

In der Version Winter 2018 veröffentlichen wir einige Verbesserungen an unseren APIs. Unten finden Sie eine vollständige Liste der Updates.

### Trigger-Kampagnen aktivieren/deaktivieren

Wir haben die Möglichkeit hinzugefügt, Trigger-Kampagnen zu aktivieren und zu deaktivieren, was den Prozess der Automatisierung Ihrer Programmvorlagen vereinfachen kann. Dies wird durch Aufrufen von zwei neu hinzugefügten Endpunkten erreicht: [Smart-Kampagne aktivieren](https://developer.adobe.com/marketo-apis/api/asset/#operation/activateSmartCampaignUsingPOST), [Smart-Kampagne deaktivieren](https://developer.adobe.com/marketo-apis/api/asset/#operation/deactivateSmartCampaignUsingPOST). Weitere Informationen finden Sie im Abschnitt &quot;Trigger [ in der ](/help/rest-api/assets.md).

### Programm nach Namen abrufen

Dem Endpunkt [Programm nach Name abrufen“ wurden zwei Parameter hinzugefügt](https://developer.adobe.com/marketo-apis/api/asset/#operation/getProgramByNameUsingGET) um das Abrufen von Programmkosten und Programm-Tags zu erleichtern. Weitere Informationen finden Sie unter **includeCosts** und **includeTags**-Parameter in der [Programme](/help/rest-api/assets.md)-Dokumentation.

### Weitere Verbesserungen

Die Bulk Extract-API ist jetzt „Workspace-fähig“. Wenn Sie einen Benutzer „Nur API“ für einen [benutzerdefinierten Service](/help/rest-api/custom-services.md) erstellen, müssen Sie eine Benutzerrolle mit API-Zugriff für einen oder mehrere Arbeitsbereiche auswählen. Zuvor wurde dem benutzerdefinierten Service Zugriff auf alle Arbeitsbereiche gewährt. Jetzt erhält der benutzerdefinierte Service nur noch Zugriff auf die Arbeitsbereiche, die bei der Erstellung des Benutzers mit nur einer API ausgewählt wurden.

Veröffentlicht am _2018-03-02_ von _David_

## Updates Frühjahr 2018

In der Version vom Frühjahr 2018 veröffentlichen wir neue REST-APIs und Verbesserungen beim Webtracking-Datenschutz. Unten finden Sie eine vollständige Liste der Updates.

REST-API

### Statische CRUD-Liste

Ermöglicht Benutzern das Remote-Erstellen, Lesen, Aktualisieren und Löschen von statischen Listeneinträgen. Ermöglicht die Verwaltung des gesamten Lebenszyklus einer statischen Liste über REST-APIs, einschließlich der Befüllung und Pflege der Mitgliedschaft. Details finden Sie [hier](/help/rest-api/static-lists.md).

### Benutzerdefinierte Aktivitätsmetadaten

Ermöglicht Benutzern die Remote-Erstellung benutzerdefinierter Aktivitätsdatensätze. Ermöglicht die Verwaltung des gesamten Lebenszyklus benutzerdefinierter Aktivitäten durch REST-APIs, einschließlich der Bearbeitung von Typen und Typattributen. Details finden Sie [hier](/help/rest-api/activities.md).

### Metadaten der Smart-Liste

Ermöglicht Benutzern das Remote-Lesen, Klonen und Löschen von Smart List-Datensätzen. Ermöglicht die Verwaltung von Smart Lists über REST-APIs. Details finden Sie [hier](/help/rest-api/smart-lists.md).

### Web-Tracking-Datenschutz

Der Munchkin JavaScript-Webtracking-Code wurde erweitert und enthält jetzt die folgenden datenschutzbezogenen Verbesserungen:

* Opt-out - Möglichkeit für Besuchende, sich dauerhaft vom Webtracking abzumelden. Details finden Sie [hier](/help/javascript-api/lead-tracking.md).
* Anonymisierung von IP-Adressen - Einhaltung lokaler und internationaler Datenschutzbestimmungen durch die Möglichkeit, die IP-Adressen von Web-Besuchern zu anonymisieren. Siehe **anonymizeIP** Konfigurationsparameter [hier](/help/javascript-api/configuration.md).

### Weitere Verbesserungen

* Der Endpunkt [Ordner abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getFolderUsingGET) gibt jetzt alle Ordner zurück, wenn ein übergeordnetes Element, das kein Stamm ist, und maxDepth=1 angegeben sind.
* Der Endpunkt [Einstiegsseite nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getLandingPageByIdUsingGET) gibt jetzt in allen Fällen URL-Attribute mit dem Protokoll zurück (http:// oder https://).

Veröffentlicht am _2018-06-29_ von _David_

## Updates Sommer 2018

Die Version Sommer 2018 ist in erster Linie eine Wartungsversion, die kleinere Verbesserungen und Fehlerbehebungen enthält. Unten finden Sie eine vollständige Liste der Updates.

### REST-API

* Es wurde Unterstützung für E-Mail-Dispositionsfelder hinzugefügt, die ursprünglich unnötig ausgelassen wurden. Diese Felder stehen nun je nach Bedarf zum Lesen und Schreiben über REST zur Verfügung.
   * Auf der schwarzen Liste
   * Marketing eingestellt
   * E-Mail angehalten
   * Relative Dringlichkeit
* Der Endpunkt [Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/lead-database-endpoint-reference/#/Leads/getLeadsByFilterUsingGET) unterstützt jetzt leadPartionId als filterType.

### Fehlerbehebungen

**REST-Endpunkt**

**Beschreibung**

[Programm genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveProgramUsingPOST)

Wenn Sie bei der Erstellung des Programms Nicht-operative E-Mails blockieren auf „false“ gesetzt hätten, würde ein Aufruf von „Programm genehmigen“ auf „true“ zurückgesetzt.

[Massenextrakt](/help/rest-api/bulk-extract.md)

Bestimmte Unicode-Zeichen in der Extraktionsausgabedatei waren beschädigt.

[Programm klonen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

* Wenn Sie das E-Mail-Programm geklont haben, wurde die SmartList-Filterlogik im resultierenden Programm unabhängig von der ursprünglichen Einstellung auf „Alle“ zurückgesetzt.
* Wenn Sie versucht haben, ein Programm zu klonen, das eine statische Liste enthielt (die gelöscht wurde), dann haben Sie einen 709-Fehler erhalten: „Die folgenden Assets werden nicht :List.“
* Wenn Sie versucht haben, ein Programm über Arbeitsbereiche hinweg zu klonen, erhalten Sie die Fehlermeldung 611, Programm kann nicht geklont werden.

[Statische Liste nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getStaticListByIdUsingGET)

Wenn Ihr benutzerdefinierter Dienst über die Berechtigung „Schreibgeschütztes Asset“ verfügt, erhalten Sie den 603-Fehler „Zugriff verweigert“.

[Lead an Marketo pushen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/pushToMarketoUsingPOST)

Wenn **cookies**-Attribut in einem Lead-Objekt **Eingabe**-Array angegeben wurde, war die vorherige anonyme Aktivität nicht mit dem neu erstellten Lead verknüpft.

[Kampagne planen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST)

Wenn Sie ein **runAt**-Datum weit in der Zukunft angegeben haben, wurde die Kampagne nie ausgeführt und **success=true** wurde zurückgegeben. Wenn das **runAt**-Datum länger als zwei Jahre in der Zukunft liegt, wird ein Fehler zurückgegeben [1042](/help/rest-api/error-codes.md).

[Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)

Wenn Sie `action=createDuplicate` und einen `externalCompanyId` angegeben haben (um den neuen Lead einer bestehenden Firma zuzuordnen), wurde der Lead einer leeren Firma zugeordnet (anstelle der angegebenen Firma).

#### Sonstiges

* Der folgende Datenänderungswert wurde aus allen Endpunktantworten entfernt: mktoClientReqId. Dies war nur für den internen Gebrauch bestimmt.
* Es wurde eine Funktion zur Fehlersuche hinzugefügt. Geben Sie einen REST-API-Fehler-Code in das Suchfeld ein und wählen Sie unten in der Liste für die automatische Vervollständigung einen aus, um zur Fehlerbeschreibung zu springen.
* Seite [Endpunkt-Referenz](/help/rest-api/endpoint-reference.md) hinzugefügt. Dies ist eine sortierbare Liste aller REST-API-Endpunkte an einem Ort. Sie können diese Seite auch verwenden, um den minimalen Berechtigungssatz zu generieren, der von Ihrer Anwendung benötigt wird. Dies ist nützlich, wenn Sie einen benutzerdefinierten Service erstellen.

Veröffentlicht am _2018-10-12_ von _David_

## Updates vom Herbst 2018

Die Herbstversion 2019 ist in erster Linie eine Wartungsversion, die kleinere Verbesserungen und Fehlerbehebungen enthält. Unten finden Sie eine vollständige Liste der Updates.

### Verbesserungen

* Hinzugefügte [E-Mail-CC](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/email-cc)Felder: Unterstützung für [Asset-API](/help/rest-api/assets.md). Die CC-Feldeinstellungen werden bei Validierungs-/Klonvorgängen (Entwurfsvalidierung per E-Mail oder E-Mail, E-Mail oder Klonen eines Programms) wie erwartet weitergegeben. Alle Endpunkte, die mit E-Mails in Verbindung stehen, geben jetzt CC-Feldwerte in der Eigenschaft **ccFields** zurück. Scrollen Sie in der unten stehenden Antwort nach unten, um ein Beispiel zu sehen. Diese Änderung betrifft die folgenden Endpunkte: [E-Mail abrufen nach ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByIdUsingGET), [E-Mail abrufen nach Name](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByNameUsingGET), [E-Mails abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailUsingGET), [E-Mail-Entwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [E-Mail-Vorlagenentwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST_1), [E-Mail klonen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Programm klonen.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

```json
{
    "success": true,
    "errors": [],
    "requestId": "cc96#166e836348b",
    "warnings": [],
    "result": [
        {
            "id": 2061,
            "name": "Test Email",
            "description": "This is a test",
            "createdAt": "2018-11-06T05:27:10Z+0000",
            "updatedAt": "2018-11-06T08:33:16Z+0000",
            "url": "<https://app-sjqe.marketo.com/#EM2061A1LA1>",
            "subject": {
                "type": "Text",
                "value": "This is a test with CC Fields"
            },
            "fromName": {
                "type": "Text",
                "value": "Tommy Tester"
            },
            "fromEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "replyEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "folder": {
                "type": "Program",
                "value": 7494,
                "folderName": "Initiative"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "approved",
            "template": 1218,
            "workspace": "Default",
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
                {
                    "attributeId": "855",
                    "objectName": "lead",
                    "displayName": "emailcc",
                    "apiName": "emailcc"
                },
                {
                    "attributeId": "857",
                    "objectName": "lead",
                    "displayName": "leadDetails",
                    "apiName": "leadDetails"
                },
                {
                    "attributeId": "859",
                    "objectName": "company",
                    "displayName": "headquarters",
                    "apiName": "headquarters"
                }
            ]
        }
    ]
}
```

### Fehlerbehebungen

* Die Unterstützung [mehrere Branding-Domains](https://experienceleague.adobe.com/de/docs/marketo/using/home) für [Asset-API](/help/rest-api/assets.md) wurde angepasst. Zuvor wurden die Einstellungen für mehrere Branding-Domains bei der Genehmigung eines E-Mail-Entwurfs, beim Klonen einer E-Mail oder beim Klonen eines Programms nicht übernommen. Dies wurde korrigiert. Diese Änderung betrifft die folgenden Endpunkte: [E-Mail-Entwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [E-Mail klonen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Programm klonen.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)
* Konfigurationseinstellung [apiOnly](/help/javascript-api/configuration.md) hinzugefügt. Standardmäßig lösen Web-Seiten, die das Munchkin-Tag enthalten, das Ereignis „Besuche auf Web-Seiten“ aus, wenn die Web-Seite im Browser geladen wird. In einigen Fällen ist dies unerwünscht. Zum Beispiel Web-Anwendungen für einzelne Seiten, die volle Kontrolle darüber benötigen, wann dieses Ereignis ausgelöst wird. Um diesen Anwendungsfall zu unterstützen, haben wir eine neue Konfigurationseinstellung **apiOnly** hinzugefügt. Bei Festlegung auf „true“ generiert das Munchkin-Tag beim Laden der Seite keine Aktivität des Typs „Besuche auf Web-Seiten“.
* Konfigurationseinstellung [domainSelectorV2](/help/javascript-api/configuration.md) hinzugefügt. Standardmäßig verarbeitet das Munchkin-Tag Web-Seiten, die auf Websites mit aus zwei Buchstaben bestehenden Domains ([-Code Top-Level-Domains) gehostet werden](https://en.wikipedia.org/wiki/Country_code_top-level_domain) (Beispiele: .io, .co, .ly) nicht korrekt. Dies führt dazu, dass das Munchkin-Cookie-Domänenattribut falsch festgelegt wird. Um ein besseres vorkonfiguriertes Erlebnis zu erzielen, haben wir eine neue Konfigurationseinstellung **domainSelectorV2** hinzugefügt. Bei „true“ wird ein verbesserter Algorithmus verwendet, um das Munchkin-Cookie-Domain-Attribut automatisch festzulegen.
* Angepasste [Opt-out](/help/javascript-api/lead-tracking.md)-Cookie-Domain. In bestimmten Fällen wurde das Domain-Attribut des Munchkin-Opt-out-Cookies (mkto_opt_out) falsch gesetzt. Das Munchkin-Opt-out-Cookie verwendet jetzt dieselbe Logik wie das Munchkin-Cookie (_mkto_trk), um das Domain-Cookie-Attribut zu ermitteln, einschließlich der Berücksichtigung der Konfigurationseinstellung **domainLevel**.
* Entwickler von Android-Anwendungen können jetzt Googles [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) direkt mit dieser SDK verwenden. Details finden Sie [hier](/help/mobile/installation.md).

Veröffentlicht am _2018-12-07_ von _David_

## Updates Frühjahr 2019

Die Version vom Frühjahr 2019 ist in erster Linie eine Wartungsversion, die kleinere Verbesserungen und Fehlerbehebungen enthält. Unten finden Sie eine vollständige Liste der Updates.

Veröffentlicht am _2019-03-19_ von _David_

## Updates vom Juni 2019

Die Version vom Juni 2019 ist in erster Linie eine Wartungsversion, die geringfügige Verbesserungen und Fehlerbehebungen enthält. Unten finden Sie eine vollständige Liste der Updates.

### REST-API

1. Es wurde eine Prüfsumme zu Status-Endpunkten für den Massenexport hinzugefügt. Sie können die Prüfsumme mit einem Hash der abgerufenen Datei vergleichen, um die Integrität der abgerufenen Datei zu überprüfen. Die Prüfsumme ist ein SHA-256-Hash der exportierten Datei und wird beim Abschluss eines Exportvorgangs im fileCheckSum-Attribut gespeichert.

Die folgenden Endpunkte geben die Prüfsumme zurück: [Export-Lead-Auftragsstatus abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Export-Lead-Aufträge abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET), [Exportaktivitätsauftragsstatus abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Exportaktivitätsaufträge abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportActivitiesUsingGET)

#### Fehlerbehebungen

1. Fehlerkorrektur - Beim [ von Dezimalzahlen in ganzzahlige Felder tritt jetzt kein Fehler mehr beim Massenimport ](/help/rest-api/bulk-custom-object-import.md) benutzerdefinierten Objekts auf. Vor der Korrektur wurde die Dezimalzahl in eine Ganzzahl umgewandelt, indem der ganze Zahlenteil zugewiesen und der Bruchteil verworfen wurde (z. B. wurde 5.432 in 5 umgewandelt). Jetzt wird der Fehler „Ungültiger Datentyp im Feld Source-ID“ für jede Zeile generiert, die eine Datenabweichung enthält.
1. Es wurde ein Problem behoben, bei dem ein mit dem Endpunkt [Programm klonen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) erstelltes E-Mail-Programm in bestimmten Fällen die Einstellungen für Kommunikationsbeschränkungen nicht berücksichtigte.
1. Es wurde ein Problem mit [ Endpunkt „Entwurf der Landingpage genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) behoben, bei dem 611 zurückgegeben wurde. „Systemfehler“, wenn die Landingpage das E-Mail-Abmeldeformular enthielt.
1. Es wurde ein Problem mit [ Endpunkt „Entwurf der Landingpage genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) behoben, bei dem 611 zurückgegeben wurde. „Systemfehler“, wenn die Einstiegsseite mithilfe des Endpunkts [Einstiegsseite klonen](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneLandingPageUsingPOST) geklont wurde.

#### Einstellungen

1. Im Rahmen der [Einstellung von E-Mail-Editor 1](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666)0 wird Assets 1.0 E-Mail Ende 2019 schreibgeschützt sein. Alle 1.0 E-Mail-Assets müssen in 2.0 als descriDiscard E-Mail-Entwurf, Bett ([) ](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) werden. Um Entwickler bei der Vorbereitung auf dieses Ereignis zu unterstützen, haben wir Warnhinweise zu allen E-Mail-bezogenen Endpunkten hinzugefügt, die versuchen, E-Mail-Assets 1.0 zu ändern. Im Folgenden finden Sie eine Beispielantwort mit der Warnung:

`{` `"success": true,` `"errors": [],` `"requestId": "15c57#16b338d6e75",` `"warnings": [` `"This is a v1 email asset. API support for modifying v1 emails is being dropped, and this operation will not work on v1 emails in the future. To avoid service interruptions, upgrade this and related assets by editing them in the User Interface."` `],` `"result": [` `"{\"service\":\"sendTestEmail\",\"result\":true}"` `]` `}`

Die folgenden E-Mail-bezogenen Endpunkte geben die Warnung zurück:

* Vollständigen Inhalt der E-Mail aktualisieren
* E-Mail-Inhalt aktualisieren
* Beispiel-E-Mail senden
* Genehmigung für E-Mail aufheben
* Programm klonen
* E-Mail klonen
* E-Mail-Entwurf genehmigen
* Abschnitt „Dynamischen Inhalt von E-Mails aktualisieren“
* E-Mail-Metadaten aktualisieren
* Programm genehmigen

E-Mail-Scripting (Apache Velocity)

#### Einstellungen

1. Eine Untergruppe der Velocity Script-Funktionen wurde aus Sicherheitsgründen deaktiviert. Dazu gehören die folgenden Methoden und Variablen: getClass(), $class, $context, $text. Weitere Informationen finden Sie [hier](https://nation.marketo.com:443/t5/knowledgebase/unsupported-velocity-tools-disabled-in-june-2019-release/ta-p/251177).

Veröffentlicht am _2019-06-14_ von _David_

## Updates August 2019

Im August 2019 veröffentlichen wir neue REST-APIs, die bestehende APIs verbessern und Fehler beheben. Unten finden Sie eine vollständige Liste der Updates.

### Verbesserungen

1. Verbesserte Funktionen für den intelligenten Kampagnen-Lebenszyklus. Es wurden neue Endpunkte hinzugefügt, mit denen Sie die folgenden Vorgänge für Smart-Kampagnen durchführen können: Nach Namen abrufen, erstellen, aktualisieren, klonen und löschen. Vollständige Informationen finden Sie [hier](/help/rest-api/smart-campaigns.md).
1. Verbesserte Endpunkte der Smart-Liste, um die Abfragefunktionen zu verbessern.
   1. [ Endpunkt „Smart-Liste nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByIdUsingGET) gibt jetzt Beschreibungen von Smart-Listen-Regeln (Trigger und Filter) zurück, wenn Sie den booleschen **includeRules** übergeben.
   1. [ Endpunkt „Smart](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListsUsingGET)Listen abrufen“ ermöglicht jetzt das Filtern der Ergebnisse nach Datumsbereich beim Übergeben der **&quot;** latestUpdatedAt und **latestUpdatedAt**. Darüber hinaus gibt dieser Endpunkt jetzt Smart Lists zurück, die Mitglieder von Kampagnen und E-Mail-Programmen sind.
1. Es wurden Endpunkte zum Extrahieren von Smart-Listen-Definitionen hinzugefügt.
   1. Endpunkt [Smart-Liste nach Smart-Kampagnen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListBySmartCampaignIdUsingGET) abrufen gibt den Smart-Listen-Datensatz für eine bestimmte Smart-Kampagnen-ID zurück.
   1. Endpunkt [Smart-Liste nach Programm-](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByProgramIdUsingGET) abrufen) gibt den Smart-Listen-Datensatz für eine bestimmte Programm-ID zurück.
1. Der Endpunkt [E-Mail-Inhalt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#operation/updateEmailContentUsingPOST) wurde verbessert, um Aktualisierungen der E-Mail-Header-Felder für E-Mails zu ermöglichen, die aus ihrer Vorlage beschädigt wurden (Betreff, Absendername, Absender-E-Mail, Antwort an). Die aus der Vorlage unterbrochene wird [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html) beschrieben.

### Fehlerbehebungen

1. Es wurde ein Problem behoben[ bei dem durch einen Aufruf von ](https://developer.adobe.com/marketo-apis/api/asset/#operation/deleteLandingPageByIdUsingPOST)Landingpage löschen“ auf einer genehmigten Landingpage die Landingpage gelöscht wurde. Jetzt wird korrekt der Fehler „709, Genehmigte Landingpage kann nicht gelöscht werden“ zurückgegeben. [LM-127271]
1. Es wurde ein Problem mit dem Endpunkt [Beispiel-E-Mail senden](https://developer.adobe.com/marketo-apis/api/asset/#operation/sendSampleEmailUsingPOST) behoben, bei dem 611 zurückgegeben wurde. „Systemfehler“, wenn die E-Mail aus der Vorlage beschädigt wurde. [LM-127288]
1. Es wurde ein Problem mit [ Endpunkt ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)Programm löschen“ behoben, bei dem in einigen Fällen ein in Verwendung befindliches Programm gelöscht wurde, anstatt die Meldung „709, Programm kann nicht gelöscht werden“ auszugeben. Die Assets werden an anderer Stelle verwendet oder sind nicht löschbar.“ [LM-125431]

### Einstellungen

1. Die API-Unterstützung für E-Mail-Editor 1.0 wird voraussichtlich im Januar 2020 eingestellt. Denken Sie daran, Ihre Assets zuvor in 2.0 zu konvertieren. Der Versuch, nach Januar E-Mail 1.0-Assets zu schreiben oder zu klonen, führt zu Fehlern anstelle von Warnungen. Weitere Informationen zu E-Mail-APIs [hier](https://nation.marketo.com:443/t5/knowledgebase/email-2-0-and-email-api-faq-s/ta-p/251423).
1. Um sich an Adobes erstklassigen Sicherheitsstandard anzupassen, werden wir die Unterstützung für Transport Layer Security (TLS) 1.0 und 1.1 ab dem 13. Dezember 2019 einstellen. Bei Systemen, die mit Marketo integriert werden und nicht dem 1.2-Protokoll entsprechen, kann der Zugriff auf Marketo Engage-Services verloren gehen. Um Ihren Marketo Engage-Zugriff beizubehalten, stellen Sie sicher, dass alle Client-Systeme vor dem 13. Dezember 2019 TLS 1.2-kompatibel sind. Weitere Informationen finden Sie [hier](https://nation.marketo.com:443/t5/knowledgebase/tls-1-0-1-1-deprecation-faq/ta-p/249085).

1. Alle Inhalte, die mit Smart Campaign zusammenhängen, befinden sich jetzt im [Smart Campaign](/help/rest-api/smart-campaigns.md)-Menüelement (unter REST API > Assets).

Veröffentlicht am _2019-08-16_ von _David_

## Updates Januar 2020

Im Januar 2020 veröffentlichen wir neue REST-APIs, die bestehende APIs verbessern und Fehler beheben. Unten finden Sie eine vollständige Liste der Updates.

* Es wurde die Möglichkeit hinzugefügt, programmgesteuert benutzerdefinierte Objekt-Schemadefinitionen zu erstellen. Auf diese Weise können Sie ein benutzerdefiniertes Objekt einmal definieren und es für so viele Instanzen wie nötig bereitstellen. Auf diese Weise können Sie es Benutzern ermöglichen, Sandbox- und Center-of-Excellence-Modelle effektiv zu nutzen. Mit können ISVs auch den Onboarding-Prozess für Kunden vereinfachen. Sie benötigen einen entsprechenden Abonnementtyp, um auf die Metadaten-API für benutzerdefinierte Objekte zugreifen zu können.
* Es wurde die Möglichkeit hinzugefügt, Programmmitglieder massenweise zu importieren und zu exportieren. Diese neue Gruppe von Endpunkten folgt dem bestehenden Marketo REST-API-Muster zum Erstellen asynchroner Massenverarbeitungsaufträge. Programmteilnehmer-Datensätze können benutzerdefinierte Felder für Programmteilnehmer und/oder Lead-Felder enthalten.
* Es wurde der Endpunkt Verfügbare Formularprogrammmitgliedsfelder abrufen hinzugefügt, um die Verwendung von benutzerdefinierten Feldern für Programmmitglieder als Formularfelder zu unterstützen. Dadurch wird eine Liste aller benutzerdefinierten Felder des Programmmitglieds zurückgegeben, die in einem Marketo-Formular verwendet werden können.
* Der Endpunkt [E-Mail-Vorlage verwenden von](/help/rest-api/email-templates.md) wurde hinzugefügt, der eine Liste von E-Mail-Assets zurückgibt, die von einer bestimmten E-Mail-Vorlage abhängen. Auf diese Weise können Sie die Auswirkungen einer potenziellen E-Mail-Vorlagenänderung schnell verstehen und diese Abhängigkeiten leichter beheben.

Veröffentlicht am _2020-01-17_ von _David_

## Abrufen aller benutzerdefinierten Objekte

Wir werden oft gefragt, wie wir mit der Marketo-API eine Liste aller [benutzerdefinierten Objekte](https://experienceleague.adobe.com/de/docs/marketo/using/home) (COs) erhalten. Die Abfrage nach COs erfordert mehr als den Namen: _a priori_ auch Wissen über jedes CO erforderlich. Die Methoden zum Abrufen dieses Wissens sind möglicherweise nicht offensichtlich, da die API keine Methode zum direkten Abfragen bereitstellt. Wie bei vielen anderen Zielen in Marketo Engage bieten Smart Lists eine Antwort für COs, die mit Personen (Leads) verknüpft sind. Smart Lists funktionieren anders bei Unternehmen und Sie erhalten eine Liste aller Personen, deren Unternehmen mit dem Objekttyp für den Filter verknüpft sind, sodass Sie Unternehmen je nach Ihren Zielen möglicherweise deduplizieren müssen. Bei jeder Genehmigung eines neuen benutzerdefinierten Objekts wird ein zugehöriger Filter erstellt. Sie wird im Format &quot;**Hat CO-**&quot; benannt. Im folgenden Beispiel lautet der benutzerdefinierte Objektname &quot;**Conference Track Subscription“** und sein Filter heißt &quot;**Has Conference Track Subscription**&quot;. Nachdem Sie die Smart-Liste erstellt haben, können Sie die Informationen abrufen, die zum Abfragen verknüpfter COs mithilfe des Endpunkts [Benutzerdefinierte Objekte“ ](/help/rest-api/custom-objects.md) sind. Exportieren Sie die Liste, um sicherzustellen, dass das verknüpfte Feld enthalten ist (entweder ID oder E-Mail-Adresse). Sie können den Export mithilfe der [Bulk Lead Extract API](/help/rest-api/bulk-lead-extract.md) Filterung nach dem **smartListName** oder **smartListId** Filter oder [Export über die Benutzeroberfläche](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/managing-people-in-smart-lists/export-people-to-excel-from-a-list-or-smart-list). Im nächsten Schritt verwenden Sie jeden verknüpften Feldwert, um verknüpfte benutzerdefinierte Objekte einzeln abzufragen. Der Name des benutzerdefinierten Objekts lautet **ConferenceTrackSubscription“** in diesem Beispiel, und sein API-Name lautet **conferenceTrackSubscription_c**. Den API-Namen finden Sie sowohl in der Benutzeroberfläche als &quot;**API-Name**&quot; als auch über die API als **name**.  Administrator | Benutzerdefinierte Marketo[Objekte/Beschriftung] Hier finden Sie ein Fragment, das vom Endpunkt &quot;[ Custom Objects API“ ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/listCustomObjectsUsingGET) wird:

```json
{
    "name": "conferenceTrackSubscription_c",
    "displayName": "Conference Track Subscription",
    "description": "Track subscription for conference attendees",
    "createdAt": "2020-01-13T19:50:06Z",
    "updatedAt": "2020-01-13T19:50:06Z",
    "idField": "marketoGUID",
    "dedupeFields": [
        "subscriptionID"
    ],
    "searchableFields": [
        [
            "subscriptionID"
        ],
        [
            "marketoGUID"
        ],
        [
            "leadID"
        ]
    ],
    "relationships": [
        {
            "field": "leadID",
            "type": "child",
            "relatedTo": {
                "name": "Lead",
                "field": "Id"
            }
        }
    ]
}
```

Um die benutzerdefinierten Objekte abzurufen, die eins zu eins (1:1) oder eins zu viele (1:N) mit den Personen in Ihrer Smart-Liste verknüpft sind, stellen Sie eine Anfrage wie die folgende:

`GET /rest/v1/customobjects/conferenceTrackSubscription_c.json?filterType=leadID&filterValues=1000302,1000303,1000304,1000306,1000307`

In diesem Beispiel ist dieses benutzerdefinierte Objekt über das Feld **leadID** mit Personen verknüpft, sodass der Filtertyp &quot;**leadID**&quot; lautet. Der Parameter für die Filterwerte ist eine kommagetrennte Liste der IDs, die vom Smart-Listen-Export übernommen werden. Die Anfrage kann so viele Filterwerte enthalten, wie Sie in einen einzelnen Anfrage-URI passen können: bis zu 8K Zeichen. Anfragen, die diese Länge überschreiten, geben einen 414-HTTP-Fehler-Code zurück. Die Antwort kann in mehr als einem Chunk zurückgegeben werden. Wenn ja, **moreResult** auf **true** gesetzt und ein **nextPageToken** eingefügt. Anschließend müssen Sie die Ergebnisse [durchblättern](/help/rest-api/paging-tokens.md) bis **moreResult** &quot;**&quot;**. Im Folgenden finden Sie einen Teil des Ergebnisses für die obige API-Anfrage:

```json
"result": [
    {
        "seq": 0,
        "marketoGUID": "d6b3ed3d-4eb8-4066-a9cd-184c8d385cfe",
        "leadID": "1000302",
        "subscriptionID": "4ad59184-6bf1-4eeb-a583-d82aeee68210",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 1,
        "marketoGUID": "e5e0aba4-f27f-494d-93ed-9cb580989bf3",
        "leadID": "1000303",
        "subscriptionID": "fc5596d5-6fa2-4848-b4a2-89d96e245c59",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 2,
        "marketoGUID": "e65007cd-86b1-4c17-8d55-057c96e1788a",
        "leadID": "1000304",
        "subscriptionID": "7e54b8a0-2170-4d81-a809-4eac349508d0",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 3,
        "marketoGUID": "39d956b2-85e2-4c24-94e7-e9fa5a09d3d0",
        "leadID": "1000306",
        "subscriptionID": "644c8e5b-fc0c-4d4a-80f8-358a65ce0a68",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 4,
        "marketoGUID": "a2151350-50c8-437f-bc71-7a054bb601f0",
        "leadID": "1000307",
        "subscriptionID": "bf14218c-ae6a-42b3-a14e-f7182903cbcd",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    }
```

Jetzt verfügen Sie über die Werte für jedes benutzerdefinierte Objekt, das direkt mit den Personen in Ihrer Smart-Liste verknüpft ist, und über das Abrufen der Werte hinaus können Sie die **marketoGUID** verwenden, um diese Objekte [Aktualisieren](/help/rest-api/custom-objects.md) oder [Löschen](/help/rest-api/custom-objects.md). Bei benutzerdefinierten Objekten, die mit Personen in einer n:n-Beziehung (n:N) verknüpft sind, gibt die obige Technik das Objekt der ersten Ebene zurück, das das Zwischenobjekt ist, das jede Person mit mehreren COs der zweiten Ebene verbindet.

Um diese COs der zweiten Ebene abzurufen, starten Sie einen neuen Satz von Abfragen für den CO-Typ der zweiten Ebene, indem Sie nach dem Verknüpfungsfeld und den Werten filtern, die aus dem Zwischenobjekt der ersten Ebene extrahiert wurden. Beispielsweise könnte das obige Objekt &quot;**Conference Track Subscription** eine andere Objektebene haben, die Sitzungen namens &quot;**&quot; repräsentiert** die wahrscheinlich durch die **subscriptionID** verknüpft wären. Die Anfrage zum Abrufen von Sitzungen, die mit den oben genannten Abonnements für Konferenzspuren verknüpft sind, würde dann wie folgt aussehen:

`GET /rest/v1/customobjects/session_c.json?filterType=subscriptionID&filterValues=4ad59184-6bf1-4eeb-a583-d82aeee68210,e5e0aba4-f27f-494d-93ed-9cb580989bf3,e65007cd-86b1-4c17-8d55-057c96e1788a,39d956b2-85e2-4c24-94e7-e9fa5a09d3d0,bf14218c-ae6a-42b3-a14e-f7182903cbcd`

_Fußnote_ _1)**smartListName**&#x200B;und **smartListId**&#x200B;Filtertypen sind für einige Abonnements nicht verfügbar. Wenn für Ihr Abonnement nicht verfügbar ist, erhalten Sie eine Fehlermeldung beim Aufruf des Endpunkts „Exportleitungs-Auftrag erstellen“ (**„1035, Nicht unterstützter Filtertyp für Zielabonnement“**). Kunden können sich an den Marketo-Support wenden, um diese Funktion in ihrem Abonnement aktivieren zu lassen._

Veröffentlicht am _2020-01-14_ von _Tony_

## So rufen Sie jede Person ab (Lead)

Wir erhalten viele Anfragen zu den Prozessen, die zum Abrufen jeder Person (Lead) aus einer Marketo Engage-Instanz erforderlich sind. Wir haben viele nützliche Antworten gegeben, aber keine ist so vollständig wie diese. Ich habe einige Schlüsselkonzepte identifiziert, die erforderlich sind, um jeden Lead mithilfe der Bulk Extract-API von Marketo zu extrahieren. Alle anderen Details können Sie aus dem Demonstrations-Code lernen, den ich erstellt habe, um damit zu arbeiten. Nachdem Sie diesen Beitrag gelesen und den Demo-Code untersucht haben, verfügen Sie über alle Informationen, die Sie benötigen, um jeden Lead aus einer Marketo Engage-Instanz abzurufen.

### Überblick

Die Kerntechnik verwendet die [Bulk Lead Extract API](/help/rest-api/bulk-lead-extract.md). Möglicherweise können Sie einen Massenexport-Lead ohne Filter durchführen, aber das ist nicht möglich: (**API erfordert einen Filter**. Die verfügbaren Filter sind **createdAt**, **staticListName**, **staticListId** **updatedAt**, **smartListName** und **smartListId**. Das Filtern nach einer Smart List ohne Filter scheint ebenfalls attraktiv zu sein. Probieren Sie das aus und Sie werden feststellen, dass das System intelligent genug ist, um eine Smart-Liste ohne Filter gleich zu behandeln: Die API erfordert auch hier einen Filter. Da wir einen Filter benötigen, ist der vertrauenswürdige und kanonische Filter &quot;**&quot;**. Dieser Filtertyp lässt Datums- und Zeitbereiche bis zu 31 Tage zu, sodass wir mehrere Aufträge ausführen und die Ergebnisse kombinieren müssen. Wir suchen zunächst das älteste Erstellungsdatum für einen Lead in der Zielinstanz. Ab diesem ältesten möglichen Datum erstellen wir Aufträge für die Dauer von 31 Tagen minus einer Sekunde (weitere dazu später). Nach der Erstellung jedes Auftrags werden wir ihn in die Warteschlange stellen und auf seinen Abschluss warten. Dann laden wir die resultierende Datei herunter und überprüfen ihre Integrität mithilfe einer Prüfsumme. Deduplizieren Sie abschließend Leads nach ID und schreiben Sie dann eindeutige Leads in eine CSV-Ausgabedatei.

### Ältesten Lead suchen

Ich verwende einen kleinen „Trick“, um das älteste mögliche Datum für einen Lead in der Zielinstanz zu erhalten. Es gibt keinen API-Endpunkt, der dieser Aufgabe gewidmet ist, daher benötigen wir etwas Kreativität. Was ich tue, ist, alle Ordner mit einer **maxDepth = 1** abzufragen, was uns eine Liste aller Ordner der obersten Ebene in der Instanz gibt. Dann sammle ich die **createdAt**-Daten, analysiere sie und finde das älteste Datum. Diese Methode funktioniert, weil einige standardmäßige Ordner der obersten Ebene mit der Instanz erstellt werden und vorher keine Leads erstellt werden konnten.

### Erforderliche Felder auswählen

Sie müssen entscheiden, welche Felder Sie extrahieren müssen. Suchen Sie die verfügbaren Felder für Ihre Zielinstanz mithilfe des Endpunkts [Lead 2 beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). Die Antwort auf diese Anfrage enthält eine Liste mit dem Namen „Felder“. Hier ist ein Auszug aus einer Beispielantwort:

```json
  "fields": [
      {
          "name": "AccountSource",
          "displayName": "Account Source",
          "dataType": "string",
          "length": 40,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "acquisitionProgramId",
          "displayName": "Acquisition Program",
          "dataType": "reference",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Active_c",
          "displayName": "Active",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "address",
          "displayName": "Address",
          "dataType": "text",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Address_lead",
          "displayName": "Address (L)",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "annualRevenue",
          "displayName": "Annual Revenue",
          "dataType": "currency",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "anonymousIP",
          "displayName": "Anonymous IP",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      }, ...
```

Dieser Endpunkt gibt eine vollständige Liste zurück, die sowohl standardmäßige als auch benutzerdefinierte Felder enthält. Je mehr Felder Sie anfordern, desto länger dauert der Abschluss Ihres Exportvorgangs und desto größer ist die resultierende Datei. Normalerweise sollten Sie nur die benötigten Felder auswählen. Nichts hindert Sie daran, jedes verfügbare Feld anzufordern, also mache ich das. Die zum Erstellen eines Exportvorgangs erforderliche Feldkennung ist der Wert **name**. Die Namenswerte werden in eine Liste aller Feldnamen extrahiert. Dann verwende ich es, um jedes verfügbare Feld anzufordern, wenn ich jeden Exportvorgang erstelle.

### Datumsbereiche des Exportvorgangs: jeweils 31 Tage

Jeder Exportvorgang kann bis zu 31 Tage dauern. Die Demo-Instanz, die ich verwende, wurde im August 2016 erstellt. Daher muss ich heute etwas mehr als 40 Jobs erstellen. Die Anzahl der Tage seit dem ersten Erstellungsdatum dividiert durch 31 aufgerundet. Die -API ermöglicht es, dass zwei Exportvorgänge gleichzeitig verarbeitet werden, sodass Sie mit zwei parallel ausgeführten Vorgängen extrahieren können. Massenextraktionsaufträge sind eine Ressource, die mit jeder anderen Integration geteilt wird, also werde ich nett sein. Ich lasse den anderen Auftrag für andere Integrationen verfügbar und werde das Ausführen einzelner Aufträge nacheinander demonstrieren. Die für den Filter **createdAt** verwendeten Datumsangaben werden anhand der [ISO 8601-](https://www.w3.org/TR/NOTE-datetime) formatiert. Sie befinden sich immer in GMT (Z+0000), sodass die Zeitzone einfach als „Z“ oder &quot;+00“ dargestellt :00. Der 1. August 2016 ist **2016-08-01T00:00:00+00:00** und 31 Tage später ist der 1. September 2016, der **2016-09-01T00:00:00+00:00 ist.** Start- und Endzeiten sind inklusiv, also ziehe ich 1 Sekunde von dieser Endzeit ab: **2016-09-01T00:00:00+00:00** wird **2016-08-31T23:59:59+00:00**. Durch Subtrahieren einer Sekunde werden überlappende Zeiten vermieden. Da GMT der Standardwert ist, können Sie auch **Z** oder **+00:00**.

### Deduplizierung

Obwohl ich mich der Mühe gemacht habe, überlappende Zeiten zu vermeiden, habe ich auch eine Deduplizierung implementiert. Dies tat ich, da es einige Randfälle gibt, in denen sich die Zeiten ändern ([Sommerzeit](https://en.wikipedia.org/wiki/Daylight_saving_time)), was zu mehrdeutigen Werten führt, und daher kann die Bulk Extract-API von Marketo ansonsten unerwartete doppelte Leads zurückgeben. Dies geschieht selten, muss jedoch bei jeder Integration mit Datums-/Uhrzeitfilterbereichen berücksichtigt werden. Ich habe eine Sekunde entfernt, um klarzustellen, dass die Zeiten inklusiv sind. Ich möchte nicht, dass Sie glauben, dass die Erstellung eines Auftrags mit den **createdAt**- und **endAt**-Zeiten **2016-08-01T00:00:00Z** bzw. **2016-09-01T00:00:00Z** keine Leads enthält, die am **2016-09-01T00:00:00Z** erstellt wurden.

### Erstellen eines Auftrags

Der erste Schritt besteht darin, einen Auftrag mit dem Endpunkt [Export-Lead-Auftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createExportLeadsUsingPOST) zu erstellen. In dieser Demo sieht die Anfrage zum Erstellen unseres ersten Exportvorgangs wie folgt aus:

`POST /bulk/v1/leads/export/create.json`

```json
{ "filter": {"createdAt": {"startAt": "2016-08-01T00:00:00",
                           "endAt": "2016-09-01T00:00:00"}},
"fields":["AccountSource","acquisitionProgramId","Active_c","address","Address_lead","annualRevenue","anonymousIP","BillingAddress","billingCity","billingCountry","BillingGeocodeAccuracy","BillingLatitude","BillingLongitude","billingPostalCode","billingState","billingStreet","blackListed","blackListedCause","city","CleanStatus","CleanStatus_account","company","CompanyDunsNumber","contactCompany","cookies","country","createdAt","customfield","DandbCompanyId","DandbCompanyId_account","dateOfBirth","dddS","department","doNotCall","doNotCallReason","dS","dueDate","DunsNumber","email","EmailBouncedDate","EmailBouncedReason","emailInvalid","emailInvalidCause","emailSuspended","emailSuspendedAt","emailSuspendedCause","externalCompanyId","externalSalesPersonId","facebookDisplayName","facebookId","facebookPhotoURL","facebookProfileURL","facebookReach","facebookReferredEnrollments","facebookReferredVisits","fax","firstName","gender","GeocodeAccuracy","holll","id","industry","inferredCity","inferredCompany","inferredCountry","inferredMetropolitanArea","inferredPhoneAreaCode","inferredPostalCode","inferredStateRegion","interested","interestedIn","isAnonymous","IsEmailBounced","isLead","iSTRUE","Jigsaw","JigsawCompanyId_account","JigsawContactId_lead","Jigsaw_account","Languages_c","lastName","LastReferencedDate","LastReferencedDate_account","lastReferredEnrollment","lastReferredVisit","LastViewedDate","LastViewedDate_account","Latitude","leadPartitionId","leadPerson","leadRevenueCycleModelId","leadRevenueStageId","leadRole","leadScore","leadSource","leadStatus","linkedInDisplayName","linkedInId","linkedInPhotoURL","linkedInProfileURL","linkedInReach","linkedInReferredEnrollments","linkedInReferredVisits","links","Longitude","MailingAddress","MailingGeocodeAccuracy","MailingLatitude","MailingLongitude","mainPhone","marketingSuspended","marketingSuspendedCause","middleName","mktoAcquisitionDate","mktocomment1","mktocomments2","mktoCompanyNotes","mktoDoNotCallCause","mktoIsCustomer","mktoIsPartner","mktoName","mktoPersonNotes","mktosync","mktotest1","mobile","mobilePhone","NaicsCode","NaicsDesc","newcustom","numberOfEmployees","originalReferrer","originalSearchEngine","originalSearchPhrase","originalSourceInfo","originalSourceType","OtherAddress","OtherGeocodeAccuracy","OtherLatitude","OtherLongitude","personPrimaryLeadInterest","personTimeZone","personType","phone","PhotoUrl","PhotoUrl_account","postalCode","priority","ProductInterest_c","rating","referal","registrationSourceInfo","registrationSourceType","relativeScore","relativeUrgency","requiredNumberofCylinder","salutation","sfdcAccountId","sfdcContactId","sfdcId","sfdcLeadId","sfdcLeadOwnerId","sfdcType","ShippingAddress","ShippingGeocodeAccuracy","ShippingLatitude","ShippingLongitude","sicCode","SicDesc","site","state","surveyAnswers","syndicationId","testBooleanField","testscore","title","totalReferredEnrollments","totalReferredVisits","Tradestyle","twitterDisplayName","twitterId","twitterPhotoURL","twitterProfileURL","twitterReach","twitterReferredEnrollments","twitterReferredVisits","uNSUBSCIBE","unsubscribed","unsubscribedReason","updatedAt","urgency","url","website","YearStarted"]}
```

Die Antwort sieht wie folgt aus:

```json
{
  "requestId": "6902#16fb52118bf",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Created",
          "createdAt": "2020-01-17T20:10:43Z"
      }
  ],
  "success": true
}
```

### Job in Warteschlange stellen

Der Job ist jetzt geschaffen, aber einfach nur da sitzen und nichts tun. Um den Auftrag auszuführen, müssen wir den Endpunkt [enqueue](https://developer.adobe.com/marketo-apis/api/mapi/#operation/enqueueExportLeadsUsingPOST) mit dem Wert **exportId** aufrufen, um den URI für die Anfrage zu erstellen. Das sieht wie folgt aus:

`POST /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/enqueue.json`

Es gibt keinen Hauptteil für diesen Beitrag. Wir verwenden hier einfach das Verb „POST HTTP“. Diese Anfrage generiert eine Antwort wie die folgende:

```json
{
  "requestId": "1836a#16fb5238a48",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Queued",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z"
      }
  ],
  "success": true
}
```

Wie ich bereits sagte, ist die Anzahl der Aufträge, die gleichzeitig ausgeführt werden können, begrenzt. Außerdem gibt es eine Begrenzung für die Anzahl an Aufträgen, die sich gleichzeitig in der Warteschlange befinden: 10. Wir brauchen mehr als 40 Arbeitsplätze, damit diese Grenze uns daran hindert, alle Arbeitsplätze gleichzeitig zu schaffen. Andere Integrationen können auch Aufträge ausführen, daher müssen wir die Möglichkeit berücksichtigen, dass alle Slots voll sind. Der Versuch, einen neuen Auftrag in die Warteschlange zu stellen, wenn bereits 10 Aufträge in der Warteschlange sind, führt zu einem [1029](/help/rest-api/error-codes.md)Fehler. Verwenden Sie bei einem **1029** ein exponentielles Backoff, bis der Auftrag in die Warteschlange gestellt werden kann. Ich warte 1 Minute und verdoppele diesen Wert jedes Mal, wenn ich einen **1029**-Fehlercode erhalte, der zwischen den Anfragen bis zu 4 Minuten lang sein darf, aber nie länger als das. Diese Technik wird als [Abgeschnittener binärer exponentieller Backoff“ ](https://devopedia.org/binary-exponential-backoff) und ist die Best Practice für behebbare Fehler und Statusprüfungen.

### Auf Abschluss des Vorgangs warten

Die Ausführung jedes Auftrags dauert einige Zeit, sodass wir den [Statusendpunkt“ aufrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET) um den Fortschritt zu überwachen. Auch hier fügen wir die **exportId** wie folgt in den Anfrage-URI ein:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/status.json`

Bevor der Auftrag abgeschlossen ist, erhalten Sie eine Antwort, die wie folgt aussieht:

```json
{
    "requestId": "153cb#16fb525435d",
    "result": [
        {
            "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-17T20:10:43Z",
            "queuedAt": "2020-01-17T20:13:23Z",
            "startedAt": "2020-01-17T20:13:49Z"
        }
    ],
    "success": true
}
```

Ich führe denselben exponentiellen Backoff aus (1 Minute bis zu 4 Minuten), während der Auftragsstatus „In Warteschlange“ lautet. Der Status wird nicht in Echtzeit aktualisiert, sondern einmal pro Minute. Es gibt nur sehr wenig Vorteile, schneller abzurufen. Ich setze den Backoff zurück, wenn sich der Auftragsstatus in „Verarbeitung läuft“ ändert. Wir warten auf den Status „Abgeschlossen“, der wie folgt aussieht:

```json
{
  "requestId": "10ad9#16fb5268f9b",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Completed",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z",
          "startedAt": "2020-01-17T20:13:49Z",
          "finishedAt": "2020-01-17T20:15:28Z",
          "numberOfRecords": 59,
          "fileSize": 74436,
          "fileChecksum": "sha256:de553362e7ffad6556ae9ea749655c35010c7f0e944fc5a85782183130dca18d"
      }
  ],
  "success": true
}
```

Der **numberOfRecords**-Wert ist null, wenn die Anfrage keine Leads zurückgibt. Ich prüfe diesen Wert und überspringe die nächsten Schritte, wenn das sinnvoll ist. Wenn Leads zurückgegeben werden, extrahiere ich den Wert **fileChecksum**. Wir verwenden sie, um die Integrität der Datei beim Herunterladen zu überprüfen.

### Leads abrufen

Wenn **numberOfRecords** größer als null ist, laden Sie die exportierte Datei mit [Export-Lead-Datei abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET) mit einer Anfrage wie der folgenden herunter:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/file.json`

Überprüfen Sie, dass die Datei ohne Fehler übertragen: Berechnen Sie die Prüfsumme der Datei und vergleichen Sie mit der **fileChecksum**, die wir zuvor gespeichert haben. Berechnen Sie die Prüfsumme mithilfe von [SHA-2](https://en.wikipedia.org/wiki/SHA-2) und insbesondere der SHA256-Hash-Funktion. Wenn die berechneten Prüfsummen nicht übereinstimmen, gab es einen Fehler bei der Dateiübertragung und Sie können entweder die Übertragung erneut versuchen oder den Vorgang abbrechen und manuell wiederherstellen.

### Daten aggregieren

Nach dem Radfahren durch alle 31 Tage vom ersten Lead bis heute, haben Sie ein komplettes Set. Außerdem verfügen Sie für jeden Bereich über eine Datei. Die einfachste Möglichkeit, eine einzelne aggregierte Datei mit jedem Lead zu erstellen, besteht darin, diese Dateien zu verketten, nachdem die Kopfzeile für alle Dateien mit Ausnahme der ersten entfernt wurde. Vergessen Sie dabei nicht, später in Ihrer Datenverarbeitungs-Pipeline mit einer möglichen Duplizierung zu rechnen und diese zu planen. In meiner Demonstration verarbeite ich die Dateien, während ich sie herunterlade. Bevor ich jede Datenzeile zur Ausgabedatei hinzufüge, dedupliziere ich sie, indem ich prüfe, ob die Lead-ID der Zeile bereits geschrieben wurde.

Ich habe einen Demonstrations-Code entwickelt, der [hier](https://github.com/Marketo/REST-Sample-Code/tree/master/python/LeadDatabase/Leads) gehostet wird und hoffentlich die Details dieses Prozesses ausfüllt und als Vorlage für Ihre eigene Entwicklung dienen kann. Der Demo-Code ist als Lern-Tool gedacht, damit ein Produktionssystem Verbesserungen an der Robustheit erfordert. **Der Code wird unter der MIT** Lizenz im Istzustand bereitgestellt, ist aber wahrscheinlich ausreichend für eine einmalige Verwendung unter menschlicher Aufsicht. Jetzt hält dich nichts mehr auf! Bei diesem Prozess extrahieren Sie jeden Lead mithilfe der Bulk Extract-API von Marketo und möglicherweise jedes Feld für die Marketo Engage-Zielinstanz. Um Ihre Daten weiter zu erweitern, rufen Sie die Aktivitäten jedes Leads mithilfe der -Techniken ab.

Veröffentlicht am _2020-03-05_ von _Tony_

## Updates Februar 2020

Im Februar 2020 veröffentlichen wir neue REST-APIs. Unten finden Sie eine vollständige Liste der Updates.

### Ankündigungen

* Nach September 2020 akzeptieren [Asset-API](/help/rest-api/assets.md)-Endpunkte den Abfrageparameter **_** nicht mehr. Dies wurde verwendet, um Abfrageparameter in einem POST-Text zu übergeben, um URI-Längenbeschränkungen zu umgehen. Um Anfragen nachzukommen, die diesen Parameter erfordern, wird die URI-Beschränkung für Asset-APIs von 6 KB auf 65 KB erhöht.
* In Bezug auf unsere Position zu ITP, lesen Sie bitte diesen Marketo-Community-Beitrag: [Browser-Cookie-Updates: Wie ist Marketo/Munchkin betroffen](https://nation.marketo.com:443/t5/knowledgebase/browser-cookie-updates-how-marketo-munchkin-is-affected/ta-p/251524)
* Die Aktivität „Status in Bearbeitung ändern“ wurde geändert. Das Attribut „Programmteilnehmer-ID“ wurde zur Unterstützung der bevorstehenden Funktion „Benutzerdefinierte Felder für Programmteilnehmer“ hinzugefügt.

Veröffentlicht am _2020-02-26_ von _David_

## Einstellung der Munchkin Associate-Lead-Methode

Mit der nächsten Version des Munchkin JavaScript-Clients, Version 159, werden wir mit der Einstellung der Munchkin-[Associate-Lead-Methode](/help/javascript-api/api-reference.md) beginnen. Ab dieser Version wird beim Aufrufen der Methode in der Browser-Konsole eine Warnung ausgegeben, die angibt, dass die Methode in einer zukünftigen Version entfernt wird. Nachdem die Methode entfernt wurde, führt der Versuch, die Methode zu verwenden, zu einem Fehler. Marketo-Kunden, bei denen wir festgestellt haben, dass sie diese Methode kürzlich verwendet haben, werden einzeln über ihre Verwendung informiert. Weitere Informationen zur Munchkin-Version 159 finden Sie [in diesem Marketing Nation-Beitrag](https://nation.marketo.com:443/t5/product-documents/munchkin-javascript-version-159-amp-associate-lead-deprecation/ta-p/299687).

### Häufig gestellte Fragen (FAQ)

Woher weiß ich, ob ich betroffen bin?

Adobe benachrichtigt Kundinnen und Kunden, wenn wir die Verwendung dieser Methode bei ihrem Abonnement beobachtet haben, und tut dies mehrere Male während des Zeitraums der veralteten Version.

Wann wird die Methode entfernt?

Diese Methode wird in Munchkin JS v161 entfernt, wo sie zusammen mit der Marketo-Version vom Oktober 2021 allgemein verfügbar sein wird.

Warum wird diese Methode entfernt?

Seit Einführung dieser Methode wurden leistungsfähigere Methoden zur Erfüllung derselben Anwendungsfälle implementiert und veröffentlicht. Um die Leistung und Gesundheit unserer Dienstleistungen zu verbessern, ist es manchmal notwendig, Funktionen zu entfernen, die nicht nach akzeptablen Standards funktionieren.

Was sollte ich anstelle dieser Methode verwenden?

Der Munchkin Associate-Lead hat zwei Hauptanwendungsfälle: die Übermittlung von Personendaten und die Verknüpfung eines Webtracking-Cookies des Browsers mit dem entsprechenden Personendatensatz in Marketo. Seit der Einführung des Munchkin Associate Leads wurden robustere Methoden für die Durchführung der Datenübermittlung und Cookie-Zuordnung implementiert.

#### Server-seitige Übermittlung

Wenn Sie keine browserseitige Übermittlung benötigen, bietet die REST-API viele Methoden für die [Übermittlung von Personendaten](/help/rest-api/leads.md) und eine [speziell entwickelte Methode für die Zuordnung von Cookies zu Personendaten](/help/rest-api/leads.md).

Wann wird Version 159 von Munchkin eingeführt?

Version 159 ist seit der Marketo-Version vom Oktober 2020 allgemein verfügbar.

Veröffentlicht am _2020-05-06_ von _Kenny_

## Durchführen einer Marketo-Formularübermittlung im Hintergrund

Wenn Ihr Unternehmen über viele verschiedene Plattformen zum Hosten von Web-Inhalten und Kundendaten verfügt, ist es relativ üblich, parallele Übermittlungen von einem Formular zu benötigen, damit die resultierenden Daten in separaten Plattformen erfasst werden können. Dazu gibt es mehrere Strategien, aber die beste ist oft die einfachste: Verwenden der Forms 2-API zum Senden eines ausgeblendeten Marketo-Formulars. Dies funktioniert mit jedem neuen Marketo-Formular, aber Sie sollten idealerweise ein leeres Formular dafür erstellen, das keine Felder enthält. Dadurch wird sichergestellt, dass das Formular nicht mehr Daten als nötig lädt, da wir nichts rendern müssen. Nehmen Sie nun einfach den [Einbettungs-Code](https://experienceleague.adobe.com/de/docs/marketo/using/home) aus Ihrem Formular und fügen Sie ihn dem Textkörper Ihrer gewünschten Seite hinzu, indem Sie eine kleine Änderung vornehmen. Ihr Einbettungs-Code enthält ein Formularelement wie das folgende:

`<form id="mktoForm_1068"></form>`

Sie sollten &#39;style=„display:none&quot;&#39; zum Element hinzufügen, damit es nicht sichtbar ist, wie in diesem Beispiel:

`<form id="mktoForm_1068" style="display:none"></form>`

Sobald das Formular eingebettet und ausgeblendet ist, ist der Code zum Senden des Formulars sehr einfach:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"<test@example.com>",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms hat auf diese Weise gesendet und verhält sich genau so, als ob der Lead ein sichtbares Formular ausgefüllt und gesendet hätte. Das Auslösen der Übermittlung variiert zwischen den verschiedenen Implementierungen, da jede eine andere Aktion hat, um sie zu veranlassen, aber Sie können sie für jede Aktion ausführen lassen. Wichtig ist, dass Sie Ihre Felder und Werte richtig einstellen. Verwenden Sie unbedingt den SOAP-API-Namen Ihrer Felder, den Sie unter Feldnamen exportieren finden, um eine korrekte Übermittlung der Werte sicherzustellen.

### Migration vom Munchkin Associate-Lead

Die Übermittlung von Hintergrundformularen ist eine der empfohlenen Ersatzmethoden für Munchkin Associate Lead. Auf der folgenden Beispielseite für HTML wird die Migration auf einer allgemeinen Ebene veranschaulicht, indem dieselben Werte, die an Associate Lead übermittelt werden, wiederverwendet werden.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName['head'](0).appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //pass the same set of values to associateLead
            //hashString: secret + email
            Munchkin.munchkinFunction('associateLead', values, "CHANGE ME");

            //submit the form
            form.submit();


        })
    </script>
</body>

</html>
```

Veröffentlicht am _2020-05-26_ von _Kenny_

## Updates Juli 2020

Im Juli 2020 veröffentlichen wir neue REST-APIs, verbessern bestehende APIs und beheben Fehler. Unten finden Sie eine vollständige Liste der Updates.

* Es wurden zwei Endpunkte hinzugefügt, mit denen Sie Benutzer abfragen und löschen können, die ihre Einladung noch nicht angenommen haben (d. h. „ausstehende“ Benutzer sind). Mit [ Endpunkt „Eingeladenen Benutzer nach ID abrufen](/help/rest-api/user-management.md) können Sie einen ausstehenden Benutzer abfragen. Der Endpunkt [Eingeladenen Benutzer löschen](/help/rest-api/user-management.md) ermöglicht das Löschen eines ausstehenden Benutzers.
* Der [Invite User](/help/rest-api/user-management.md)-Endpunkt wurde aktualisiert, um ISO 8601-konforme Datums- und Zeitzeichenfolgen für den Parameter **expiresAt** zu akzeptieren.
* Sowohl [ Endpunkte „Benutzer nach ID abrufen](/help/rest-api/user-management.md) als auch [Benutzerattribut aktualisieren](/help/rest-api/user-management.md) wurden aktualisiert, um die letzte Benutzeranmeldezeit im Attribut **lastLoginAt“**.
* Es wurde ein Problem behoben[ bei dem der Endpunkt &quot;](https://developer.adobe.com/marketo-apis/api/asset/#operation/createStaticListUsingPOST) Liste erstellen“ den Fehler „611, Systemfehler“ zurückgab, wenn versucht wurde, eine bereits vorhandene statische Liste zu erstellen. Es wurde geändert und der Fehler „709, statische Liste mit demselben Namen existiert bereits“ wird zurückgegeben. [LM-135934]
* Es wurde ein Problem behoben[ bei dem der Endpunkt ](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailUsingPOST)E-Mail erstellen“ den Fehler „611, Systemfehler“ zurückgab, wenn versucht wurde, eine bereits vorhandene E-Mail zu erstellen. Geändert, um Fehler „709, E-Mail mit demselben Namen existiert bereits“ zurückzugeben. [LM-138648]
* Es wurde ein Problem behoben, bei dem Endpunkte für Landingpage-Abfragen falsche „createdAt **-Werte**. Die Endpunkte gaben die Zeit zurück, zu der eine Landingpage zuletzt genehmigt wurde. Sie kehren jetzt zu der Zeit zurück, zu der eine Landingpage erstellt wurde. [LM-138648]
* Es wurde ein Problem behoben[ bei dem der Endpunkt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST)Zusammenführen von Leads“ den Fehler „611, Systemfehler“ für einen ungültigen Zusammenführungsvorgang zurückgab. Tritt auf, wenn die Zusammenführung zu einem doppelten Lead führte und **mergeinCRM** auf „true“ gesetzt wurde. Es wurde geändert und der Fehler „712“ wird zurückgegeben. Es wird ein doppelter Datensatz erstellt. Es wird empfohlen, stattdessen einen vorhandenen Datensatz zu verwenden.“ [LM-137463]

Veröffentlicht am _2020-08-01_ von _David_

## Gastbeitrag - Vertiefung: Benutzerdefiniertes Objekt vs. benutzerdefinierte Aktivitäten vs. benutzerdefiniertes Feld

**Dies ist ein Gastbeitrag von Amit Jain, Marketo Champion 2020, MarTech IT Specialist** Als Enterprise-Marketing-Automatisierungsplattform verwaltet Marketo Ihre Benutzer-/Kunden-/Interessenteninformationen, die aus verschiedenen Quellen erfasst wurden, und verwaltet diese in Marketo, um die Personalisierung, Segmentierung und Berichterstellung zu verbessern. Diese Quellen können von Ihren Website-Formularen über die Erstellung Ihrer CRM-Daten bis hin zu Ihren E-Commerce-Daten reichen. Marketo bietet Standardobjekte wie Lead, Unternehmen, Opportunity und Aktivität. Diese Standardobjekte reichen manchmal nicht aus, um die neuen Marketing-Anforderungen erweiterter Datensätze zu erfüllen, die in Marketo verfügbar sein sollen.

Zum Beispiel Artikel, die zum Warenkorb hinzugefügt wurden, Studenten, die sich für verschiedene Kurse beworben haben, Produkte, die einer bestimmten Person gehören, und Einverständnisse für verschiedene Abonnements usw. Diese Informationen können für Marketing-Fachleute von entscheidender Bedeutung sein, um die Interaktion mit Kunden/Interessenten aufrechtzuerhalten, indem plattformübergreifend personalisiertere Inhalte und ein einheitlicheres Benutzererlebnis bereitgestellt werden. Um diese Geschäftsanforderungen zu erfüllen, bietet Marketo benutzerdefinierte Möglichkeiten, diesen Datentyp in benutzerdefinierten Feldern, benutzerdefinierten Aktivitäten und benutzerdefinierten Objekten zu speichern. Ich habe beobachtet, dass Menschen Probleme haben zu verstehen, wann welche der Optionen zu verwenden ist. In diesem Blogpost (**werden wir im Detail verstehen, was diese drei Dinge tatsächlich sind und wann wir welches mit einigen Beispielen verwenden sollten.**

**Benutzerdefinierte Felder**

Das „Lead“-Objekt in Marketo ist das Master-Objekt und alles andere ist direkt oder indirekt mit diesem verbunden. Mit Marketo können Sie benutzerdefinierte Felder für das „Lead“-Objekt und das „Unternehmen“-Objekt erstellen. Kürzlich wurde die Unterstützung benutzerdefinierter Felder für „Programmteilnehmer“ angekündigt. Diese benutzerdefinierten Felder erfüllen Ihre Anforderung, bestimmte Datentypen zu speichern. Beispielsweise müssen Sie möglicherweise die Auftragsfunktion oder andere Einverständnisse des Benutzers speichern. Es gibt zwei Arten von benutzerdefinierten Feldern in Marketo:

1. **Aus CRM-Feldern synchronisiert:** Im Rahmen der automatischen Marketo ↔︎ SFDC-Synchronisierung synchronisiert Marketo alle benutzerdefinierten Felder, die für Marketo-Benutzende in SFDC sichtbar sind, in Lead-, Kontakt-, Konto- und Opportunity-Objekten.
1. **Nur Marketo-Felder** Sie können ein Feld direkt in Marketo erstellen. Die Daten dieser Felder werden nicht mit SFDC synchronisiert.

**Schnelltipps**

* Verfügen Sie über ein reines Marketo-Feld und möchten jetzt die Daten in SFDC synchronisieren? In [diesem Blog](https://themarketingautomationblog.com/2019/12/06/field-management-merging-remapping-hiding-marketo-fields/) erfahren Sie, wie Sie das tun können.
* Weitere Informationen über benutzerdefinierte Felder finden Sie [hier](https://themarketingautomationblog.com/2019/11/15/knowing-your-marketo-fields/).
* Marketo-APIs unterstützen ab jetzt nicht mehr die Aktualisierung/Erstellung benutzerdefinierter Felder.

**Benutzerdefinierte Objekte**

Neben Standardobjekten können Sie mit Marketo auch eigene benutzerdefinierte Objekte erstellen. _Sie können benutzerdefinierte Objekte erstellen und sich entweder auf ein Firmen- oder Lead-Objekt oder auf ein anderes benutzerdefiniertes Objekt beziehen._ Ein benutzerdefiniertes Objekt ist ein Satz benutzerdefinierter Datensätze, die die standardmäßigen Lead- und Firmendatensätze ergänzen. Benutzerdefinierte Objekte ermöglichen es Ihnen, zusätzliche Daten skalierbar zu speichern und diese Daten mit einem Lead- oder Firmendatensatz zu verknüpfen. Sie können ein benutzerdefiniertes Objekt mit einer beliebigen Kombination aus standardmäßigen (Verknüpfungsfelder) und benutzerdefinierten Feldern erstellen, diese Felder ausfüllen, um ein benutzerdefiniertes Objekt (_)_ erstellen, und diese Datensätze dann mit einem Lead- oder Firmendatensatz verknüpfen. Leistungsstarke und flexible verknüpfte Datensätze ergänzen Ihre Segmente, Smart Lists und Kampagnen, indem Sie diese Assets mit Informationen erstellen, die in Lead- und Unternehmensfeldern nicht zu finden sind. Ein benutzerdefiniertes Objekt kann einen der folgenden Beziehungstypen aufweisen:

* **Eins-zu-eins-Beziehung**: Jedes benutzerdefinierte Objekt hat einen einzelnen Lead/Firmenobjekt-Datensatz.
* **Eins-zu-Viele-Beziehung**: wobei jedes benutzerdefinierte Objekt mehrere benutzerdefinierte Objektdatensätze enthält, die sich auf einen Lead/ein Unternehmen beziehen.
* **Viele-zu-viele-Beziehung:** bei der mehrere benutzerdefinierte Objektdatensätze mit mehreren Lead-/Unternehmensobjekten verknüpft werden können. Beispielsweise werden mehrere Studierende in mehreren Kursen aus einem Kurskatalog eingeschrieben.

**Schnelltipps**

* Weitere Informationen zum Einrichten benutzerdefinierter Objekte [hier](https://experienceleague.adobe.com/de/docs/marketo/using/home).
* Sie können das benutzerdefinierte Objekt von Marketo als Zwischenobjekt verwenden, d. h. als benutzerdefiniertes Objekt des benutzerdefinierten Objekts.

### Warum benutzerdefinierte Objekte?

Benutzerdefinierte Objekte ermöglichen es Ihnen, eindeutige Daten zu kompilieren und zu verwenden, die für ein Unternehmen oder einen Lead relevant sind, aber nicht unbedingt statische Informationen über das Unternehmen oder den Lead selbst darstellen. Während sich Lead-Felder auf die Lead-Informationen eines Kontakts (_E-Mail-_, _Postleitzahl_ usw.), Geschäftsinformationen (_Auftragstitel_, _Branche_ usw.) oder systemgesteuerte Informationen (_Marketo ID_, eine SFDC ID oder ein Erstellungsdatum usw.) beziehen, können benutzerdefinierte Objektfelder vollständig angepasst werden. Verwenden Sie beispielsweise benutzerdefinierte Objekte, um Informationen wie die folgenden zu speichern:

* Der Kaufverlauf eines Benutzers.
* Warenkorbinformationen.
* Gibt an, ob ein Kunde einen Ihrer zeitlich begrenzten Rabattcodes für Werbeaktionen verwendet hat.
* Ergänzende Informationen aus Umfrageergebnissen, Interviews und der Teilnahme an Veranstaltungen.
* Informationen zur Testversion eines Benutzers, einschließlich der URL der Testinstanz, Startdatum, Enddatum, Anzahl der Benutzer usw.

### Benutzerdefinierte Objektbeschränkungen in Marketo

1. Standardmäßig können Sie mit Marketo 10 benutzerdefinierte Objekte erstellen. Sie können das Limit gegen eine zusätzliche Abonnementgebühr erhöhen.
1. Die Standardanzahl von Feldern pro Objekt beträgt 50, Sie können jedoch bei Bedarf Marketo-Unterstützung für zusätzliche Felder anfordern. Vielen Dank [Michael Florin](https://www.linkedin.com/in/michaelflorin/) für zusätzliche Beiträge.
1. Es gibt ein Limit für die Anzahl der Datensätze, die Sie in allen benutzerdefinierten Objekten haben können. Das hängt von Ihrem Abonnement bei Marketo ab. Diese Obergrenze kann gegen eine zusätzliche Abonnementgebühr erhöht werden.
1. Wenn ein benutzerdefiniertes Objekt mithilfe der API erstellt wurde, erlaubt Marketo nicht, das CO-Schema über die Marketo-Benutzeroberfläche zu bearbeiten.

**Schnelltipps**

* Die CRUD-Vorgänge (Erstellen, Lesen, Aktualisieren und Löschen) der Marketo-APIs unterstützen benutzerdefinierte Objekte.
* Sie können diese benutzerdefinierten Objektdaten für die E-Mail-Personalisierung mit dem Velocity-Skript verwenden.
* Alle Endpunkte, die sich auf benutzerdefinierte Objekte beziehen, finden Sie [hier](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportProgramMembersStatusUsingGET).

### Terminologie benutzerdefinierter Objekte

**Benutzerdefiniertes Objekt**: Ein Container, der eine Gruppierung aller benutzerdefinierten Objektdatensätze enthält. Wird auch als Datenkartensatz oder benutzerdefinierte Tabelle bezeichnet. **Benutzerdefinierter Objektdatensatz**: Datendatensatz mit zusätzlichen Feldinformationen, die mit einem Lead oder einem Unternehmen verknüpft werden können. Ein Datensatz kann aus standardmäßigen Lead- oder Firmenfeldern und benutzerdefinierten Objektaufzeichnungsfeldern bestehen. Wird formell als Datenkarte oder Datentabellenzeile bezeichnet. **Benutzerdefiniertes Objektaufzeichnungsfeld**: Vollständig anpassbare Felder zum Erfassen eindeutiger oder temporärer Informationen. Diese Felder werden erstellt und im benutzerdefinierten Objekt selbst gespeichert. Wird im Allgemeinen als Datenkartenfeld oder Datenbanktabellenfeld/-spalte bezeichnet. **Verknüpfungsfeld**: Spezieller Typ des Datensatzfelds für ein benutzerdefiniertes Objekt, um die Beziehung zwischen einem benutzerdefinierten Objektdatensatz und einem verknüpften Lead-/Firmenobjektdatensatz zu definieren. Beim Erstellen benutzerdefinierter Objekte müssen Sie Verknüpfungsfelder bereitstellen, um den benutzerdefinierten Objektdatensatz mit dem richtigen übergeordneten Datensatz zu verbinden.

* Bei einer Eins-zu-Viele- oder Eins-zu-eins-benutzerdefinierten Struktur können Sie das Verknüpfungsfeld im benutzerdefinierten Objekt verwenden, um es mit einer Person oder einem Unternehmen zu verbinden.
* Bei einer n:n-Struktur verwenden Sie zwei Verknüpfungsfelder, die mit einem separat erstellten Zwischenobjekt verbunden sind (bei dem es sich auch um eine Art von benutzerdefiniertem Objekt handelt). Eine Verknüpfung stellt eine Verbindung zu Personen oder Unternehmen in Ihrer Datenbank her, die andere eine Verbindung zum benutzerdefinierten Objekt. In diesem Fall befindet sich das Verknüpfungsfeld nicht im benutzerdefinierten Objekt selbst.

### Eigene Aktivitäten

**Es gibt mehrere Möglichkeiten, wie jemand mit unserer Organisation interagieren kann. Sie können die Website Ihres Unternehmens besuchen, an einer Ihrer Messen teilnehmen oder vielleicht auf einen Link in einer von Ihnen gesendeten E-Mail klicken. Bei diesen Aktionen handelt es sich** Aktivitäten, und egal welche Aktion sie ausführen, Marketo erfasst sie, damit Ihre Marketing- und Vertriebsteams das Benutzerverhalten für personalisierte und einheitliche Interaktionen besser verstehen können. **_Benutzerdefinierte Aktivitäten_** _kann Ihnen dabei helfen, eine Aktivität zu verfolgen, die nicht mit einem Marketo-Formular, einer E-Mail oder einer Landingpage verbunden_. Wenn Sie beispielsweise verfolgen möchten, wann jemand ein Video auf einer Website angesehen oder eine Umfrage durchgeführt hat, verwenden Sie eine benutzerdefinierte Aktivität. Benutzerdefinierte Aktivitäten unterscheiden sich von benutzerdefinierten Objekten. Verwenden Sie benutzerdefinierte Objekte, wenn sich der Wert ändern kann (d. h., wenn sich die „Autofarbe“ von Blau in Rot ändert). Verwenden Sie benutzerdefinierte Aktivitäten, wenn Sie aufgetretene Momente verfolgen und deren Details sich nicht ändern können (z. B. „gekauftes Auto„). Standardmäßig ist die maximale Anzahl von benutzerdefinierten Aktivitäten, die definiert werden können, auf 10 begrenzt. Dies kann gegen eine zusätzliche Abonnementgebühr erhöht werden. Gemäß der [Marketo-Richtlinie zur Datenaufbewahrung](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents) werden benutzerdefinierte Aktivitäten nach 25 Monaten automatisch gelöscht.

**Benutzerdefinierte Aktivität** Nicht-Marketo-Ereignisse, die Sie in Marketo verfolgen möchten. **Benutzerdefinierte Aktivitäts-ID:** Weisen Sie der benutzerdefinierten Aktivität von Marketo eine numerische ID zu, die beim Versuch verwendet werden kann, die Aktivitätsdaten mithilfe der Marketo-API per Push zu übertragen oder abzurufen. **Benutzerdefinierte Aktivitätsfelder:** Aktivitätsmetadaten können in einem Aktivitätsfeld gespeichert werden. Wenn Sie beispielsweise die Ansichten auf einem Video nachverfolgen, können die Felder die Seiten-URL, der Videotitel usw. sein. **Primäres Feld für benutzerdefinierte Aktivität** Benutzerdefinierte Aktivitätsfelder, die als Filterkriterien für Smart-Listen verwendet werden können.

**Schnelltipps**

* Die API-Endpunkte für benutzerdefinierte Aktivitäten sind verfügbar [hier.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST)

## Benutzerdefiniertes Objekt vs. benutzerdefinierte Aktivität

**Benutzerdefiniertes Objekt**

**Benutzerdefinierte Aktivität**

1

Standardmäßig maximal 10 benutzerdefinierte Objekte pro Instanz.

Standardmäßig maximal 10 benutzerdefinierte Aktivitäten.

2

Max. 50 benutzerdefinierte Objektfelder pro benutzerdefiniertem Objekt.

Jeder benutzerdefinierte Aktivitätstyp kann bis zu 20 sekundäre Attribute aufweisen.

3

Max. 1 mm benutzerdefinierte Objektaufzeichnungen; kann je nach Abonnement variieren.

Nach 25 Monaten werden benutzerdefinierte Aktivitäten gemäß der Marketo-Richtlinie zur Datenaufbewahrung gelöscht.

4

Kann als Filter und Trigger in Smart-Listen und Smart-Kampagnen verwendet werden.

Kann als Filter und Trigger in Smart-Listen und Smart-Kampagnen verwendet werden.

5

Kann zum Personalisieren des E-Mail-Inhalts verwendet werden.

Kann nicht zum Personalisieren des E-Mail-Inhalts verwendet werden.

6

Kann CRUD-Vorgänge für einen benutzerdefinierten Objektdatensatz ausführen.

In benutzerdefinierten Aktivitäten sind nur Erstellen und Lesen zulässig.

7

Verwenden Sie benutzerdefinierte Objekte, wenn sich der Wert ändern kann (d. h., wenn sich die „Autofarbe“ von Blau in Rot ändert).

Verwenden Sie benutzerdefinierte Aktivitäten, wenn Sie aufgetretene Momente verfolgen und deren Details sich nicht ändern können (z. B. „gekauftes Auto„).

8

Benutzerdefinierte Objekte sagen Ihnen die Tatsache. d. h. Barwert.

Benutzerdefinierte Aktivitäten erzählen Ihnen von den Ereignissen, die in der Vergangenheit stattgefunden haben.

Veröffentlicht am _2020-10-18_ von _Amit_

## Updates Januar 2021

Im Januar 2021 veröffentlichen wir neue REST-APIs und beheben mehrere Fehler. Unten finden Sie eine vollständige Liste der Updates.

* Der Endpunkt [Formular senden](/help/rest-api/leads.md) wurde hinzugefügt, mit dem Sie programmgesteuerte Formularübermittlungen durchführen können. Formulare von Drittanbietern können jetzt in Marketo Forms integriert werden, um bestehende Marketing-Workflows zu nutzen.
* Endpunkt [Abrufen des vollständigen Inhalts einer Landingpage](/help/rest-api/landing-pages.md) hinzugefügt, der die serialisierte HTML-Version einer Landingpage zurückgibt. Ermöglicht die Wiedergabe vollständig personalisierter Vorschauen von Landingpages, ohne dass eine Anmeldung bei Marketo Engage erforderlich ist. Auf diese Weise können Bearbeitungs- und Übersetzungs-Workflows in integrierten Anwendungen optimiert werden.
* Sie können jetzt die Anzahl der benutzerdefinierten Objekte konfigurieren, auf die über das Velocity-Skript zugegriffen werden kann. Konfigurationsanweisungen finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).

### Fehlerbehebungen

* Es wurde ein Problem behoben[ bei dem der Endpunkt ](/help/rest-api/user-management.md)Benutzer löschen“ es Ihnen erlaubte, einen Benutzer, der nur über eine API verfügt und von einem benutzerdefinierten Service verwendet wird, zu löschen. Jetzt wird der Fehler „611, You cannot delete an API user that is used in API service“ zurückgegeben. [LM-141893]
* Es wurde ein Problem behoben[ bei dem der Endpunkt ](/help/rest-api/user-management.md)Get Users“ in einigen Fällen gelöschte Benutzer zurückgab. [LM-141542]
* Es wurde ein Problem behoben[ bei dem der Endpunkt ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)Programm klonen“. Wenn Sie einen Programmnamen mit mehr als 255 Zeichen angeben, wird „611, Fehler beim Klonen des Programms“ zurückgegeben. Jetzt wird „701, Name darf nicht mehr als 255 Zeichen“ zurückgegeben. [LM-143436]
* Es wurde ein Problem mit [ Endpunkt „Landingpage-Entwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) behoben. Bei der Genehmigung einer Landingpage mit aktivierter mobiler Version werden in der Desktop-Version in bestimmten Fällen Inhalte der mobilen Version angezeigt. [LM-146867]
* Fehlerkorrektur - Der Endpunkt [Genehmigung für Landingpage aufheben](https://developer.adobe.com/marketo-apis/api/asset/#operation/unapproveLandingPageByIdUsingPOST) ermöglicht es jetzt, die Genehmigung für eine Landingpage, die als Folgeseite von einem oder mehreren Formularen verwendet wird, aufzuheben. Es wird jetzt der Fehler „709, Unapproval Landingpage failed“ (Genehmigung der Landingpage aufheben fehlgeschlagen) zurückgegeben. Eine Landingpage wird von einem oder mehreren Formularen als Folgeseite mit Formular-IDs verwendet [_„formId1, formId2,…_]&quot;. [LM-143326]

Veröffentlicht am _2021-01-15_ von _David_

## Munchkin 160 Beta und Beacon-API

**27. Januar 2021:** Einige Marketo-Benutzende, die von der Einstellung für Associate Lead betroffen sind, haben fälschlicherweise eine E-Mail-Benachrichtigung erhalten, in der darauf hingewiesen wird, dass sie die Munchkin Beta-Einstellung auf einer oder mehreren ihrer Instanzen aktiviert haben. Diese Version wird gespeichert, bis die korrekte Zielgruppe benachrichtigt werden kann. Ab Version 160 von Munchkin JavaScript wird die [Beacon-API](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) zur Standardmethode für die Kommunikation von Munchkin mit dem Marketo-Backend. Diese wurde im Sommer 2020 mit der Veröffentlichung der Version 159 über den Konfigurationsparameter **useBeaconAPI** als Option verfügbar. Die Beacon-API bietet gegenüber der alten XMLHttpRequest-Methode mehrere Vorteile, die wesentliche Verbesserung besteht jedoch darin, dass es sich um eine nicht blockierende asynchrone API für HTTP-Kommunikation handelt, die in allen modernen Internet-Browsern verwendet werden kann. Obwohl die meisten Benutzer von Munchkin keine Änderung im Verhalten der Website bemerken, verhindert dieses Update, dass Munchkin die Navigation blockiert, während sie darauf warten, ein Klickereignis an das Backend zu senden. Dies verhindert ganz einfach, dass Munchkin einen Browser veranlasst, nach dem Klicken auf einen Link zu einer neuen Seite zu „hängen“. Dies war für einige Marketo-Kunden ein seltenes, aber frustrierendes Ereignis.

Seit dem 27. Januar 2021 ist der Rollout dieser Version bis zur Neuplanung zurückgestellt. Obwohl keine Probleme im Zusammenhang mit dieser Änderung erwartet werden und beim Testen keine festgestellt wurden, ist es für Marketo nicht möglich, alle möglichen Bereitstellungskonfigurationen von Munchkin zu testen. Möglicherweise möchten Sie diese Änderungen zuvor testen oder auf diese Änderung verzichten, bis diese Version allgemein verfügbar ist. Nachfolgend finden Sie Anweisungen für verschiedene Szenarien.

### Beacon-API testen

Wenn Sie die aktualisierte Beacon-API im Hinblick auf die bevorstehende Version testen möchten, können Sie dies tun, indem Sie den Parameter **useBeaconAPI** zu Ihrer Munchkin-Konfiguration auf einer externen Testseite hinzufügen. Dieser Test funktioniert entweder mit der allgemein verfügbaren oder mit der Betaversion von Munchkin. Der Konfigurationsparameter wird unten im zweiten Argument des Aufrufs der `Munchkin.init()`-Methode in Zeile 7 angezeigt: `{ 'useBeaconAPI': true}`

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827', {"useBeaconAPI":true});
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

### Deaktivieren von Munchkin Beta auf Marketo-Landingpages

Um Munchkin Beta Marketo auf Landingpages zu deaktivieren, müssen Sie auf Ihr [Treasure Chest](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features)-Menü im Admin-Bereich Ihres Abonnements zugreifen und die Einstellung von Munchkin Beta auf Landingpages auf Deaktiviert ändern.

### Deaktivieren von Munchkin Beta auf externen Seiten

Wenn Sie die Beta-Version von Munchkin JavaScript auf externen Web-Seiten bereitgestellt haben und auf diese Änderung verzichten möchten, bis sie allgemein verfügbar ist, müssen Sie Ihr Munchkin-JS-Snippet ändern, um das **Munchkin“ auszuwählen.**&#x200B;**js**-Datei anstelle der **Munchkin-Beta.**&#x200B;**js**-Datei. Im folgenden Beispiel ist dies der Wert der Variablen **s.src** in Zeile 11. Ihr Code-Ausschnitt ähnelt möglicherweise nicht dem Beispiel oder wird von einem Tag-Manager auf Ihren externen Seiten bereitgestellt. Möglicherweise müssen Sie sich mit Ihren IT-Ressourcen oder dem Benutzer in Verbindung setzen, der Ihre Websites mit aktiviertem Munchkin-Tracking verwaltet.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';//This line should have the munchkin.js file, not munchkin-beta.js
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

Veröffentlicht am _2021-01-08_ von _Kenny_

## Abschließende Einstellung der API von E-Mail V1

[Die Einstellung der E-Mail V1 begann vor fast zwei Jahren](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) und ab der Wartungsversion für März an die Abonnements für London und die Niederlande am 17. März 2021 und alle anderen Abonnements am 19. März 2021 wird die gesamte API-Unterstützung für E-Mails der Version 1 eingestellt. Nach dieser Version führt jeder Versuch, über die Asset-APIs mit E-Mails der Version 1 zu interagieren, zu Fehlern und es werden keine Aktionen durchgeführt. Alle bekannten verbleibenden Benutzer seit dem 24. Februar 2021 wurden benachrichtigt, es sind jedoch noch Integrationen vorhanden, die versuchen können, mit diesen Assets zu interagieren. Die am häufigsten betroffenen Integrationstypen sind Services, die Digital Asset Management, Übersetzung und Lokalisierung anbieten. Falls Sie aufgrund dieser Änderung Integrationsfehler feststellen, können [ problematische Assets dennoch aktualisieren, indem Sie sie bearbeiten und genehmigen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/transitioning-to-email-editor-2-0). Nachdem ein E-Mail-Asset auf Version 2 aktualisiert wurde, sollten Sie es mit integrierten Services wieder verwenden können.

Veröffentlicht am _2021-03-17_ von _Kenny_

## Updates Mai 2021

Im Mai 2021 veröffentlichen wir neue REST-APIs, erweitern bestehende REST-APIs und beheben mehrere Fehler. Unten finden Sie eine vollständige Liste der Updates.

* Es wurden Programmteilnehmer-APIs hinzugefügt, mit denen Sie Programmmitgliedschaftseinträge abrufen, aktualisieren und löschen können. Weitere Informationen finden Sie [REST API > Lead-Datenbank > Programmmitglieder](/help/rest-api/program-members.md).
* Es wurden APIs zum Extrahieren benutzerdefinierter Massenobjekte hinzugefügt, mit denen Sie benutzerdefinierte Marketo-Objektdatensätze der ersten Ebene exportieren können, die mit Leads in einer Eins-zu-viele-Beziehung verknüpft sind. Weitere Informationen finden Sie unter [REST API > Bulk Extract > Bulk Custom Object Extract](/help/rest-api/bulk-custom-object-extract.md).
* Wir haben sowohl die [Lead-API](/help/rest-api/leads.md) als auch die [Bulk-Lead-Extract-](/help/rest-api/bulk-lead-extract.md)) verbessert, um Benutzerinnen und Benutzern das Abrufen der Adobe Experience Cloud-ID (ECID) zu ermöglichen. Auf diese Weise können Benutzende, die [Zielgruppen aus Adobe Experience Cloud synchronisieren](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html?lang=de) Leads identifizieren, die verknüpfte ECIDs haben. Dies eröffnet [Integrationsmöglichkeiten](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360024277392-Adobe-Experience-Cloud-Using-the-ECID-for-integration) mit anderen Adobe Experience Cloud-Produkten.
* Wir haben die API [Massenimport von Leads“ verbessert](/help/rest-api/bulk-lead-import.md) um das Hinzufügen von Leads zu Unternehmensdatensätzen während des Importvorgangs zu unterstützen. Dazu fügen Sie das Feld **externalCompanyId** in die Importdatei ein.
* Wir haben mehrere Programm-Endpunkte erweitert, um die Parität mit den in der Marketo Engage-Benutzeroberfläche verfügbaren Funktionen zu gewährleisten. Die Endpunkte [Programme erstellen](/help/rest-api/assets.md) und [Programme klonen](https://developer.adobe.com/marketo-apis/api/asset/) wurden verbessert, um Vorgänge zum Erstellen, Klonen oder Verschieben von Ereignisprogrammen zu ermöglichen. Dies ist für Benutzende gedacht, die Veranstaltungsprogramme organisieren, indem sie unter anderen Programmtypen „verschachtelt“ werden. Wir haben auch den Endpunkt [Programm löschen](https://developer.adobe.com/marketo-apis/api/asset/) verbessert, um das Löschen von Programmen zu ermöglichen, die die folgenden Assets enthalten: Push-Benachrichtigungen, In-App-Nachrichten, Berichte, Landingpages mit eingebettetem Social Assets.
* Als Marketo-Administrator können Sie [ein bestimmtes Feld als „sensibel“ markieren](https://experienceleague.adobe.com/de/docs/marketo/using/home) sodass seine Werte [nie in Formularen vorausgefüllt werden](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/forms/form-fields/disable-pre-fill-for-a-form-field) und so die sensiblen Daten der Benutzenden geschützt werden. Wir haben mehrere Formularfeld-Endpunkte erweitert, um für Parität mit dieser Funktion zu sorgen, die in der Marketo Engage-Benutzeroberfläche zu finden ist.

### Fehlerbehebungen

* Es wurde ein Problem mit dem Endpunkt Programm löschen behoben. Wenn Sie versucht haben, ein Programm in einem freigegebenen Ordner zu löschen, wird „611, Systemfehler“ zurückgegeben. Jetzt wird „Target-Programm befindet sich in einem freigegebenen Ordner und kann nicht gelöscht werden. Die Freigabe des Ordners muss aufgehoben werden, bevor versucht wird, ihn zu löschen.“
* Es wurde ein Problem mit dem Klonprogramm-Endpunkt behoben. Wenn Sie versucht haben, ein Programm zu klonen, das eine DateTime in einem Flussschritt enthält, wird „611, Systemfehler“ zurückgegeben. Jetzt klont es das Programm erfolgreich.
* Es wurde ein Problem mit dem Endpunkt „Programme erstellen“ behoben, durch das Sie versehentlich ein Programm unter einem E-Mail-Programm erstellen konnten (was nicht zulässig ist).
* Es wurde ein Problem mit dem Klonprogramm-Endpunkt behoben. Wenn Sie ein Programm geklont haben, das eine Landingpage enthält, fehlte dem Namen der Landingpage im Zielprogramm ein Unterstrich zwischen Programmname und Landingpage-Name. Z. B. `http://<_pod_\>.marketo.com/lp/<_munchkin_\>/<_program name_\>**_**<_LP name_\>.html`

Veröffentlicht am _2021-05-07_ von _David_

## Zeitlich begrenztes Angebot für Adobe Exchange

Die Unterstützung der Marketo Engage-Partner-Community ist eine der Säulen für den Erfolg unserer Kunden. Wir möchten sicherstellen, dass das Marketo Engage-Integrations-Ökosystem im Exchange Marketplace gut vertreten ist, und haben ein Sonderangebot für LaunchPoint-Partner. Für einen sehr begrenzten Zeitraum, der nicht verlängert wird, bieten wir unseren LaunchPoint-Partnern bis Ende 2022 eine kostenlose Innovate-Partnerschaft im Exchange-Programm (Wert von ca. 15.000 $). Wir haben dieses Angebot entwickelt, um LaunchPoint-Partner dazu zu ermutigen, ihre Integrationslisten im Exchange-Partnerportal zu erstellen, das dann im Adobe Exchange Marketplace öffentlich durchsucht werden kann. um eine vollständige Liste der Vorteile der Innovate-Partnerschaft zu sehen, die Sie bis Dezember 2022 kostenlos erhalten.

1. Wechseln Sie zum [Adobe Exchange Partner Support Center](https://adobeexchangeec.zendesk.com/hc/en-us?mkt_tok=NjA4LURIVi05MTUAAAF-P5lIeVWOuBmKMS_uE_NpgFKtC0ukt7z_ksnq_Sbzb6mzXUuXpqpqQeujtPdZ24WcjMdptygQSR9XrYt_Cw)
1. Klicken Sie oben rechts auf „Anfrage senden“
1. Wählen **unten im Dropdown-Menü Ihr Problem aus** Wählen Sie &quot;Adobe Exchange-Support“
1. Geben **unter „Ihre E** Mail-Adresse“ Ihre E-Mail-Adresse ein
1. Geben Sie **Feld &quot;**&quot; ein
1. Geben Sie **Feld &quot;**&quot; ein
1. Wählen Sie in **Dropdown-** „Support-Typ“ die Option „Programm-Support“
1. Wählen Sie im Dropdown-Menü **Adobe Exchange** Produkt die Option &quot;Adobe Exchange-Programm“ aus
1. Senden Sie das Formular. Unser Team wird sich in Kürze mit Ihnen in Verbindung setzen!

Veröffentlicht am _2021-07-22_ von _David_

## Updates August 2021

Im August 2021 verbessern wir bestehende REST-APIs und beheben mehrere Mängel. Unten finden Sie eine vollständige Liste der Updates.

* Die Bulk Activity Extract-API wurde verbessert, damit Benutzer primäre Attribute für 6 verschiedene Aktivitätstypen filtern können. Weitere Informationen finden Sie unter [Massenaktivität extrahieren](/help/rest-api/bulk-activity-extract.md).
* Um den Benutzern von Marketo Sales Connect besseren Zugriff auf ihre Verkaufsaktivitätsdaten zu ermöglichen, haben wir zusätzliche Attribute für Verkaufsaktivitäten aktiviert. Wir haben die folgenden Attribute den Aktivitäten E-Mail-Versand, E-Mail-Versand öffnen und E-Mail-Versand klicken hinzugefügt:

* Marketo-Vertriebspersonen-ID - Eindeutige ID für Personendatensatz in Sales Connect
* Name der Verkaufskampagne : Name der Verkaufskampagne, wenn die E-Mail Teil einer Verkaufskampagne war
* Verkaufskampagnen-URL : Vertriebs-Connect-URL für eine Verkaufskampagne, wenn die E-Mail Teil einer Verkaufskampagne war
* Name der Verkaufsvorlage : Name der E-Mail-Vorlage in Sales Connect, falls eine Vorlage verwendet wurde
* Verkaufsvorlagen-URL : Vertriebs-Connect-URL für eine E-Mail-Vorlage, wenn eine Vorlage verwendet wurde

### E-Mails

* Der Endpunkt „E-Mails abrufen“ wurde verbessert, indem ein Filter `earliestUpdatedAt`/`latestUpdatedAt` hinzugefügt wurde. Auf diese Weise können Sie das Feld `updatedAt` verwenden, um nur nach einer Teilmenge von E-Mails zu suchen, was eine inkrementelle Synchronisierung ermöglicht.
* Wir haben die Endpunkte „E-Mails abrufen“, „E-Mail nach Namen abrufen“ und „E-Mail nach ID abrufen“ verbessert, um das Abrufen von E-Mail[Einträgen vom Typ „Champion“ und &quot;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/email-tests-champion-challenger/add-an-email-champion-challenger)&quot; zu unterstützen.

### Fehlerbehebungen

* Es wurde ein Problem mit dem Get Users-Endpunkt behoben. Benutzende, denen eine Lizenz für [Marketing-Kalender](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/marketing-calendar/understanding-the-calendar/issue-revoke-a-marketing-calendar-license) erteilt wurde, wurden nicht zurückgegeben. Benutzende des Marketing-Kalenders werden jetzt korrekt zurückgegeben.
* Es wurde ein Problem mit dem Endpunkt Formular senden behoben. Bei doppelten Lead-Einträgen wird das Übermittlungsformular verwendet, um den Fehler „1007, Multiple Lead Match Lookup Criteria“ (Suchkriterien für mehrere Leads) zu erzeugen. Das Übermittlungsformular aktualisiert nun den zuletzt aktualisierten Datensatz auf dieselbe Weise wie die [Forms 2.0-](/help/javascript-api/forms-api-reference.md).
* Mehrere irreführende Fehlermeldungen, die von den Endpunkten „Lead-Feld aktualisieren“ und „Lead-Felder erstellen“ zurückgegeben werden, wurden verbessert. [LM-151890, LM-151888, LM-151889]
* Es wurde ein Problem mit den Endpunkten „Lead-Feld nach Name abrufen“ und „Lead-Felder abrufen“ behoben. Bei beiden Endpunkten können möglicherweise etwas veraltete Informationen zurückgegeben werden. Sie geben jetzt immer aktuelle Informationen zurück.
* Fehlerkorrektur - Bei der [Bulk Extract-API](/help/rest-api/bulk-extract.md) tritt jetzt kein Fehler mehr auf, wenn der HTTP-Header „Bereich“ für den partiellen Abruf verwendet wird, bei dem das letzte Byte im Bereich nicht zurückgegeben wurde.
* Es wurde ein Problem mit dem Endpunkt „Snippet-Metadaten aktualisieren“ behoben. Beim Aktualisieren des Namens oder der Beschreibung des Ausschnitts wurde der Status des Ausschnitts in „Genehmigt mit Entwurf“ geändert. Jetzt bleibt der ausgeschnittene Status nach dem Aktualisieren des Ausschnittnamens oder der Beschreibung unverändert.
* Es wurde ein Problem mit dem Endpunkt E-Mail-Modul hinzufügen behoben. Beim Hinzufügen eines Moduls, das einen Ausschnitt enthielt, wurde „611, Systemfehler“ zurückgegeben. Dadurch wird das Modul nun korrekt zur E-Mail hinzugefügt.
* Es wurde ein Problem mit dem Endpunkt Programm löschen behoben. Beim Löschen eines Programms, das ein lokales In-App-Nachrichten-Asset enthielt, wurde der Fehler „611, Systemfehler“ zurückgegeben. Das Programm wird nun korrekt gelöscht.

Veröffentlicht am _2021-08-22_ von _David_

## Rollout von Munchkin Version 161

Am 7. September 2021 beginnt Munchkin Version 161 mit der Einführung von 10 % der Abonnements mit aktiviertem Munchkin Beta, gefolgt von 50 % am 16. September und 100 % am 30. September. Diese Änderung betrifft Landingpages in Marketo und die Version der Datei munchkin-beta.js, die externen Landingpages bereitgestellt wird, welche aus Abonnements geladen werden, für die die neue Version ausgerollt wurde. Diese Version verwirft vollständig die Lead-Methode von Munchkin Associate, eine Funktion, die die Übermittlung von Personendaten an ein Marketo-Abonnement und den zugehörigen Webbrowsing-Verlauf mit einem Datensatz mit bekannter Person ermöglicht. Der verknüpfte Lead wird zugunsten von moderneren und sichereren Alternativen entfernt, wie der [Forms JS-API](/help/javascript-api/forms-api-reference.md), der Formularübermittlungs-API und der [Associate Lead REST-API](/help/rest-api/leads.md). Wenn Sie oder Ihr Unternehmen diese Methode verwenden, sollten Sie bis zum 12. Oktober 2021, wenn der Rollout der Version Oktober geplant ist, von der Nutzung wegmigrieren. Wenn Sie die Betaversion von Munchkin nicht mehr verwenden möchten, können Sie die Nutzung auf Marketo-Landingpages deaktivieren, indem Sie die Funktion &quot;Munchkin Beta auf Landingpages“ im Menü „Schatztruhe`disabled` auf [&#128279;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features). Wenn Sie Munchkin Beta JavaScript auf externen Web-Seiten bereitgestellt haben und zum standardmäßigen Munchkin-Veröffentlichungskanal wechseln möchten, müssen Sie Ihr Codefragment aktualisieren, um Munchkin JavaScript von munchkin.js anstelle von munchkin-beta.js zu laden.

Veröffentlicht am _2021-08-24_ von _Kenny_

## Ende der Nutzungsdauer von TLS 1.0 und TLS 1.1 für Munchkin Service

Ab dem 21. Oktober 2021 akzeptiert munchkin.marketo.net, das Munchkin JavaScript für Besuchende bereitstellt, keine Verbindungen mehr, die [TLS 1.0 oder TLS 1.1.](https://en.wikipedia.org/wiki/Transport_Layer_Security) verwenden. Diese Verschlüsselungsstandards werden nicht mehr als Teil der Best Practices für die Web-Sicherheit akzeptiert und von modernen Browser-Versionen nicht mehr unterstützt. Es sind keine negativen Auswirkungen infolge dieser Änderung zu erwarten.

Veröffentlicht am _2021-10-04_ von _Kenny_

## Updates Oktober 2021

Im Oktober 2021 verbessern wir bestehende REST-APIs und beheben mehrere Mängel. Unten finden Sie eine vollständige Liste der Updates.

* Der Endpunkt [Formular senden](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) wurde erweitert, sodass er benutzerdefinierte Felder von Programmmitgliedern als Teil der Formularübermittlung unterstützt. Optional können Sie wie [ beschrieben ein Programm als das Programm angeben, dem ein Formular hinzugefügt werden soll, und/oder das Programm, dem benutzerdefinierte Felder für Programmmitglieder hinzugefügt werden ](/help/rest-api/leads.md).
Der Endpunkt [Abrufen von Programmmitgliedern](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getProgramMembersUsingGET) wurde verbessert, sodass Datumsbereichsabfragen basierend auf dem updateAt-Attribut unterstützt werden. Dazu werden Start- und End-Datums-/Uhrzeit-Parameter wie [hier) ](/help/rest-api/program-members.md).
* Die APIs [Lead-Felder](/help/rest-api/leads.md) zur Unterstützung [sensibler Felder](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/field-management/mark-a-field-as-sensitive) wurden verbessert. Die Endpunkte [Lead-Feld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldByNameUsingGET), [Lead-Felder abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldsUsingGET), [Lead-Felder erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) und [Lead-Feld aktualisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updateLeadFieldUsingPOST) unterstützen jetzt das Attribut isSensitive .

### Fehlerbehebungen

* Es wurde ein Problem mit der [User Management](/help/rest-api/user-management.md)-API behoben. Bezieht sich auf Marketo-Benutzende, die für die Verwendung mit [Sales Insight](https://business.adobe.com/products/marketo/sales-insight.html) konfiguriert sind. Diese Benutzer werden jetzt vom Endpunkt [Get Users“ zurückgegeben](https://developer.adobe.com/marketo-apis/api/user/#operation/getUsersUsingGET) und diese Benutzer können jetzt über den Endpunkt [Delete User](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST) gelöscht werden. [LM-155864]
* Es wurde ein Problem mit dem Endpunkt [Rich-Text](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/addRichTextFieldUsingPOST)Feld hinzufügen“ behoben. Beim Hinzufügen eines Rich-Text-Felds, das länger als 65.000 Zeichen ist, zu einer E-Mail, Landingpage, einem Snippet oder Formular wurde der Fehler „611, Systemfehler“ zurückgegeben. Sie gibt jetzt den Fehler „701 zurück. Vorgang kann nicht abgeschlossen werden. &#39;Inhalt&#39; überschreitet eine maximale Länge von 65.535 Byte“.

Veröffentlicht am _2021-10-25_ von _David_

## Updates Januar 2022

Im Januar 2022 verbessern wir bestehende REST-APIs und beheben mehrere Mängel. Unten finden Sie eine vollständige Liste der Updates.

* Die API [Massenextraktion benutzerdefinierter Objekte“ wurde erweitert](/help/rest-api/bulk-custom-object-extract.md) damit Benutzer mithilfe eines Datumsbereichs **updatedAt** filtern können.
* Es wurden Metadaten-APIs für Programmteilnehmerfelder hinzugefügt, mit denen Sie Metadaten für Programmteilnehmerfelder erstellen, aktualisieren und abrufen können. Weitere Informationen finden Sie [Programmmitglieder > Felder](/help/rest-api/program-members.md).
* Es wurden Metadaten-APIs für Unternehmensfelder hinzugefügt, mit denen Sie Metadaten für Unternehmensfelder abrufen können. Weitere Informationen finden Sie [Unternehmen > Felder](/help/rest-api/companies.md).
* Es wurden Metadaten-APIs für Opportunity-Felder hinzugefügt, mit denen Sie Metadaten für Opportunity-Felder abrufen können. Weitere Informationen finden Sie unter [Opportunities > Felder](/help/rest-api/opportunities.md).
* Es wurden APIs für Metadaten von Feldern eines benannten Kontos hinzugefügt, mit denen Sie Metadaten für Felder eines benannten Kontos abrufen können. Weitere Informationen finden Sie [Benannte Konten > Felder](/help/rest-api/named-accounts.md).
* Die Endpunkte für Feldmetadaten wurden aktualisiert, um eine neue boolesche Eigenschaft **isApiCreated** zurückzugeben, die angibt, ob ein Feld von der REST-API erstellt wurde oder nicht.

### Fehlerbehebungen

* Es wurde ein Latenzproblem zwischen dem Zeitpunkt des Aufrufs des Endpunkts [Lead-Felder erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) und dem Zeitpunkt behoben, zu dem das neu erstellte Lead-Feld in der Smart-Liste verfügbar war. [LM-152838]
* Es wurde ein Problem mit dem Endpunkt [Lead-Felder erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) behoben, bei dem erstellte Felder nicht in der Dropdown-Liste Formularfelder verfügbar waren, die zum [Hinzufügen von Feldern zum Formular](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/forms/creating-a-form/add-a-field-to-a-form) in der Marketo Engage-Benutzeroberfläche verwendet wurde. [LM-158243]
* Es wurde ein Problem mit dem Endpunkt [Kampagnen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET) behoben, bei dem auslösbare Kampagnen nicht zurückgegeben wurden, wenn der Parameter isTriggerable=true angegeben wurde. [LM-158283]
* Es wurde ein Problem behoben[ bei dem der Endpunkt „Leads nach ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteTokenByNameUsingPOST) abrufen“ in bestimmten Fällen den Fehler „611, Systemfehler“ zurückgab. [LM-157214]
* Bereinigung mehrerer Fehlermeldungen, die vom Endpunkt [Lead-Feld aktualisieren](/help/rest-api/leads.md) zurückgegeben wurden. [LM-151886, LM-151888, LM-151889]

Veröffentlicht am _2022-01-27_ von _David_

## Updates März 2022

Im März 2022 verbessern wir bestehende REST-APIs und beheben mehrere Mängel. Unten finden Sie eine vollständige Liste der Updates.

* Wir haben das Feld **actionResult** zur Exportdatei hinzugefügt, die von der Bulk Activity Extract-API erstellt wird. Dieses Feld kann verwendet werden, um zwischen erfolgreichen, übersprungenen und fehlgeschlagenen Aktivitäten zu unterscheiden.
* Wir haben das Feld **isOpenTrackingDisabled** zu Antworten aus der [E-Mail-API](/help/rest-api/emails.md) hinzugefügt. Mit diesem Feld kann bestimmt werden, ob die Funktion [Öffnungstracking deaktivieren](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-editor-v2-0-overview) aktiviert ist.
* Wir haben zwei Endpunkte hinzugefügt, mit denen Sie Programm-Tags selektiv verwalten können. Mit [ Endpunkt ](/help/rest-api/programs.md)Programm-Tags aktualisieren“ können Sie ein Programm-Tag selektiv aktualisieren. Mit [ Endpunkt ](/help/rest-api/programs.md)Löschen von Programm-Tags“ können Sie ein Programm-Tag selektiv löschen.
* Wir haben den Parameter **isExecutable** zum Endpunkt [Klonen von Smart Campaign](/help/rest-api/smart-campaigns.md) hinzugefügt. Mit diesem Parameter können Sie ein Programm als ausführbares Programm klonen.
* Wir haben das Feld **headStart** zur [Programme-API](/help/rest-api/programs.md) hinzugefügt. Auf diese Weise können Sie die Einstellung &quot;[-Start“ für E](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/head-start-for-email-programs)Mail-Programme erstellen, aktualisieren und abrufen.

### Fehlerbehebungen

* Es wurde ein Problem mit dem Endpunkt [Dynamischen Inhalt von E-Mails abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) behoben. Beim Versuch, Betreffzeilen mit dynamischem Inhalt aus E-Mails abzurufen, die fehlerhafte Vorlagenbeziehungen hatten, wurde der Fehler 709 „API erlaubt nur Vorgänge für E-Mails mit einer Vorlage“ zurückgegeben. Der Endpunkt gibt jetzt den dynamischen Inhalt zurück. [LM-152331]
* Es wurde ein Problem mit [ Endpunkt „Leads ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)&quot; behoben. Wenn Sie externalSalesPersonId verwenden, um die Verkaufsperson mithilfe von externalSalesPersonId mit einem Lead zu verknüpfen, und action = createDuplicate verwenden, tritt die Verkaufspersonenverknüpfung nicht auf. [LM-158990]

### Adobe IMS-Integration

* Diejenigen, die in [Adobe IMS](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) integriert wurden, können nicht alle [Marketo User Management-APIs ](/help/rest-api/user-management.md). Die folgenden Endpunkte geben einen Fehler zurück, wenn sie auf Marketo-Instanzen aufgerufen werden, die in Adobe IMS integriert wurden: [Benutzer ](https://developer.adobe.com/marketo-apis/api/user/#operation/inviteUserUsingPOST), [Eingeladenen Benutzer per ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#operation/getInvitedUserUsingGET), [Benutzerattribute aktualisieren](https://developer.adobe.com/marketo-apis/api/user/#operation/updateUserAttributeUsingPOST), [Benutzer löschen](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST) und [Eingeladenen Benutzer löschen](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteInvitedUserUsingPOST). Als Ersatz sollten die [Adobe User Management](https://developer.adobe.com/umapi/)APIs verwendet werden.

Veröffentlicht am _2022-03-14_ von _David_

## Updates vom Mai 2022-

Im Mai 2022 verbessern wir bestehende REST-APIs und beheben mehrere Mängel. Unten finden Sie eine vollständige Liste der Updates.

* Wir haben die Möglichkeit hinzugefügt, [Unternehmen](/help/rest-api/companies.md), [Opportunity](/help/rest-api/opportunities.md) und [Vertriebspersonen](/help/rest-api/sales-persons.md) abzurufen, wenn entweder [SFDC Sync](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) in Ihrer Marketo Engage-Instanz aktiviert sind.
* Der Endpunkt [Dynamischen Inhalt für E-Mails abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) wurde aktualisiert, damit Sie [Dynamischen Inhalt](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/using-dynamic-content-in-an-email) aus einer E-Mail-Betreffzeile abrufen können. Dies funktioniert unabhängig davon, ob die angegebene E-Mail mit einer E-Mail-Vorlage verknüpft ist.

`POST /rest/asset/v1/form/{id}/field/State.json?values=[{"label":"Alaska"},{"value":"AK"},{"label":"West Virginia","value":"WV"},{"label":"Wyoming","value":"WY"}]`

* Der Endpunkt [Hinzufügen von Formularfeldsichtbarkeitsregeln](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllProgramMemberFieldsUsingGET) wurde aktualisiert, sodass Sie mehrere Vergleichswerte für den Typ **isNot** hinzufügen können [Unsichtbarkeitsregeln](/help/rest-api/forms.md). Siehe folgendes Beispiel:

`POST /rest/asset/v1/form/{id}/field/LastName/visibility.json?visibilityRule={"ruleType":"show","rules":[{"subjectField":"LastName","operator":"isNot","values":["A","B","C"]}`

### Fehlerbehebungen

* Es wurde ein Problem mit dem Endpunkt [Formular senden](/help/rest-api/leads.md) behoben, das beim Übergeben von „null“ für ein Attribut im Parameter [leadFormFields](/help/rest-api/leads.md) auftrat und einen Fehler „611, Systemfehler“ zurückgab. Jetzt wird korrekt der Fehler „1003, Formularvalidierung fehlgeschlagen“ zurückgegeben. [LM-162213]

Veröffentlicht am _2022-05-09_ von _David_

## Updates vom August 2022

Im August 2022 erweitern wir bestehende REST-APIs. Unten finden Sie eine vollständige Liste der Updates.

Wir haben mehrere neue Filter hinzugefügt, die beim Aufrufen des Auftrags-Endpunkts „Abonnenten des Exportprogramms erstellen“ verwendet werden können. Beachten Sie, dass viele der Filter in Kombination miteinander verwendet werden können, um den extrahierten Datensatz zu verfeinern.

* Der **programIds**-Filter kann verwendet werden, um bis zu 10 Programmkennungen anzugeben, die den Durchsatz verbessern können.
* Der **isExhausted**-Filter kann verwendet werden, um Datensätze nach Personen [Personen mit erschöpftem Inhalt) ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content).
* Der **NurtureCadence**-Filter kann verwendet werden, um Datensätze auf der Grundlage [Interaktionsprogrammkadenz“ ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/program-flow-actions/change-engagement-program-cadence).
* Der Filter **statusNames** kann zum Filtern von Datensätzen nach einem oder mehreren [Programmstatus](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/understanding-program-membership) verwendet werden.
* Der **updatedAt**-Filter kann verwendet werden, um Datensätze basierend auf einem Datumsbereich zu filtern.

### Ankündigungen

* Das Verhalten des Endpunkts [Identität](https://developer.adobe.com/marketo-apis/api/identity/#operation/identityUsingGET) hat sich geändert. Wenn Sie den Endpunkt aufrufen und keinen Parameter **access_token“**, wird der Fehler „603, Access denied“ zurückgegeben. Zuvor wurde der Fehler „600, Leeres Zugriffstoken“ zurückgegeben. Beachten Sie, dass der Fehler „600, Leeres Zugriffstoken“ veraltet ist.

Veröffentlicht am _2022-09-03_ von _David_

## Updates Oktober 2022

Im Oktober 2022 erweitern wir bestehende REST-APIs. Unten finden Sie eine vollständige Liste der Updates.

* Wir haben die API [Massenimport von Leads](/help/rest-api/bulk-lead-import.md) verbessert, um das Hinzufügen von Leads zu Datensätzen von Vertriebspersonen während des Importvorgangs zu unterstützen. Dazu fügen Sie das Feld **externalSalesPersonId** in die Importdatei ein.
* Es wurde ein Problem mit dem Endpunkt [Lead-Felder erstellen](/help/rest-api/leads.md) behoben, der beim Erstellen von Feldern vom Typ Bewertung auftrat. Diese Felder waren nicht für die Verwendung in der Flussaktion [Score ändern](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/change-score) in der Marketo Engage-Benutzeroberfläche verfügbar. [LM-166815]

### Ankündigungen

* Das Programmmitgliedschaftsattribut `acquiredBy` wurde geändert, um aktualisiert werden zu können.

Veröffentlicht am _2022-10-18_ von _David_

## Bevorstehende Änderung der Marketo Forms REST-API

Ab der Version 2022.R2, die derzeit für die Woche des 24. März 2023 geplant ist, geben die Adobe Marketo Engage Forms Asset-APIs durchgängig nur den Namen des Formulars ohne vorangestellten Programmnamen zurück, unabhängig davon, ob das Formular ein untergeordnetes Element eines Programms ist oder nicht. Durch diese Änderung wird das Verhalten der Forms-API in Bezug auf Asset-Namen mit dem Rest der Adobe Marketo Engage Asset-APIs konsistent. Um Service-Unterbrechungen zu vermeiden, sollten Sie alle Integrationen überprüfen, die Marketo Engage Forms-APIs verwenden, und mit Ihren Integratoren zusammenarbeiten, um festzustellen, ob Änderungen erforderlich sind, um dies vor dieser bevorstehenden Änderung zu berücksichtigen. Von den Forms-APIs zurückgegebene Namen haben inkonsistent ein Präfix für Programmnamen für Formulare, die untergeordnete Elemente von Programmen in Marketing-Aktivitäten sind. Beispielsweise kann ein Formular mit dem Namen „Form 1“, das ein untergeordnetes Element von „Programm 1“ war, von der API wie folgt zurückgegeben werden: Programm 1.Formular 1 oder Formular 1 Ab Version 2022.R2 wird der Name eines Formulars immer ohne einen vorangestellten Programmnamen zurückgegeben. Unter Verwendung des gleichen Beispiels würde der Name immer wie folgt lauten: Form 1

Veröffentlicht am _2022-11-04_ von _Kenny_

## Updates Januar 2023

Im Januar 2023 haben wir eine API-bezogene Verbesserung an der Admin-Benutzeroberfläche vorgenommen und geben zwei Ankündigungen ab. Unten finden Sie eine vollständige Liste der Updates.

Admin-Benutzeroberfläche

### Massenauszug von Blei

* Die Marketo Engage Admin-Benutzeroberfläche wurde erweitert, sodass Sie die tägliche Massenextraktions-API-Kapazitätszuweisung für Ihr Abonnement anzeigen können. Darüber hinaus können Sie die Kapazitätsnutzung durch den API-Benutzer in den letzten 7 Tagen anzeigen. Weitere Informationen finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/settings/bulk-export-api-information).

### Fehlerbehebungen

* Es wurde ein Problem mit [ Endpunkt „Opportunities ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST)&quot; behoben. In bestimmten Fällen wurde keine Aktivität vom Typ „Aus Opportunity entfernen“ generiert. [LM-172208]

### Ankündigungen

* Siehe [diesen Artikel](https://nation.marketo.com/t5/product-documents/upcoming-change-to-marketo-rest-api/ta-p/331698) über die Marketo-Community bezüglich der REST-API und einer Änderung der HTTP-Antwortnachricht [Reason Phrase](https://www.rfc-editor.org/rfc/rfc7230#section-3.1.2).
* Das Programmmitgliedschaftsattribut **statusReason** wurde geändert, um aktualisiert werden zu können.

Veröffentlicht am _2023-01-21_ von _David_
