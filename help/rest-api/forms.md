---
title: Formulare
feature: REST API, Forms
description: Erstellen und Verwalten von Formularen über die API.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 2%

---

# Formulare

[Endpunkt-Referenz für Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Endpunkt-Referenz für Formularfelder](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

Marketo Forms verfügt über einen komplexen Satz von Endpunkten, der die vollständige Steuerung der Formularverwaltung über Remote-Systeme ermöglicht. Die Struktur von Formularen kann komplex sein, da es viele verschiedene Typen von Objekten gibt, die als Teil eines Formulars verwaltet werden müssen: Forms, Felder, Feldsätze, Sichtbarkeitsregeln und Folgeseitenregeln.

## Abfrage

Forms unterstützt die Standardmethoden zum Abrufen von Assets [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) und [durch Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Jede Formularantwort enthält alle Eigenschaften mit Ausnahme der Feldliste.

### Nach ID

[Formular nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) akzeptiert eine `id` als Pfadparameter und gibt einen Formulardatensatz zurück.

```
GET /rest/asset/v1/form/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Nach Name

[Formular nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) akzeptiert eine `name` als Pfadparameter und gibt einen Formulardatensatz zurück.

```
GET /rest/asset/v1/form/byName.json?name=newForm
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Durchsuchen

[Forms abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) Formulare funktionieren wie andere Asset-API-Durchsuchen-Endpunkte und ermöglichen das optionale Filtern nach `status`, `maxReturn` und `offset`. Status kann „Genehmigt“, „Mit Entwurf genehmigt“ oder „Entwurf“ sein.

```
GET /rest/asset/v1/forms.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "645d#154e3d499ac",
    "result": [
        {
            "id": 227,
            "name": "aKAUVDfbsX",
            "description": "",
            "createdAt": "2016-05-18T20:36:20Z+0000",
            "updatedAt": "2016-05-18T20:36:20Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO227B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        },
        {
            "id": 695,
            "name": "AoMXgfFbma",
            "description": "",
            "createdAt": "2016-05-19T18:50:40Z+0000",
            "updatedAt": "2016-05-19T18:50:40Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO695B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 565,
                "folderName": "WfUvYmlcyT"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

### Feldliste

Das Abrufen der Feldliste für ein Formular erfolgt auf Formularbasis.

```
GET /rest/asset/v1/form/{id}/fields.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2165#154eee00d01",
    "result": [
        {
            "id": "FirstName",
            "label": "First Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "LastName",
            "label": "Last Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 1,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Email",
            "label": "Email Address:",
            "dataType": "email",
            "validationMessage": "Must be valid email. <span class='mktoErrorDetail'>example@yourdomain.com</span>",
            "rowNumber": 2,
            "columnNumber": 0,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Profiling",
            "dataType": "profiling",
            "rowNumber": 3,
            "columnNumber": 0
        }
    ]
}
```

Beim Bearbeiten von Feldern oder deren Verhalten innerhalb eines Formulars sollte die Feldliste immer abgerufen werden, bevor Sie Änderungen vornehmen. Dadurch wird sichergestellt, dass beim Aktualisieren oder Löschen die richtige Feld-ID angegeben wird.

### Feldtypen

| UI-Typ | API-Name |
|--------------|-----------------|
| Kontrollkästchen | Kontrollkästchen |
| Optionsfeld | Funk |
| Textbereich | Textbereich |
| Auswahlliste | Auswahlliste |
| Zeichenfolge | string |
| E-Mail | E-Mail |
| Datum | Datum |
| Zahl | number |
| Double | double |
| Telefon | Telefon |
| URL | URL |
| Währung | currency |
| Kontrollkästchen | single_checkbox |
| Schieberegler | Bereich |


### Abhängigkeiten

Der Endpunkt [Formular abrufen von](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) nimmt eine Formular-`id` als Pfadparameter und gibt die Liste der Assets zurück, die vom Formular abhängen. Forms kann von den folgenden Asset-Typen verwendet werden: Landingpages, Smart-Listen, Smart-Kampagnen, Berichte, E-Mail-Programme.

```
GET /rest/asset/v1/form/{id}/usedBy.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fdf4#17285b25038",
    "warnings": [],
    "result": [
        {
            "id": 1038,
            "name": "LP Redirect Rules Program.LP Test 01",
            "type": "Landing Page",
            "status": "approved",
            "updatedAt": "2020-02-23T01:31:21Z+0000"
        }
    ]
}
```

## Erstellen und aktualisieren

Beim [Erstellen eines ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST) gibt es nur zwei erforderliche Felder: den übergeordneten Ordner des Formulars und den Namen des Formulars. Alle anderen Parameter sind optional und haben einen Standardwert. Wenn das Formular erstellt wird, enthält es drei Standardfelder: Vorname, Nachname, E-Mail.

```
POST /rest/asset/v1/forms.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=newForm&description=test&folder={"type": "Folder","id": 293}&language=French
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

