# Versionierung und Revert

Ihre Aufgabe ist es, die Versionierung für den Bucket aus Aufgabe 1 zu
aktivieren, die `index.html` zu aktualisieren, und dann zur vorherigen Version
zurückzukehren.

## Versioning aktivieren

Zum Aktivieren der Versionierung mit dem CLI benötigen Sie den Bucket-Namen und
einen Konfigurationsstatus. Erstellen Sie eine JSON-Datei namens
`versioning-config.json` mit folgendem Inhalt:

```json
{
  "Status": "Enabled"
}
```

Der eigentliche Befehl lautet:

```bash
aws s3api put-bucket-versioning \
  --bucket <bucket-name> \
  --versioning-configuration file://versioning-config.json
```

Ersetzen Sie dabei den Bucket-Namen.

## Datei aktualisieren

Bearbeiten Sie Ihre `index.html`-Datei. Ändern Sie den Text, z. B.:

  ```html
  <html>
    <body>
      <h1>Willkommen zur Version 2!</h1>
      <p>Diese Seite wurde aktualisiert.</p>
    </body>
  </html>
  ```

Laden Sie die Datei erneut hoch. Überprüfen Sie die Website-URL – der neue
Inhalt sollte sichtbar sein.

## Versionsliste anzeigen

Verwenden Sie den Befehl

```bash
aws s3api list-object-versions --bucket <bucket-name>
```

Identifizieren Sie die **Version ID** der **ersten Version** Ihrer
`index.html`-Datei.

## Revert zur alten Version

Die einfachste Art, einen Revert (Zurücksetzung) durchzuführen, ist, die alte
Version zu kopieren und sie als neue, aktuelle Version zu speichern. Die alte
Version kann dabei auch die VersionId `null` haben.

```bash
aws s3api copy-object \
  --bucket <bucket-name> \
  --copy-source "<bucket-name>/index.html?versionId=<erste-version-id>" \
  --key index.html
```

Überprüfen Sie die Website-URL. Der ursprüngliche Text sollte wieder sichtbar
sein.
