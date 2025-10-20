---
title: Erste Schritte
description: Erste Schritte mit Marketo Engage-APIs und Datenmodellen, einschließlich Leads, Aktivitäten, Programmen, Tags, Listen, REST-Anleitungen und Hinweisen zur Einstellung von SOAP.
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
source-git-commit: 5f2dcb4864cdcd110ba9f199ef9c86dcee522335
workflow-type: tm+mt
source-wordcount: '1355'
ht-degree: 1%

---

# Erste Schritte

Marketo Engage ist eine Marketing-Automatisierungsplattform, mit der Marketing-Experten personalisierte Multi-Channel-Programme und -Kampagnen für Interessenten und Kunden verwalten können. Die Marketo Engage-Plattform kann mithilfe von Integrationspunkten erweitert werden. Nachfolgend finden Sie die Kernentitäten und ihre Beziehungen.

>[!NOTE]
>Die SOAP-API wird nicht mehr unterstützt und ist nach dem 31. Januar 2026 nicht mehr verfügbar. Alle neuen Entwicklungen sollten mit der Marketo [REST-API](./rest-api/rest-api.md) durchgeführt werden, und bestehende Services sollten bis zu diesem Datum migriert werden, um Unterbrechungen im Service zu vermeiden. Wenn Sie über einen Service verfügen, der die SOAP-API verwendet, finden Sie im SOAP-API[Migrationshandbuch](./soap-api/migration.md) Informationen zur Migration.
>

Wenn entweder die native SFDC- oder MS Dynamics CRM-Verbindung in einer Marketo Engage-Instanz aktiviert ist, sind die folgenden Objekte schreibgeschützt: Unternehmen, Opportunity, Opportunity-Rolle, Vertriebsperson

![Datenmodell](assets/data_model.png)

## Person (Leads)

Menschen sind das Fundament jeder Marketing-Automatisierungsplattform. Innerhalb von Marketo werden alle Nicht-Vertriebspersonendatensätze als Leads bezeichnet, unabhängig davon, ob sie aus Vertriebsperspektive als Leads, Interessenten, Verdächtige, Kontakte usw. gekennzeichnet sind. Das Lead-Objekt enthält eine Reihe von Standardfeldern, wie E-Mail, Vorname und Nachname. Zusätzliche Felder können zum Lead-Objekttyp hinzugefügt werden, um die Informationstypen zu erweitern, die mit Datensätzen im System verknüpft sind. Benutzerdefinierte Attribute können genau wie die Standardfelder gelesen und geschrieben werden. Eine vollständige Liste der Felder finden Sie im Menü **[!UICONTROL Admin]** > **[!UICONTROL Feldverwaltung]** von Marketo. Leads werden in Marketo durch das Feld ID eindeutig identifiziert. Andere eindeutige Schlüssel müssen extern vom System erzwungen werden.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Aktivitäten

Leads interagieren auf verschiedene Weise mit Ihrer Organisation. Ein Lead kann eine Seite auf der Website Ihres Unternehmens besuchen, an einer Messe teilnehmen oder ein Whitepaper herunterladen. Jede dieser Aktionen kann in Marketo erfasst werden, damit Marketing-Experten besser verstehen können, welche Aktivitäten ein Lead wann ausgeführt hat, damit sie zeitnahe und relevante Nachrichten koordinieren können. Aktivitäten werden durch die Lead-ID immer wieder mit Leads verbunden.

Sie können Ihre eigenen benutzerdefinierten Aktivitäten definieren. Nachdem Sie eine benutzerdefinierte Aktivität erstellt und veröffentlicht haben, können Sie benutzerdefinierte Aktivitäten über die Marketo-API hinzufügen. Weitere Informationen zu benutzerdefinierten Aktivitäten finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programme und Kampagnen

Ein Programm ist der Mechanismus, mit dem ein Marketing-Experte alle seine verschiedenen Arten von Marketing-Maßnahmen von einem zentralen Ort aus organisiert. Ein Beispiel für ein Programm ist ein E-Mail-Versand. Ein Lead kann mehrere Aktionen/Aktivitäten im Zusammenhang mit einem bestimmten Programm ausführen, die mit dem Programm verknüpft werden. Dies wird als Lead-Progression bezeichnet. Ein Beispiel für einen Fortschritt eines E-Mail-Blastprogramms wäre der Versand einer E-Mail an einen Lead, der Zeitpunkt, zu dem die Person die E-Mail geöffnet hat, oder die Frage, ob sie durch einen Link in der E-Mail geklickt hat.

Kampagnen werden erstellt, um einen bestimmten Zweck und ein bestimmtes Ziel innerhalb eines Programms zu erfüllen. Ein Beispiel für eine Kampagne könnte darin bestehen, eine Gruppe von Leads einzugrenzen und ihnen die E-Mail-Nachricht zu senden oder einen Vertriebsmitarbeiter zur Nachverfolgung zu benachrichtigen, wenn ein Lead über einen Link im E-Mail-Programm klickt.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

## Tags

Tags sind eine Möglichkeit, Daten zu Berichtszwecken zu gruppieren. Diese Kennungen bieten die Möglichkeit, Daten zu kategorisieren und zu definieren, wie Sie Berichte zu Ihrem Programm erstellen möchten, um die Effektivität und den ROI des Programms zu verstehen.

Als Marketo-Administrator können Sie erforderliche und optionale Tag-Typen erstellen, die ausgewählt werden können, wenn ein Marketo-Benutzer ein Programm erstellt. Mögliche Werte für jeden dieser Tag-Typen werden von Ihnen definiert und spiegeln wider, wie Ihr Unternehmen benutzerdefinierte Tags zu Berichtszwecken verwenden möchte.