Forms werden [aktualisiert](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) mit einem ähnlichen Aufruf über ihre ID. Während der Erstellung oder Aktualisierung sind alle grundlegenden Stilparameter aufrufbar und bearbeitbar, sodass Sie ändern können, wie das Formular dem Endbenutzer angezeigt wird.

```
POST /rest/asset/v1/form/736.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=updated name&description=This is a test for updateapi&language=English&progressiveProfiling=true&locale=en_US
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154e3cf6efe",
    "result": [
        {
            "id": 736,
            "name": "updated name",
            "description": "This is a test for update api",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:28:23Z+0000",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

Das bekannte Verhalten der Besucher- und Dankeseiten kann nicht über die Formularaufrufe „Erstellen“ oder „Aktualisieren“ geändert werden und der Zugriff muss über die entsprechenden Endpunkte erfolgen.

## Feldmetadaten

Um Felder, die zu einem Formular gehören, ordnungsgemäß hinzuzufügen oder zu bearbeiten, müssen Sie die Liste der gültigen Felder für die Zielinstanz abrufen. Feldinteraktionen werden immer auf Grundlage der ID-Eigenschaft des Felds durchgeführt, die für jedes Element im Ergebnis angezeigt wird.

Bei Lead-Feldern erfolgt dies über den Endpunkt [Verfügbare Formularfelder abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) und umfasst den Datentyp und die Standard-Metadaten für das Feld, wenn es einem Formular hinzugefügt wird.

```
GET /rest/asset/v1/form/fields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "176ca#167a9808f4c",
    "warnings": [],
    "result": [
        {
            "id": "AnnualRevenue",
            "isRequired": false,
            "dataType": "currency"
        },
        {
            "id": "City",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Company",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Country",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Description",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 32000,
            "visibleRows": 2
        },
        {
            "id": "Email",
            "isRequired": false,
            "dataType": "email"
        },
        {
            "id": "Fax",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "FirstName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Industry",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LastName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LeadSource",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "MobilePhone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "NumberOfEmployees",
            "isRequired": false,
            "dataType": "int"
        },
        {
            "id": "Phone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "PostalCode",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Rating",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Salutation",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "Mr.,Ms.,Mrs.,Dr.,Prof."
        },
        {
            "id": "State",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "AK::AK,AL::AL,AR::AR,AZ::AZ,CA::CA,CO::CO,CT::CT,DE::DE,FL::FL,GA::GA,HI::HI,IA::IA,ID::ID,IL::IL,IN::IN,KS::KS,KY::KY,LA::LA,MA::MA,MD::MD,ME::ME,MI::MI,MN::MN,MO::MO,MS::MS,MT::MT,NC::NC,ND::ND,NE::NE,NH::NH,NJ::NJ,NM::NM,NV::NV,NY::NY,OH::OH,OK::OK,OR::OR,PA::PA,RI::RI,SC::SC,SD::SD,TN::TN,TX::TX,UT::UT,VA::VA,VT::VT,WA::WA,WI::WI,WV::WV,WY::WY"
        },
        {
            "id": "Street",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 2000,
            "visibleRows": 2
        },
        {
            "id": "Title",
            "isRequired": false,
            "dataType": "picklist"
        }
    ]
}
```

Für benutzerdefinierte Felder für Programmteilnehmer rufen Sie [Verfügbare Felder für Programmteilnehmer abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  Endpunkt zum Abrufen der benutzerdefinierten Felddatentypen und Standardmetadaten des Programmmitglieds. Um diese Felder in einem Formular verwenden zu können, muss sich das Formular unter einem Programm (nicht in Design Studio) befinden. Landingpages, die Formulare enthalten, die diese Felder verwenden, müssen sich auch unter einem Programm befinden (können nicht in Design Studio gespeichert oder in Design Studio geklont werden).

```
GET /rest/asset/v1/form/programMemberFields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "109c6#16fa0b9c51a",
    "warnings": [],
    "result": [
        {
            "id": "pMCFCustomField01",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "pMCFCustomField02",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "myPMCF",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        }
    ]
}
```

### Bearbeitungsfeld

Jedes Formular enthält eine bearbeitbare Liste von Feldern, die den Endbenutzenden angezeigt wird, wenn sie geladen werden. Jedes Feld wird einzeln über die jeweiligen Endpunkte zur Feldliste hinzugefügt, aktualisiert oder gelöscht.

[Feld hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) erfordert nur die ID des übergeordneten Formulars und die fieldId des Felds. Alle anderen Felder sind entweder leer oder haben Standardwerte, die auf ihrem Datentyp und den Feldmetadaten basieren. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON.

```
POST /rest/asset/v1/form/{id}/fields.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
fieldId=NumberOfEmployees&maxLength=125&defaultValue=this is default&required=true&fieldWidth=100&validationMessage=hey, you there?&label=employee count&hintText=Hint me&minValue=10
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1826e#154f41b214c",
    "result": [
        {
            "id": "NumberOfEmployees",
            "label": "employee count",
            "fieldWidth": 100,
            "dataType": "number",
            "defaultValue": "this is default",
            "validationMessage": "hey, you there?",
            "rowNumber": 5,
            "columnNumber": 0,
            "required": true,
            "formPrefill": true,
            "fieldMetaData": {
                "minValue": 10,
                "maxValue": null
            },
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "hintText": "Hint me"
        }
    ]
}
```

Bei Aktualisierungen können dieselben Felder wie beim Hinzufügen eines Felds bearbeitet werden. Entsprechend sind die Formular-ID und die fieldId erforderlich, mit dem Unterschied, dass die fieldId ein Pfadparameter und kein Abfrageparameter bei der Durchführung von Aktualisierungen ist.

```
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
label=enter the last name here
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5634#15508303abb",
    "result": [
        {
            "id": "LastName",
            "label": "enter the last name here",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        }
    ]
}
```

Im obigen Beispiel aktualisieren wir das Feld LastName , das eine einfache Zeichenfolge ist. Einige Formularfelder sind komplexer. Beispiel: Das Feld Anrede ist ein Feldtyp „Auswählen“, der eine Liste von Elementen und einen Standardwert enthält. Wenn Sie ein Feld vom Typ „Auswählen“ hinzufügen oder aktualisieren und eine der Auswahlmöglichkeiten nicht auf „true“ `isDefault`, hat die erste Auswahl keinen Wert und erhält die Bezeichnung „Auswählen…“

![Anrede](assets/form-field-salutation.png)

Um die Listenelemente zu aktualisieren, sieht das Format des Parameters „values“ wie folgt aus:

```
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
values=[{"label":"Select...","value":"","isDefault":true,"selected":true}, {"label":"MR","value":"MR"}, {"label":"MS","value":"MS"}, {"label":"MRS","value":"MRS"}, {"label":"DR","value":"DR"}, {"label":"PROF","value":"PROF"}]
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "71fd#1588d9d1b0c",
  "result": [
    {
      "id": "Salutation",
      "label": "Salutation:",
      "dataType": "select",
      "validationMessage": "This field is required.",
      "rowNumber": 3,
      "columnNumber": 0,
      "required": false,
      "formPrefill": true,
      "fieldMetaData": {
        "multiSelect": false,
        "values": [
          {
            "label": "Select...",
            "value": "",
            "isDefault": true,
            "selected": true
          },
          {
            "label": "MR",
            "value": "MR"
          },
          {
            "label": "MS",
            "value": "MS"
          },
          {
            "label": "MRS",
            "value": "MRS"
          },
          {
            "label": "DR",
            "value": "DR"
          },
          {
            "label": "PROF",
            "value": "PROF"
          }
        ],
        "visibleLines": 1
      },
      "visibilityRules": {
        "ruleType": "alwaysShow"
      }
    }
  ]
}
```

 

Um zu bestimmen, wie Sie ein komplexes Formularfeld formatieren, sehen Sie sich die Antwort von Feld zum Formular hinzufügen an.

### Feld wird neu angeordnet

Die Felder in einem Formular müssen alle als Einheit über den Endpunkt [Formularfeldpositionen ändern](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST) neu angeordnet werden. Der Endpunkt erfordert einen Parameter namens `positions`, ein JSON-Array von Objekten mit drei Elementen:

- columnNumber
- rowNumber
- fieldName (bezieht sich auf die ID des Felds)

Felder in einem Formular sind in einer tabellenähnlichen Oberfläche mit bis zu drei Spalten und bis zu zehn Zeilen angeordnet. Sowohl Zeile als auch Spalte werden von 0 indiziert, sodass die erste Zeile und die erste Spalte durch die Übergabe von 0 angezeigt werden. Alle Felder müssen eine eindeutige Position einnehmen.

Wenn das Zielfeld auch eine Feldgruppe ist, sollte sein Datensatz innerhalb des Positions-Arrays auch einen Parameter namens fieldList enthalten, ein Array von Objekten, die dieselben ColumnNumber-, rowNumber- und fieldName-Member enthalten. Die Feldgruppe selbst wird für ihre Position in der übergeordneten Liste als einzelnes Feld behandelt, während ihre Unterfelder entsprechend den angegebenen Positionen im fieldList-Parameter positioniert werden.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"FirstName"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"}, {"columnNumber":0,"rowNumber":2, "fieldName":"Email"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bb18#15508ef9c04",
    "result": [
        {
            "id": 764
        }
    ]
}
```

