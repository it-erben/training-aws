# Aufgabe: Static Website Hosting mit der CLI

Ihre Aufgabe ist es, einen S3-Bucket zu erstellen, Static Website Hosting zu aktivieren und eine einfache `index.html` hochzuladen, die dann über eine öffentliche URL erreichbar ist.

---

## Bucket erstellen

Erstellen Sie mit der Konsole oder mit dem CLI (`aws s3 mb`) einen neuen S3-Bucket.

## Static Website Hosting aktivieren

Wir aktivieren in dieser Übung das Static Website Hosting für den Bucket. Damit lassen sich Webseiten hosten.
Dies geht über die Konsole oder über das CLI. Hier machen wir dies einmal mit dem CLI.

Der Befehl benötigt den Bucket-Namen und eine Konfigurationsdatei im JSON-Format. Erstellen Sie eine Datei namens `website-config.json` mit folgendem Inhalt:
  ```json
  {
    "IndexDocument": {
      "Suffix": "index.html"
    }
  }
  ```

Der eigentliche Befehl lautet `aws s3api put-bucket-website --bucket <bucket-name> --website-configuration file://website-config.json`. Ersetzen Sie den Bucket-Namen in dem Befehl.

## Öffentlichen Zugriff erlauben

Standardmäßig ist der öffentliche Zugriff blockiert. Um die Website sichtbar zu machen, müssen Sie eine Bucket Policy erstellen und den öffentlichen Zugriff für den Bucket erlauben. Ersetzen Sie den Bucket-Namen in diesem Befehl.

```bash 
aws s3api delete-public-access-block --bucket <bucket-name>
```

Erstellen Sie eine JSON-Datei namens `public-policy.json` mit folgendem Inhalt:
```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::<bucket-name>/*"
      }
  ]
}
```
**Wichtig:** Ersetzen Sie `<bucket-name>` durch Ihren Bucket-Namen und **deaktivieren Sie in der Konsole** die Blockierung des öffentlichen Zugriffs, bevor Sie die Policy anwenden.

Der eigentliche Befehl lautet dann `aws s3api put-bucket-policy --bucket <bucket-name> --policy file://public-policy.json`
Ersetzen Sie auch hier den Bucket-Namen.

## Datei hochladen und testen

Erstellen Sie eine einfache Textdatei namens `index.html` mit folgendem Inhalt:

```html
<html>
<body>
  <h1>Willkommen zur Static Website!</h1>
  <p>Diese Seite wurde mit der AWS CLI hochgeladen.</p>
</body>
</html>
```


Verwenden Sie das CLI oder die Konsole, um die eben erstellte `index.html` in den Bucket hochzuladen.
Die Website-URL lautet `http://<bucket-name>.s3-website.<region>.amazonaws.com` mit Ihrem Bucket-Namen und Ihrer Region. Ihr `index.html`-Inhalt sollte sichtbar sein.
