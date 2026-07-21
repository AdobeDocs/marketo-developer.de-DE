---
title: Tags
feature: REST API, Tags
description: Abfragen von Tag-Typen, Abrufen zulässiger Werte nach Namen, Aktualisieren oder Löschen von Programm-Tags in Marketo über die REST Asset-API, mit Anfragebeispielen.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
TQID: https://experienceleague.adobe.com/zjdyfoofVWytE0Q-K4lk598jmleTSFOD7tSRqeAHsjk
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 2%

---

# Tags

[Tags-Endpunktreferenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags)

Tags sind benutzerdefinierte Felder für Programme. Ein Tag kann auf einen oder mehrere Programmtypen angewendet werden und kann erforderlich oder optional sein. Mit einem Tag kann auch eine Liste zulässiger Werte definiert werden, aus denen Benutzerinnen und Benutzer auswählen müssen.

## Abfrage

Fragt Tags nach dem Standard-Asset-Muster ab. Tags haben keinen ById-Endpunkt. Um die zulässigen Werte für ein Tag abzurufen, fragen Sie das Tag nach Namen ab.

### Tags abrufen

```http
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

```http
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

Verwenden Sie den Endpunkt [Programm-Tag aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST), um den Wert für einen Tag-Typ zu aktualisieren. Alle Parameter sind erforderlich:

- Der `id` Pfadparameter gibt die Programm-ID an.
- Der Parameter &quot;`tagType` path“ gibt den zu aktualisierenden Tag-Typ an.
- Der `tagValue` Abfrageparameter gibt den neuen Wert an.

```http
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

Um mehrere Tags zu aktualisieren, verwenden Sie den Endpunkt [Programmmetadaten aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST). Siehe dazu das Beispiel im [Abschnitt Programmaktualisierung](programs.md#update).

## Löschen

Verwenden Sie den Endpunkt [Programm-Tag löschen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/deleteProgramUsingPOST), um einen nicht erforderlichen Tag-Typ zu löschen. Der Parameter &quot;`id` path“ gibt die Programm-ID und der Parameter &quot;`tagType` path“ den Tag-Typ an, der gelöscht werden soll.

```http
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
