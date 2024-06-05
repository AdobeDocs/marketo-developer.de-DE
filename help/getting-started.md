---
title: Erste Schritte
description: Erste Schritte mit Marketo Engage-APIs
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
source-git-commit: 82bea1ab3d0d83a8867bb7efefb828ce2d92747c
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 1%

---

# Erste Schritte

Marketo Engage ist eine Marketing-Automatisierungsplattform, mit der Marketing-Experten personalisierte Multi-Channel-Programme und -Kampagnen für potenzielle Kunden und Kunden verwalten können. Die Marketo Engage-Plattform kann mithilfe von Integrationspunkten erweitert werden. Unten finden Sie die Kernentitäten und ihre Beziehungen.

Die folgenden Objekte sind nicht über die REST-API verfügbar, wenn die native Synchronisierung aktiviert ist: Unternehmen, Chancen, Angebotsrolle, Vertriebsmitarbeiter

![Datenmodell](assets/data_model.png)

## Person (Leads)

Menschen sind die Grundlage jeder Marketing-Automatisierungsplattform. Innerhalb von Marketo werden alle nicht verkauften Personendatensätze als Leads bezeichnet, unabhängig davon, ob sie als Leads, Interessenten, Verdächtige, Kontakte usw. aus der Sicht des Verkaufs bezeichnet werden. Das Lead-Objekt enthält eine Reihe von [Standardfelder](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadFieldsUsingGET) wie E-Mail, Vorname und Nachname. Dem Lead-Objekttyp können zusätzliche Felder hinzugefügt werden, um die mit Datensätzen im System verknüpften Informationstypen zu erweitern. Benutzerdefinierte Attribute können genauso gelesen und geschrieben werden wie Standardfelder. Eine vollständige Liste der Felder finden Sie in der Marketo **[!UICONTROL Admin]** > **[!UICONTROL Feldverwaltung]** Menü. Leads werden in Marketo durch das ID-Feld eindeutig identifiziert. Andere eindeutige Schlüssel müssen extern vom System erzwungen werden.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [SOAP](soap-api/leads.md), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Aktivitäten

Leads bieten verschiedene Möglichkeiten, mit Ihrer Organisation zu interagieren. Ein Lead kann eine Seite auf der Website Ihres Unternehmens besuchen, an einer Messe teilnehmen oder ein Whitepaper herunterladen. Jede dieser Aktionen kann in Marketo erfasst werden, damit ein Marketing-Experte besser verstehen kann, welche Aktivitäten ein Interessent durchgeführt hat und wann er zeitnahe und relevante Nachrichten koordinieren kann. Aktivitäten werden immer mit Leads durch leadId verknüpft.

Sie können Ihre eigenen benutzerdefinierten Aktivitäten definieren. Nachdem Sie eine benutzerdefinierte Aktivität erstellt und veröffentlicht haben, können Sie benutzerdefinierte Aktivitäten über die Marketo-API hinzufügen. Weitere Informationen zu benutzerdefinierten Aktivitäten finden Sie [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [SOAP](soap-api/activities.md), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programme und Kampagnen

Ein Programm ist der Mechanismus, mit dem ein Marketing-Experte all seine verschiedenen Marketing-Maßnahmen von einem zentralen Ort aus organisiert. Ein Beispiel für ein Programm ist eine E-Mail-Nachricht. Ein Lead kann mehrere Aktionen/Aktivitäten im Zusammenhang mit einem bestimmten Programm ausführen, die mit dem Programm verknüpft werden. Dies wird als Lead-Progression bezeichnet. Ein Beispiel für die Progression eines E-Mail-Schnellbearbeitungsprogramms würde aufzeichnen, wenn ein Lead per E-Mail versendet wird, wenn die Person die E-Mail geöffnet hat oder ob sie durch einen Link in der E-Mail geklickt hat.

Kampagnen werden erstellt, um einen bestimmten Zweck und ein bestimmtes Ziel innerhalb eines Programms zu erfüllen. Ein Beispiel für eine Kampagne könnte darin bestehen, eine Gruppe von Leads einzuschränken und ihnen die E-Mail-Veröffentlichung zu senden oder einen Vertriebsmitarbeiter über ein Follow-up zu benachrichtigen, wenn ein Lead innerhalb des E-Mail-Schnellprogramms auf einen Link klickt.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns), [SOAP](soap-api/getcampaignsforsource.md)

## Tags

Tags sind eine Möglichkeit, Daten zu Berichtszwecken zu gruppieren. Diese Kennungen bieten die Möglichkeit, Daten zu kategorisieren und zu definieren, wie Sie über Ihr Programm Berichte erstellen möchten, um die Effektivität und den ROI des Programms zu verstehen.

Als Marketo-Administrator haben Sie die Möglichkeit, erforderliche und optionale Tagtypen zu erstellen, die zur Auswahl verfügbar sind, wenn ein Marketo-Benutzer ein Programm erstellt. Mögliche Werte für jeden dieser Tag-Typen werden von Ihnen definiert und spiegeln wider, wie Ihr Unternehmen benutzerdefinierte Tags zu Berichtszwecken verwenden möchte.

Sie können beispielsweise einen benutzerdefinierten Tag-Typ &quot;Region&quot;mit mehreren Tag-Werten erstellen (z. B. Nordosten, Südosten), mit dem Sie analysieren können, welche Region die meisten Leads generiert. Oder Sie können beispielsweise einen &quot;Inhaber&quot;-Tag-Typ erstellen, mit dem Sie bewerten und verstehen können, welche Programmeigentümer (z. B. Maria, David oder John) den größten Einfluss auf die Erstellung von Leads und Chancen haben. Weitere Informationen zu Tags finden Sie unter [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/), [SOAP](soap-api/gettags.md)