### RTF

Rich-Text-Felder werden über einen [separaten Endpunkt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) von Lead-Feldern hinzugefügt. Der Feldinhalt wird als multipart/form-data übergeben. Es sollte als HTML-Inhalt strukturiert sein, der kein Skript, Meta-Tags oder Link-Tags enthält.

```
POST /rest/asset/v1/form/{id}/richText.json
```

```
Content-Type: multipart/form-data; boundary=---------------------------9051914041544843365972754266
-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="text"
Content-Type: text/html
<div>Fancy Rich Text Component</div>
-----------------------------9051914041544843365972754266--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "82c8#154f423bf5c",
    "result": [
        {
            "id": "SHRtbFRleHRfMjAxNi0wNS0yN1QxNDozNDoyNC4xMTVa",
            "labelWidth": 260,
            "dataType": "htmltext",
            "rowNumber": 8,
            "columnNumber": 0,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "text": "<div>Fancy Rich Text Component</div>"
        }
    ]
}
```

### Feldsatz

Marketo Forms verfügen über eine optionale Komponente, die als Feldsätze bezeichnet wird. Feldsätze sind Gruppen von Feldern, die in der Feldliste der obersten Ebene für die Zwecke der Verschiebung und Behandlung durch Sichtbarkeitsregeln als ein einzelnes Feld behandelt werden. Wenn beispielsweise ein Feld für Kompatibilitätsanforderungen vorhanden ist und ein Client „Ja“ auswählt, kann ein Feldsatz angezeigt werden, der Felder für HIPAA- und PCI-Kompatibilitätsanforderungen enthält.

