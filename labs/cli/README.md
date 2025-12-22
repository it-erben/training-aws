# AWS CLI Übung: Ihr erster S3 Bucket

**Ziel:** In dieser Übung lernen Sie, wie Sie die AWS Cloud Shell starten und
mit einfachen AWS CLI-Befehlen einen S3 Bucket erstellen, eine Datei hochladen
und anschließend alles wieder bereinigen.

-----

## Schritt 1: AWS Cloud Shell starten

Zuerst müssen Sie sich in Ihre AWS Management Console einloggen.

1. Nach dem Login finden Sie in der Navigationsleiste oben rechts das Cloud
   Shell-Symbol (ein kleines Terminal-Icon `>_`).
2. Klicken Sie darauf, um die AWS Cloud Shell zu starten. Es kann einen Moment
   dauern, bis die Umgebung vollständig geladen ist.
3. Die Cloud Shell ist eine browserbasierte Shell mit bereits vorinstalliertem
   und konfiguriertem AWS CLI. Sie müssen also keine Anmeldeinformationen
   manuell einrichten.

-----

## Schritt 2: AWS CLI überprüfen

Bevor Sie beginnen, stellen Sie sicher, dass das AWS CLI korrekt funktioniert.
Geben Sie dazu den folgenden Befehl in die Cloud Shell ein und drücken Sie
Enter:

```bash
aws --version
```

Dieser Befehl sollte Ihnen die installierte Version des AWS CLI, von Python und
von Linux anzeigen.

-----

## Schritt 3: Mit Amazon S3 arbeiten

In diesem Teil der Übung interagieren Sie mit Amazon Simple Storage Service
(S3). Sie werden einen "Bucket" erstellen. Ein Bucket ist ein Container für
Objekte (Dateien), die Sie in S3 speichern.

1. **Buckets auflisten:** Sehen Sie sich zuerst an, ob bereits S3 Buckets in
   Ihrem Konto existieren.

    ```bash
    aws s3 ls
    ```

   Wenn Sie noch keine Buckets haben, wird dieser Befehl keine Ausgabe
   erzeugen.

2. **Einen S3 Bucket erstellen:** Jetzt erstellen Sie Ihren eigenen Bucket.
   **Wichtig:** Bucket-Namen müssen global eindeutig sein. Ersetzen Sie
   `ihr-einzigartiger-bucket-name` durch einen von Ihnen gewählten Namen.
   Verwenden Sie nur Kleinbuchstaben, Zahlen und Bindestriche. Ein guter Start
   ist Ihr Name plus das heutige Datum, z.B. `maria-musterfrau-20231026`.

    ```bash
    aws s3 mb s3://ihr-einzigartiger-bucket-name
    ```

   Wenn der Befehl erfolgreich war, erhalten Sie eine Bestätigung wie
   `make_bucket: ihr-einzigartiger-bucket-name`.

   Weitere Informationen zu diesem Befehl finden Sie in der [offiziellen AWS
   CLI-Dokumentation für `aws s3
   mb`](https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html).

3. **Erstellung überprüfen:** Listen Sie erneut alle Ihre Buckets auf, um zu
   sehen, ob Ihr neuer Bucket jetzt angezeigt wird.

    ```bash
    aws s3 ls
    ```

-----

## Schritt 4: Dateien hoch- und herunterladen

Jetzt, wo Sie einen Bucket haben, können Sie Dateien darin speichern.

1. **Eine Testdatei erstellen:** Erzeugen Sie eine kleine Textdatei direkt in
   der Cloud Shell.

    ```bash
    echo "Hallo aus der AWS Cloud Shell" > testdatei.txt
    ```

2. **Datei in den S3 Bucket hochladen:** Kopieren Sie die gerade erstellte Datei
   in Ihren Bucket. Vergessen Sie nicht, `ihr-einzigartiger-bucket-name` wieder
   anzupassen.

    ```bash
    aws s3 cp testdatei.txt s3://ihr-einzigartiger-bucket-name/
    ```

3. **Inhalt des Buckets überprüfen:** Zeigen Sie den Inhalt Ihres Buckets an, um
   zu bestätigen, dass die Datei erfolgreich hochgeladen wurde.

    ```bash
    aws s3 ls s3://ihr-einzigartiger-bucket-name/
    ```

-----

## Schritt 5: Recherche-Aufgabe

Das AWS CLI ist ein mächtiges Werkzeug mit Tausenden von Befehlen und Optionen.
Ein wichtiger Skill ist es, die eingebaute Hilfe und die Online-Dokumentation zu
nutzen.

**Ihre Aufgabe:** Finden Sie selbst heraus, wie Sie:

1. Die Hilfe für den `aws s3`-Befehlssatz anzeigen.
2. Den Befehl finden, um die `testdatei.txt` aus Ihrem S3 Bucket wieder in Ihre
   Cloud Shell-Umgebung herunterzuladen (z.B. unter dem Namen
   `heruntergeladene-datei.txt`).

**Tipp:** Beginnen Sie Ihre Recherche mit `aws s3 help`.

-----

## Schritt 6: Aufräumen

Es ist eine bewährte Praxis in der Cloud, Ressourcen, die Sie nicht mehr
benötigen, zu löschen, um unerwartete Kosten zu vermeiden.

1. **Datei aus dem Bucket löschen:** Zuerst müssen Sie alle Objekte aus einem
   Bucket entfernen, bevor Sie den Bucket selbst löschen können.

    ```bash
    aws s3 rm s3://ihr-einzigartiger-bucket-name/testdatei.txt
    ```

2. **S3 Bucket löschen:** Jetzt können Sie den leeren Bucket entfernen.

    ```bash
    aws s3 rb s3://ihr-einzigartiger-bucket-name
    ```

   Die [Dokumentation für `aws s3
   rb`](https://docs.aws.amazon.com/cli/latest/reference/s3/rb.html) enthält
   weitere Details zu diesem Befehl.

-----

**Glückwunsch\!** Sie haben die grundlegenden Befehle des AWS CLI zur Verwaltung
von S3 Buckets und Objekten direkt in der AWS Cloud Shell erfolgreich verwendet.