Beispielsweise können Sie einen benutzerdefinierten Tag-Typ „Region“ mit mehreren Tag-Werten (z. B. Nordosten, Südosten) erstellen, um zu analysieren, welche Region die meisten Leads generiert. Sie können auch einen Tag-Typ „Verantwortlicher“ erstellen, mit dem Sie beurteilen und verstehen können, welche Programm-Verantwortlichen (z. B. Maria, David oder John) die größte Auswirkung auf die Erstellung von Leads und Opportunities haben. Weitere Informationen zu Tags finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Listen

Listen ermöglichen es einem Marketing-Experten, eine Sammlung von Leads zu organisieren. In Marketo gibt es zwei Arten von Listen: statische Listen und intelligente Listen. Eine statische Liste ist eine feste Liste von Leads, die ein Marketer nach Belieben hinzufügen oder entfernen kann. Eine Smart-Liste ist eine dynamische Sammlung von Leads, die auf einer Reihe spezieller Merkmale basieren. Ein Beispiel für eine Smart-Liste wäre „Alle Leads, die die Preisseite auf unserer Website besucht haben“. Diese Smart-Liste wächst weiter, je mehr Leads die Preisseite besuchen. Weitere Informationen zu Listen finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/home).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

## Opportunitys

Marketing-Experten liefern Leads zu Verkäufen in Form einer Opportunity. Eine Opportunity stellt einen potenziellen Verkaufsabschluss dar und ist mit einem Lead oder Kontakt und einer Organisation in Marketo verbunden. Eine Opportunity-Rolle ist die Schnittmenge zwischen einem bestimmten Lead und einer Organisation. Die Opportunity-Rolle bezieht sich auf die Funktion eines Leads innerhalb der Organisation.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

## Firmen

Eine Organisation, in Marketo manchmal auch als Konto bezeichnet, bezieht sich auf die Organisation, der eine Person angehört. Bei der Verwendung der ROI-Berichterstellung in Marketo oder der Umsatzzyklusanalyse (RCA) ist es wichtig, Personen mit ihrer Organisation und ihren Opportunities zu verknüpfen, damit die richtige ROI-Attribution bestimmt werden kann.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

## Assets

Assets bezieht sich auf Landingpages, E-Mails, Formulare und Bilder, die innerhalb eines Programms verwendet werden. Assets kann entweder lokal in einem bestimmten Programm oder global sein. Globale Assets sind für jedes Programm verfügbar.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Token

Token ermöglichen es einem Marketing-Experten, Nachrichten mit Assets zu personalisieren und Logik in Flussaktionen hinzuzufügen. Es gibt Token für das gesamte System, Programme, Leads und Unternehmen. Ein Beispiel für ein Lead-Token ist {{lead.First Name}}. Dieses Token kann in einer E-Mail platziert werden, um den Vornamen des Leads anzuzeigen.

Auf Programm- oder Ordnerebene definierte Token werden in Marketo als „Meine Token“ bezeichnet. Meine Token können einen von drei Typen aufweisen: lokal, vererbt oder überschrieben.

Meine Token, die lokal in einem bestimmten Kampagnenordner oder -programm erstellt wurden, stehen diesem bestimmten Programm oder Kampagnenordner (lokal) zur Verfügung. Meine Token, die auf der Kampagnenordnerebene erstellt werden, sind für alle Programme in diesem Kampagnenordner verfügbar (geerbt). Meine Token, die auf Programmebene mit benutzerdefinierten Werten geändert werden, ändern den übergeordneten Wert meines Tokens des Tokens auf Programmebene nicht (überschrieben).

Meine Token verwenden die Namenskonvention {{my.My Token}}, wobei das Wort „my“ am Anfang des Token-Namens hinzugefügt wird. Wenn Sie beispielsweise einen Datumstyp My Token mit dem Namen EventDate erstellen, wird der Name des Tokens {{my.EventDate}}. Weitere Informationen zu „Meine Token“ finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

## Benutzerdefinierte Objekte

Ein benutzerdefiniertes Marketo-Objekt ermöglicht die Erstellung einer Eins-zu-Viele- oder Viele-zu-Viele-Beziehung (Edge-Bridge-Edge) zwischen Ihren Marketo-Leads und den benutzerdefinierten Objektdatensätzen. Nachdem Sie ein benutzerdefiniertes Marketo-Objekt erstellt und veröffentlicht haben, können Sie über die Marketo-API CRUD-Vorgänge für das benutzerdefinierte Objekt durchführen. Weitere Informationen zur Erstellung benutzerdefinierter Objekte finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/home). Wenn dem benutzerdefinierten Objekt neue Datensätze hinzugefügt werden, können Sie einen Smart-Listen-Trigger verwenden, um zu antworten. Sie können benutzerdefinierte Objektdaten auch als Filter in Smart-Listen (Segmentierung) oder in E-Mails mit [E-Mail-Skripterstellung](email-scripting.md) verwenden.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects)

## Vertriebspersonal

Kundendatensätze und Lead-Beziehungen können in Marketo verwaltet werden, wenn keine native CRM-Integration aktiviert ist. Diese Datensätze enthalten grundlegende Informationen über den Vertriebspersonal, wie Name, E-Mail und Tätigkeitsbezeichnung, die zum Filtern von und Token in Marketo verwendet werden können, wenn ein Lead einem Lead gehört. Die Beziehung zu einem Vertriebspersonal wird auf Lead-Ebene über das Feld „externalSalesPersonId“ verwaltet, das über die API [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) aktualisiert werden muss.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