Felder in Feldsätzen sind für das Formular als Ganzes eindeutig, sodass doppelte Felder möglicherweise nicht sowohl in der Liste der übergeordneten Felder des Formulars als auch in einer untergeordneten Feldgruppe enthalten sind. Feldsätze werden über den Endpunkt [Feldsatz zu Formular hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) hinzugefügt und erscheinen dann im Ergebnis von [Felder für Formular abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Felder werden einem Feldsatz hinzugefügt, indem sie über „Feldpositionen aktualisieren“ in die [ des Feldsatzes verschoben ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Für diese Endpunkte werden Daten als POST x-www-form-urlencoded übergeben, nicht als JSON.

## Sichtbarkeitsregel

Jedes Feld kann über eine Reihe von Sichtbarkeitsregeln verfügen, die bestimmen, ob das Feld von einem Besucher gesehen werden kann, je nachdem, welche Werte er in das Formular eingegeben hat. Die Regeln stellen einen Vergleich zwischen dem Wert eines subjectField im Formular und einer Liste von Werten in der Regel her. Jedes Feld kann einen einzigen Sichtbarkeitsregeltyp, Einblenden, Ausblenden oder Immer anzeigen und anschließend eine Liste von auszuwertenden Regeln enthalten. Die Regeln werden von oben nach unten ausgewertet, und die erste Regel, die als „true“ ausgewertet wird, ist die, die angewendet wird.

Das Ändern der Sichtbarkeitsregeln ist eine destruktive Aktualisierung.

```
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
visibilityRule={"ruleType":"show", "rules":[{"subjectField": "LastName", "operator": "isNotEmpty", "values": [], "altLabel": "Email:"}]}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "ab4a#15509030601",
    "result": [
        {
            "formFieldId": "Email",
            "ruleType": "show",
            "rules": [
                {
                    "subjectField": "LastName",
                    "operator": "isNotEmpty",
                    "values": [],
                    "altLabel": "Email:"
                }
            ]
        }
    ]
}
```

Eine vollständige Liste der verfügbaren Operatoren finden Sie auf der Endpunkt-Referenzseite für [Hinzufügen von Sichtbarkeitsregeln für Formularfelder](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Nachbereitung

Marketo-Formulare können ein dynamisches Verhalten im Anschluss an Seiten aufweisen, bei dem Regeln für die Umleitung zu einer bestimmten Seite oder das Verweilen auf der aktuellen Seite basierend auf dem Inhalt bestimmter Felder bei der Übermittlung angewendet werden können. Regeln können synonym als Dankeseitenregeln oder Folgeseitenregeln bezeichnet werden. Diese Regeln werden als JSON-Array mit den Membern `followupType`, `followupValue`, `operator`, `subjectField`, `values` und `default` dargestellt. `default` ist ein boolescher Wert, bei dem nur ein Datensatz im Array „true“ sein kann. Wenn ein Besucher für keine anderen Regeln qualifiziert ist, wird die als Standard festgelegte Regel verwendet. `followupType` kann entweder lp oder url sein, wobei lp eine Marketo-Landingpage-ID für `followupValue` angibt und url eine URL zu einer anderen Seite angibt. Der Operator wird verwendet, um den Wert des Betrefffelds mit der Liste der bereitgestellten Werte zu vergleichen.

## Senden-Schaltfläche

Die Gestaltung der Senden-Schaltfläche des Formulars wird mit dem Endpunkt [Senden-Schaltfläche aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST) verwaltet. ButtonPosition, buttonStyle, label und waitLabel (die Beschriftung, die angezeigt wird, während die Übermittlung aussteht) können geändert werden.

Dies ist ein destruktives Update.

## Genehmigung

Wie die meisten anderen Assets folgen Formulare einem Modell mit Entwurfsgenehmigung, bei dem es eine Entwurfsversion und/oder eine genehmigte Version geben kann. Wenn Aktualisierungen auf ein Formular angewendet werden, werden sie immer zuerst auf die Entwurfsversion angewendet und erst dann live angezeigt, wenn das Formular genehmigt wurde. Bei der Genehmigung eines Formulars wird die aktuelle Entwurfsversion verwendet und die genehmigte Version (sofern vorhanden) wird durch den Entwurf ersetzt. Wenn das Formular aus der Live-Phase entfernt werden muss, muss es zunächst nicht genehmigt werden, wodurch alle aktuellen Entwürfe gelöscht und die genehmigte Version in einen reinen Entwurfsstatus zurückgestuft wird. Die Genehmigung für Forms sollte immer aufgehoben werden, bevor versucht wird, eine Löschung durchzuführen.

## Progressive Profilerstellung

Wenn die progressive Profilerstellung für ein Formular aktiviert ist, wird eine Feldgruppe mit dem Namen „Profilerstellung“ in die Feldliste aufgenommen. Um Felder zur progressiven Profilliste hinzuzufügen oder daraus zu entfernen, müssen Sie den Endpunkt Feldpositionen aktualisieren verwenden. Dieser Endpunkt führt destruktive Aktualisierungen durch, sodass alle Felder im Formular in jeder Anfrage enthalten sein müssen. Im folgenden Beispiel wird das Feld „Telefon“ zur Liste der progressiven Profile hinzugefügt.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"Email"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"},{"columnNumber":0,"rowNumber":2,"fieldName":"Company"},{"columnNumber":0,"rowNumber":3,"fieldName":"Website"},{"columnNumber":0,"rowNumber":4,"fieldName":"Profiling","fieldList":[{"columnNumber":0,"rowNumber":0,"fieldName":"Phone"}]}]
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d6a#164190dbdf2",
    "result": [
        {
            "id": 1031
        }
    ]
}
```
