---
title: Erweiterter Sicherheitszugriffsmodus
feature: Mobile Marketing
description: Details zum erweiterten Sicherheitszugriffsmodus
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Erweiterter Sicherheitszugriffsmodus

Das Marketo SDK stellt Methoden zum Festlegen und Entfernen der Sicherheitssignatur bereit. Es gibt auch eine Dienstprogrammmethode zum Abrufen der Geräte-ID. Die Geräte-ID sollte bei der Anmeldung zusammen mit der E-Mail an den Kundenserver zur Berechnung der Sicherheitssignatur übergeben werden. Das SDK sollte den neuen Treffer-Endpunkt, der auf den oben aufgeführten Algorithmus verweist, aufrufen, um die erforderlichen Felder zum Instanziieren des Signaturobjekts abzurufen. Das Festlegen dieser Signatur im SDK ist ein notwendiger Schritt, wenn der Sicherheitszugriffsmodus in Marketo Mobile Admin aktiviert wurde.

## Einrichten des sicheren Zugriffsmodus

Diese Einrichtung muss implementiert werden, bevor der sichere Zugriffsmodus über die Seite Marketo Admin > Mobile Apps und Geräte aktiviert wurde. Die folgenden weiteren Schritte beschreiben den Prozess, der zum Abschließen der Sicherheitsvalidierung erforderlich ist:

Der sichere Zugriffsmodus erfordert die Implementierung des Signaturalgorithmus auf der Kundenseite, der einen Endpunkt zum Abrufen des Zugriffsschlüssels, der berechneten Signatur, des Ablaufzeitstempels und der E-Mail-Adresse bereitstellt. Dieser Algorithmus erfordert den Zugriffsschlüssel des Benutzers, das Zugriffsgeheimnis, die E-Mail-Adresse, den Zeitstempel und die Geräte-ID, um die Berechnung durchzuführen. Der Kunde ist für die Einrichtung des Endpunkts, die Implementierung des Algorithmus zur Durchführung von Signaturberechnungen und die Beibehaltung des Ablaufzeitstempels zuständig.

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

Das Marketo SDK stellt neue Methoden zum Festlegen und Entfernen der Sicherheitssignatur bereit. Es gibt auch eine Dienstprogrammmethode zum Abrufen der Geräte-ID. Die Geräte-ID sollte bei der Anmeldung zusammen mit der E-Mail an den Kundenserver zur Berechnung der Sicherheitssignatur übergeben werden. Das SDK sollte den neuen Treffer-Endpunkt, der auf den oben aufgeführten Algorithmus verweist, aufrufen, um die erforderlichen Felder zum Instanziieren des Signaturobjekts abzurufen. Das Festlegen dieser Signatur im SDK ist ein notwendiger Schritt, wenn der Sicherheitszugriffsmodus in Marketo Mobile Admin aktiviert wurde.

### iOS

```
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

```
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

```
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
