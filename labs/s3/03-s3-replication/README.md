# Übung: S3 Bucket Replication einrichten und testen

In dieser Übung lernen Sie, wie man eine S3 Cross-Region Replication (CRR)
zwischen zwei Buckets einrichtet, wie man sie testet und wie man wichtige
Schritte mit der AWS Management Console und dem AWS CLI (`aws s3`) durchführt.

---

## Szenario

Ihre Firma speichert wichtige Daten in einem S3 Bucket in eu-central-1
(Frankfurt). Zur Erhöhung der Ausfallsicherheit sollen alle neuen Objekte
automatisch in einen zweiten Bucket in eu-west-1 (Irland) repliziert werden.

---

## Quell- und Ziel-Bucket anlegen (CLI)

Legen Sie zwei Buckets an:

* Quellbucket: my-replication-source-`deinname` in **eu-central-1**
* Zielbucket: my-replication-dest-`deinname` in **eu-west-1**

Sie können dafür Konsole oder CLI verwenden.

> Tipp: Bucket-Namen müssen global eindeutig sein – wählen Sie also einen
> einzigartigen Namen.

---

## Versionierung aktivieren (Konsole empfohlen)

Die Replikation funktioniert nur mit aktivierter Versionierung. Die Aktivierung
geht am einfachsten über die S3-Konsole:

Öffnen Sie für jeden Bucket: → Eigenschaften → Bucket Versioning → Bearbeiten In
diesem Dialog können Sie die Versionierung aktivieren.

> Es gibt auch eine Möglichkeit, dies mit einem CLI-Befehl durchzuführen. Mehr
> Informationen findest du hier:
>
> <https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html>

---

## Replikationsrolle anlegen (Konsole oder CLI)

Erstellen Sie eine IAM-Rolle für S3, die Replikation durchführen darf. Mit der
Konsole geht dies so:

1. Öffne die IAM-Konsole → Rollen → Rolle erstellen
2. Typ: S3
3. Berechtigungen: `AmazonS3FullAccess`
4. Name: `S3ReplicationRole`
Sie können dafür auch das CLI verwenden.

Merken Sie sich die ARN der Rolle – sie wird im nächsten Schritt benötigt.

---

## Replikationsregel einrichten (Konsole empfohlen)

Wechslen Sie in die S3-Konsole:

1. Öffnen Sie den Quellbucket
2. Gehen Sie zu Verwaltung → Replikationsregeln → Replikationsregel erstellen
3. Regelname: `replicate-to-ireland`
4. Quelle: Gesamter Bucket
5. Keinen Filter anwenden (Regelumfang auswählen → Auf alle Objekte anwenden)
6. Ziel: Ihr Zielbucket in Irland
7. IAM-Rolle: Die eben erstellte S3ReplicationRole
8. Aktivieren Sie die Regel

> Optional: Aktivieren Sie Replication Time Control (RTC) für eine garantierte
> Replikationszeit < 15 Minuten (kostenpflichtig).

---

## Replikation testen (CLI)

Erstellen Sie nun eine Testdatei und laden Sie sie in den Quellbucket hoch. Nach
einigen Minuten sollte die Datei automatisch im Zielbucket erscheinen.

---

## Bonusaufgabe: Filter hinzufügen

Löschen Sie die eben erstellte Replikationsregel. Fügen Sie eine Regel hinzu,
die nur Objekte mit dem Prefix `logs/` repliziert. Erstellen Sie anschließend
eine Datei unter diesem Pfad und eine Datei, die nicht in diesem Pfad liegt.
Überprüfen Sie nach einigen Minuten, welche Dateien angekommen sind im
Zielbucket.

---

## Aufräumen

Zum Schluss löschen Sie beide Buckets bitte wieder.