## Listen

Mit Listen können Marketing-Experten eine Sammlung von Leads organisieren. In Marketo gibt es zwei Arten von Listen: &quot;Statisch&quot;und &quot;intelligent&quot;. Eine statische Liste ist eine feste Liste von Leads, die ein Marketingexperte nach Wahl hinzufügen oder entfernen kann. Eine intelligente Liste ist eine dynamische Sammlung von Leads, die auf einer Reihe von festgelegten Eigenschaften basiert. Ein Beispiel für eine intelligente Liste wäre &quot;Alle Leads, die die Preisseite auf unserer Website besucht haben&quot;. Diese intelligente Liste wächst weiter, wenn mehr Interessenten die Preisseite besuchen. Weitere Informationen zu Listen finden Sie unter [here](https://experienceleague.adobe.com/en/docs/marketo/using/home).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists), [SOAP](soap-api/getimporttoliststatus.md)

## Opportunitys

Marketingexperten liefern Leads zum Verkauf in Form einer Chance. Eine Chance stellt ein potenzielles Verkaufsgeschäft dar und ist mit einem Lead oder Kontakt und einer Organisation in Marketo verbunden. Eine Opportunity-Rolle ist die Schnittmenge zwischen einem bestimmten Lead und einer Organisation. Die Opportunity-Rolle bezieht sich auf die Funktion eines Leads innerhalb der Organisation.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities), [SOAP](soap-api/getmobjects.md)

## Unternehmen

Eine Organisation, manchmal auch als Konto in Marketo bezeichnet, bezieht sich auf die Organisation, zu der eine Person gehört. Bei der Verwendung von ROI-Berichten in Marketo oder von Revenue Cycle Analytics (RCA) ist es wichtig, Personen mit ihrer Organisation und ihren Chancen zu verknüpfen, damit die richtige ROI-Zuordnung ermittelt werden kann.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies), [SOAP](soap-api/leads.md)

## Assets

Assets beziehen sich auf Landingpages, E-Mails, Formulare und Bilder, die in einem Programm verwendet werden. Assets können entweder lokal in einem bestimmten Programm oder global sein. Globale Assets sind in allen Programmen verfügbar.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Token

Token ermöglichen es Marketingexperten, Nachrichten mit Assets zu personalisieren und Logik in Fluss-Aktionen hinzuzufügen. Es gibt Token für das gesamte System, Programme, Leads und Unternehmen. Ein Beispiel für ein Lead-Token ist {{lead.First Name}}. Dieses Token kann in eine E-Mail eingefügt werden, um den Vornamen des Leads anzuzeigen.

Auf Programm- oder Ordnerebene definierte Token werden in Marketo als &quot;My Tokens&quot;bezeichnet. Meine Token können einen von drei Typen sein: lokal, vererbt oder überschrieben.

Meine Token, die lokal in einem bestimmten Kampagnenordner oder Programm erstellt werden, stehen diesem spezifischen Programm oder Kampagnenordner (lokal) zur Verfügung. Meine Token, die auf Kampagnenordnerebene erstellt werden, stehen für alle Programme zur Verfügung, die in diesem Kampagnenordner enthalten sind (geerbt). Meine Token, die auf Programmebene mit benutzerdefinierten Werten geändert werden, ändern nicht den übergeordneten My Token -Wert des Tokens auf der Programmebene des -Ordners (überschrieben).

Meine Token verwenden die Namenskonvention {{my.My Token}}, with the word "my" added to the beginning of the token name. For example, if you create a Date type My Token with the name EventDate, the name of the token is {{my.EventDate}}. Weitere Informationen zu My Tokens finden Sie unter [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens), [SOAP](soap-api/getcampaignsforsource.md)

## Benutzerdefinierte Objekte

Ein benutzerdefiniertes Marketo-Objekt ermöglicht die Erstellung einer Eins-zu-viele- oder n-zu-viele-Beziehung (Edge-Bridge-Edge) zwischen Ihren Marketo-Leads und den benutzerdefinierten Objektdatensätzen. Nachdem Sie ein benutzerdefiniertes Marketo-Objekt erstellt und veröffentlicht haben, können Sie CRUD-Vorgänge für das benutzerdefinierte Objekt über die Marketo-API durchführen. Weitere Informationen zur Erstellung benutzerdefinierter Objekte finden Sie [here](https://experienceleague.adobe.com/en/docs/marketo/using/home). Wenn dem benutzerdefinierten Objekt neue Datensätze hinzugefügt werden, können Sie einen Smart-List-Trigger verwenden, um zu antworten. Sie können benutzerdefinierte Objektdaten auch als Filter in Smart-Listen (Segmentierung) oder in E-Mails mit [Email Scripting](email-scripting.md).

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects), [SOAP](soap-api/custom-objects.md)

## Vertriebsmitarbeiter

Datensätze von Vertriebsmitarbeitern und Lead-Beziehungen können in Marketo verwaltet werden, wenn keine native CRM-Integration aktiviert ist. Diese Datensätze enthalten grundlegende Informationen über die Vertriebsperson, wie z. B. Name, E-Mail und Auftragstitel, die zum Filtern und Token in Marketo verwendet werden können, wenn ein Lead im Besitz eines einzigen ist. Die Beziehung zu einer Vertriebsperson wird auf Lead-Ebene über das Feld &quot;externalSalesPersonId&quot; verwaltet, das über das [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) API.

Verwandte APIs: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
