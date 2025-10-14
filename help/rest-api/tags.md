---
title: Tags
feature: REST API, Tags
description: Abfragen von Tag-Typen, Abrufen zulässiger Werte nach Namen, Aktualisieren oder Löschen von Programm-Tags in Marketo über die REST Asset-API, mit Anfragebeispielen.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 2%

---

# Tags

[Tags-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Tags sind benutzerdefinierte Felder für Programme. Jedes Tag kann sich auf einen oder mehrere Programmtypen beziehen und kann entweder erforderlich oder optional sein, je nachdem, wie das Tag definiert wurde. Tags können auch eine Liste zulässiger Werte bereitstellen, aus denen Sie zur Verwendung auswählen müssen.

## Abfrage

Tags werden mit dem Standard-Asset-Muster abgefragt, haben jedoch keinen Endpunkt für nach ID. Die Liste der zulässigen Werte für ein Tag wird nur zurückgegeben, wenn das Tag anhand des Namens abgefragt wird.

### Tags abrufen

```
GET /rest/asset/v1/tagTypes.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1488a#1504ecfccf8",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true
        },
        {
            "tagType": "AAA2 Required Event Tag Type",
            "applicableProgramTypes": "[event]",
            "required": true
        },
        {
            "tagType": "AAA3 Not Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": false
        }
    ]
}
```

### Nach Name

```
GET /rest/asset/v1/tagType/byName.json?name=AAA1 Required Tag Type
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "8a44#1504ed0da2f",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true,
            "allowableValues": "[AAA1 RT1, AAA1 RT2, AAA1 RT3, AAA1 RT4]"
        }
    ]
}
```

## Update

Der Endpunkt [Programm-Tag](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) ermöglicht es Ihnen, den Wert für einen bestimmten Tag-Typ zu aktualisieren. Der Endpunkt akzeptiert einen `id` und `tagType` Pfadparameter, die die Programm-ID und den zu aktualisierenden Tag-Typ angeben. Ein `tagValue` Abfrageparameter wird verwendet, um den neuen Wert für den Tag-Typ anzugeben. Alle Parameter sind erforderlich.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}.json?tagValue=David
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fd84#17f84a885a6",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```

Tags können massenweise mit dem Endpunkt [Programmmetadaten aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) aktualisiert werden. Ein Beispiel dafür finden Sie [hier](programs.md#update).

## Löschen

Mit [&#x200B; Endpunkt &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST)Programm-Tag löschen“ können Sie einen nicht erforderlichen Tag-Typ löschen. Der Endpunkt akzeptiert `id` und `tagType` Pfadparameter, die die Programm-ID und den zu löschenden Tag-Typ angeben.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d998#17f84ad36a7",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```
