---
title: Tags
feature: REST API, Tags
description: Verwalten Sie Tags für Programme in Marketo.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 2%

---

# Tags

[Tags-Endpunktverweis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Tags sind benutzerdefinierte Felder für Programme. Jedes Tag kann für einen oder mehrere Programmtypen gelten und je nach Definition des Tags entweder erforderlich oder optional sein. Tags können auch eine Liste der zulässigen Werte bereitstellen, aus denen ausgewählt werden muss.

## Anfrage

Tags werden mit dem standardmäßigen Asset-Muster abgefragt, haben jedoch keinen Endpunkt für &quot;Nach ID&quot;. Die Liste der zulässigen Werte für ein Tag wird nur zurückgegeben, wenn das Tag nach Namen abgefragt wird.

### Abrufen von Tags

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

## Aktualisierung

Mit dem Endpunkt [Programm-Tag aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) können Sie den Wert für einen bestimmten Tag-Typ aktualisieren. Der Endpunkt akzeptiert Pfadparameter `id` und `tagType`, die die Programm-ID und den zu aktualisierenden Tag-Typ angeben. Ein `tagValue` -Abfrageparameter wird verwendet, um den neuen Wert für den Tag-Typ anzugeben. Alle Parameter sind erforderlich.

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

Tags können gebündelt mit dem Endpunkt [Programmmetadaten aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) aktualisiert werden. Ein Beispiel dafür finden Sie [hier](programs.md#update).

## Löschen

Mit dem Endpunkt [Programm-Tag löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) können Sie einen nicht erforderlichen Tag-Typ löschen. Der Endpunkt akzeptiert Pfadparameter `id` und `tagType`, die die Programm-ID und den zu löschenden Tag-Typ angeben.

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
