---
title: Erweiterter Sicherheitszugriffsmodus
feature: Mobile Marketing
description: Erfahren Sie mehr über den erweiterten Sicherheitszugriffsmodus für Marketo Mobile SDK mit Beispielen für die HMAC-Signaturgenerierung, die Einrichtung von Server-Endpunkten, die Verwendung von Geräte-IDs und iOS und Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
TQID: https://experienceleague.adobe.com/F6lH1aGbCakK-E6IU4wLwYw58BG2-CRE-Ras2bMHeO8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 1%

---

# Erweiterter Sicherheitszugriffsmodus

Der erweiterte Sicherheitszugriffsmodus erfordert, dass Marketo SDK eine Sicherheitssignatur abruft und festlegt. SDK bietet Methoden zum Festlegen und Entfernen der Signatur und eine Dienstprogrammmethode zum Abrufen der Geräte-ID.

Senden Sie bei der Anmeldung die Geräte-ID und die E-Mail-Adresse an den Kundenserver, um die Sicherheitssignatur zu berechnen. Die SDK ruft dann den Kunden-Endpunkt auf, um die Felder abzurufen, die zum Instanziieren des Signaturobjekts erforderlich sind. Wenn der Sicherheitszugriffsmodus in Marketo Mobile Admin aktiviert ist, müssen Sie diese Signatur in der SDK festlegen.

## Einrichtung des abgesicherten Zugriffsmodus

Implementieren Sie diese Einrichtung, bevor Sie den abgesicherten Zugriffsmodus auf der Seite &quot;Marketo Admin“ > „Mobile Apps und Geräte“ aktivieren.

Der Modus für sicheren Zugriff erfordert einen Server-seitigen Signaturalgorithmus und einen Kundenendpunkt. Der Endpunkt gibt die folgenden Werte zurück:

- Zugriffsschlüssel
- Berechnete Signatur
- Zeitstempel der Gültigkeit
- E-Mail-Adresse

Der Algorithmus verwendet den Benutzerzugriffsschlüssel, das Zugriffsgeheimnis, die E-Mail-Adresse, den Zeitstempel und die Geräte-ID. Der Kunde muss den Endpunkt einrichten, die Signaturberechnung implementieren und den Ablaufzeitstempel auf dem neuesten Stand halten.

```python
import argparse
import datetime
import hashlib
import hmac


ACCESS_KEY = 'Your Access Key'
ACCESS_SECRET = 'Your access secret'

# Key should not be unicode
def get_signing_key(timestamp):
    return 'MKTO' + ACCESS_SECRET + str(timestamp)

def get_string_to_sign(email, uuid):
    return email + uuid

def get_hmac(key, string_to_sign):
    return hmac.new(key, string_to_sign.encode('utf-8'), hashlib.sha256).hexdigest()

def get_epoch_plus_day():
    epoch = datetime.datetime.utcfromtimestamp(0)
    valid_until_dt = datetime.datetime.utcnow() + datetime.timedelta(days=1)
    return long((valid_until_dt - epoch).total_seconds())

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--email", required=True, help="email address")
    parser.add_argument("-u", "--uuid", required=True, help="Device install id")
    parser.add_argument("-t", "--timestamp", type=int, help="Valid until timestamp")
    args = parser.parse_args()
    string_to_sign = get_string_to_sign(args.email, args.uuid)
    if not args.timestamp:
        valid_until = get_epoch_plus_day()
    else:
        valid_until = args.timestamp
    signing_key = get_signing_key(valid_until)
    hmac_string = get_hmac(signing_key, string_to_sign)
    print 'HMAC is ', hmac_string
```

Verwenden Sie die plattformspezifischen Methoden, um die Sicherheitssignatur festzulegen oder zu entfernen und die Geräte-ID abzurufen.

### iOS

```objectivec
Marketo * sharedInstance =[Marketo sharedInstance];

// set secure signature
MKTSecuritySignature *signature =
[[MKTSecuritySignature alloc] initWithAccessKey:<ACCESS_KEY> signature:<SIGNATURE_TOKEN> timestamp:<EXPIRY_TIMESTAMP> email:<EMAIL>];
[sharedInstance setSecureSignature:signature];

// remove signature
[sharedInstance removeSecureSignature];

// get device id
[sharedInstance getDeviceId];
```

```swift
let sharedInstance = Marketo.sharedInstance()

 // set secure signature
let signature = MKTSecuritySignature(accessKey: <ACCESS_KEY>, signature: <SIGNATURE_TOKEN> , timestamp: <EXPIRY_TIMESTAMP>, email: <EMAIL>)
sharedInstance.setSecureSignature(signature)

// remove signature
[sharedInstance removeSecureSignature];

// get device id
sharedInstance.getDeviceId()
```

### Android

```java
Marketo sdk = Marketo.getInstance(getApplicationContext());

// set signature
MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
secureMode.setAccessKey(<ACCESS_KEY>);
secureMode.setEmail(<EMAIL_ADDRESS>);
secureMode.setSignature(<SIGNATURE_TOKEN>);
secureMode.setTimestamp(<EXPIRY_DATE>);
if (secureMode.isValid()) {
  sdk.setSecureSignature(secureMode);
}

// remove signature
sdk.removeSecureSignature();

// get device id
sdk.getDeviceId();
```
