---
title: "Forms"
feature: REST API, Forms
description: "Erstellen und verwalten Sie Formulare über die API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 2%

---


# Formulare

[Forms-Endpunktverweis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Formularfelder-Endpunktverweis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

Marketo forms verfügt über einen komplexen Satz von Endpunkten, die die vollständige Steuerung der Formularverwaltung über Remote-Systeme ermöglichen. Die Formularstruktur kann komplex sein, da es viele verschiedene Objekttypen gibt, die als Teil eines Formulars verwaltet werden müssen: Forms, Felder, Feldsätze, Sichtbarkeitsregeln und Regeln für Follower-Seiten.

## Anfrage

Forms unterstützt die Standardmethoden zum Abrufen von Assets, [by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET), und [durch Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Jede Formularantwort enthält alle zugehörigen Eigenschaften mit Ausnahme der Feldliste.

### Nach ID

[Formular nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) nimmt ein Formular an `id` als Pfadparameter und gibt einen Formulardatensatz zurück.

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

[Formular nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) nimmt ein Formular an `name` als Pfadparameter und gibt einen Formulardatensatz zurück.

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

[Forms abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) Formulare funktionieren wie andere Asset-API-Durchsuchen-Endpunkte und ermöglichen eine optionale Filterung nach `status`, `maxReturn`, und `offset`. Der Status kann wie folgt lauten: genehmigt, mit Entwurf genehmigt oder Entwurf.

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

Das Abrufen der Feldliste für ein Formular erfolgt pro Formular.

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

Beim Bearbeiten von Feldern oder deren Verhalten innerhalb eines Formulars sollte die Feldliste immer abgerufen werden, bevor Sie Bearbeitungen durchführen. Dadurch wird sichergestellt, dass Sie beim Aktualisieren oder Löschen die richtige Feld-ID angeben.

### Feldtypen

| UI-Typ | API-Name |
|--------------|-----------------|
| Kontrollkästchen | Kontrollkästchen |
| Optionsfeld | radio |
| Textbereich | Textbereich |
| Auswahlliste | picklist |
| Zeichenfolge | string |
| E-Mail | E-Mail |
| Datum | Datum |
| Zahl | number |
| Doppelt | double |
| Telefon | Telefon |
| URL | URL |
| Währung | currency |
| Kontrollkästchen | single_checkbox |
| Schieberegler | Bereich |


### Abhängigkeiten

Die [Formular abrufen, das von verwendet wird](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) Endpunkt nimmt ein Formular an `id` als Pfadparameter und gibt die Liste der Assets zurück, die vom Formular abhängen. Forms kann von den folgenden Asset-Typen verwendet werden: Landingpages, Smart-Listen, Smart-Kampagnen, Berichte, E-Mail-Programme.

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

## Erstellen und Aktualisieren

Wann [Erstellen eines Formulars](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST) Es gibt nur zwei erforderliche Felder: den übergeordneten Ordner des Formulars, den Namen des Formulars. Alle anderen Parameter sind mit dem Standardwert optional. Wenn das Formular erstellt wird, enthält es drei Standardfelder: Vorname, Nachname, E-Mail.

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

Forms sind [aktualisiert](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) mit einem ähnlichen Aufruf über ihre ID. Während der Erstellung oder Aktualisierung sind alle grundlegenden Formatierungsparameter verfügbar und bearbeitbar, sodass Sie die Anzeige des Formulars für den Endbenutzer ändern können.

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

Das Seitenverhalten des bekannten Besuchers und Danksagens kann nicht über die Formularaufrufe zum Erstellen oder Aktualisieren von Formularen geändert werden und muss über die jeweiligen Endpunkte aufgerufen werden.

## Feldmetadaten

Um Felder, die zu einem Formular gehören, ordnungsgemäß hinzuzufügen oder zu bearbeiten, müssen Sie die Liste der gültigen Felder für die Zielinstanz abrufen. Feldinteraktionen werden immer auf der Grundlage der ID-Eigenschaft des Felds durchgeführt, die für jedes Element im Ergebnis angezeigt wird.

Bei Lead-Feldern wird dies mithilfe der Variablen [Verfügbare Formularfelder abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) -Endpunkt und enthält den Datentyp und die Standard-Metadaten für das Feld, wenn es zu einem Formular hinzugefügt wird.

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

Rufen Sie für benutzerdefinierte Felder des Programmmitglieds auf [Verfügbare Felder für Programmteilnehmer im Formular abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  Endpunkt zum Abrufen der benutzerdefinierten Felddatentypen für Programmmitglieder und der Standardmetadaten. Um diese Felder in einem Formular zu verwenden, muss sich das Formular unter einem Programm (nicht in Design Studio) befinden. Landingpages, die Formulare enthalten, die diese Felder verwenden, müssen sich auch unter einem Programm befinden (sie dürfen sich nicht in Design Studio befinden oder in Design Studio geklont werden).

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

Jedes Formular enthält eine bearbeitbare Liste von Feldern, die dem Endbenutzer beim Laden angezeigt werden. Jedes Feld wird einzeln über seine jeweiligen Endpunkte hinzugefügt, aktualisiert oder aus der Feldliste gelöscht.

[Feld hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) erfordert nur die ID des übergeordneten Formulars und die fieldId des Felds. Alle anderen Felder sind entweder leer oder haben Standardwerte, die auf ihrem Datentyp und ihren Feldmetadaten basieren. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON.

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

Aktualisierungen können alle Felder bearbeiten, die mit dem Hinzufügen eines Felds übereinstimmen, und erfordern in ähnlicher Weise die Formular-ID und die fieldId, mit der Ausnahme, dass fieldId bei Aktualisierungen ein Pfadparameter und kein Abfrageparameter ist.

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

Im obigen Beispiel aktualisieren wir das Feld LastName , das eine einfache Zeichenfolge ist. Einige Formularfelder sind komplexer. Beispielsweise ist das Anrede -Feld ein Feldtyp &quot;select&quot;, der eine Liste von Elementen und einen Standardwert enthält. Wenn Sie ein Auswahlfeld hinzufügen oder aktualisieren, es sei denn, Sie legen eine der Optionen fest, dass ein `isDefault` den Wert &quot;true&quot;hat, hat die erste Auswahl keinen Wert und wird mit &quot;Select..&quot;beschriftet.

![Anrede](assets/form-field-salutation.png)

Um die Listenelemente zu aktualisieren, sieht der Parameter &quot;values&quot;folgendermaßen aus:

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

 

Um zu bestimmen, wie ein komplexes Formularfeld formatiert werden soll, sehen Sie sich die Antwort aus &quot;Feld zum Formular hinzufügen&quot;an.

### Feld neu anordnen

Die Felder in einem Formular müssen als Einheit neu angeordnet werden über die [Ändern der Position von Formularfeldern](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST) -Endpunkt. Der Endpunkt erfordert einen Parameter namens `positions`, ein JSON-Array von Objekten mit drei Elementen:

- columnNumber
- rowNumber
- fieldName (bezieht sich auf die ID des Felds)

Die Felder in einem Formular sind in einer tabellenähnlichen Benutzeroberfläche mit bis zu drei Spalten und bis zu zehn Zeilen angeordnet. Sowohl die Zeile als auch die Spalte werden von 0 indexiert, sodass die erste Zeile und die erste Spalte beide durch Übergabe einer 0 angezeigt werden. Alle Felder müssen eine eindeutige Position einnehmen

Wenn es sich bei dem Zielfeld auch um ein Feldset handelt, sollte der Datensatz im Positionsarray auch einen Parameter namens fieldList, ein Array von Objekten, die dieselben Member columnNumber, rowNumber und fieldName enthalten, enthalten. Das Feldset selbst wird als einzelnes Feld für seine Position in der übergeordneten Liste behandelt, während seine Unterfelder entsprechend den angegebenen Positionen im Parameter fieldList positioniert werden.

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

Rich-Text-Felder werden über eine [separater Endpunkt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) aus Lead-Feldern. Der Feldinhalt wird als mehrteilige Formulardaten übergeben. Sie sollte als HTML-Inhalt strukturiert sein, der keine Skript-, Meta-Tags oder Link-Tags enthält.

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

Marketo forms bietet eine optionale Komponente namens &quot;fieldsets&quot;. Fieldsets sind Gruppen von Feldern, die zum Zweck der Bewegung und der Behandlung durch Sichtbarkeitsregeln in der obersten Felderliste als ein Feld behandelt werden. Wenn beispielsweise ein Feld für Compliance-Anforderungen vorhanden ist und ein Client &quot;yes&quot;auswählt, wird möglicherweise ein Feldset mit Feldern für HIPAA- und PCI-Compliance-Anforderungen angezeigt.

Felder in Feldsätzen sind für das gesamte Formular eindeutig. Daher sind doppelte Felder nicht sowohl in der Liste der übergeordneten Felder des Formulars als auch in einem untergeordneten Feldsatz enthalten. Fieldsets werden über das [Feldsatz zum Formular hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) -Endpunkt und werden dann im Ergebnis von [Felder für Formular abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Felder werden einem Feldsatz hinzugefügt, indem sie über in die fieldList des Felds verschoben werden. [Feldpositionen aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Für diese Endpunkte werden Daten als POST x-www-form-urlencoded und nicht als JSON übergeben.

## Sichtbarkeitsregel

Jedes Feld kann über einen Satz von Sichtbarkeitsregeln verfügen, die bestimmen, ob das Feld von einem Besucher in Abhängigkeit von den Werten, die er in das Formular eingegeben hat, gesehen werden kann. Die Regeln vergleichen den Wert eines im Formular vorhandenen subjectField mit einer Liste von Werten, die in der Regel angegeben sind. Jedes Feld kann einen Typ von Sichtbarkeitsregel aufweisen, anzeigen, ausblenden oder immer einblenden und dann eine Liste von auszuwertenden Regeln. Die Regeln werden von oben nach unten ausgewertet, und die erste Regel, die &quot;true&quot;ergibt, wird angewendet.

Das Ändern von Sichtbarkeitsregeln ist eine destruktive Aktualisierung.

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

Eine vollständige Liste der verfügbaren Operatoren finden Sie auf der Endpunktreferenzseite für [Anzeigeregeln für Formularfelder hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Follower

Marketo-Formulare weisen möglicherweise ein dynamisches Seitenverhalten auf, bei dem Regeln zum Weiterleiten auf eine bestimmte Seite oder zum Bleiben auf der aktuellen Seite basierend auf dem Inhalt der angegebenen Felder bei der Übermittlung angewendet werden können. Regeln können synonym als Regeln für Dankeseite oder Regeln für Follower-Seite bezeichnet werden. Diese Regeln werden als JSON-Array mit den Mitgliedern dargestellt `followupType`, `followupValue`, `operator`, `subjectField`, `values`, und `default`. `default` ist ein boolescher Wert, für den nur ein Datensatz im Array wahr sein kann. Wenn ein Besucher für keine anderen Regeln qualifiziert ist, wird die als Standard festgelegte Regel verwendet. `followupType` kann entweder lp oder url sein, wobei lp eine Marketo-Landingpage-ID für `followupValue`und url zeigt eine URL zu einer anderen Seite an. Der Operator wird verwendet, um den Wert des Betrefffelds mit der Liste der bereitgestellten Werte zu vergleichen.

## Senden-Schaltfläche

Die Formatierung der Senden-Schaltfläche des Formulars wird mit dem [Senden-Schaltfläche aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST) -Endpunkt. Die Schaltflächen &quot;buttonPosition&quot;, &quot;buttonStyle&quot;, &quot;label&quot;und &quot;waitLabel&quot;(die Beschriftung, die beim Senden angezeigt wird, steht aus) können geändert werden.

Dies ist eine destruktive Aktualisierung.

## Genehmigung

Wie die meisten anderen Assets folgen Formulare einem Entwurf-genehmigten Modell, bei dem es eine Entwurfsversion und/oder eine genehmigte Version geben kann. Wenn Aktualisierungen auf ein Formular angewendet werden, werden sie immer zuerst auf die Entwurfsversion angewendet und erst dann live angezeigt, wenn das Formular genehmigt wurde. Die Genehmigung eines Formulars nimmt die aktuelle Entwurfsversion an und ersetzt die genehmigte Version, sofern vorhanden, durch den Entwurf. Wenn das Formular live heruntergeladen werden muss, muss es zunächst nicht genehmigt werden. Dadurch werden alle aktuellen Entwürfe gelöscht und die genehmigte Version in den Status &quot;Nur Entwurf&quot;gedeutet. Forms sollte immer nicht genehmigt werden, bevor der Löschvorgang versucht wird.

## Progressive Profilerstellung

Wenn die progressive Profilerstellung für ein Formular aktiviert ist, wird ein Feldsatz namens &quot;Profiling&quot;in die Feldliste aufgenommen. Um Felder zur progressiven Profiling-Liste hinzuzufügen oder daraus zu entfernen, müssen Sie den Endpunkt Feldpositionen aktualisieren verwenden. Dieser Endpunkt führt destruktive Aktualisierungen durch, sodass alle Felder im Formular in jeder Anfrage enthalten sein müssen. Im folgenden Beispiel wird das Feld &quot;Telefon&quot;zur progressiven Profiling-Liste hinzugefügt.

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
