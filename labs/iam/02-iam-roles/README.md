# Übungsaufgabe: IAM-Rollen für S3-Zugriff

Diese Übungsaufgabe soll Ihnen dabei helfen, den Umgang mit AWS IAM-Rollen sowohl über die Konsole als auch über das CLI zu vertiefen. Sie werden eine Rolle erstellen, testen und anschließend den Prozess über die Befehlszeile wiederholen.

-----

## Teil 1: Erstellen einer IAM-Rolle über die Konsole

1.  Öffnen Sie die AWS-Management-Konsole und navigieren Sie zu **IAM** wie in der letzten Aufgabe.
2.  Wählen Sie im Navigationsbereich auf der linken Seite **Rollen** und klicken Sie dann auf **Rolle erstellen**.
3.  Wählen Sie als **Vertrauenswürdige Entität** den Typ **AWS-Konto**. Belassen Sie die Einstellung unten auf "Dieses Konto".
4.  Suchen Sie im Bereich **Berechtigungen hinzufügen** nach der Richtlinie **AmazonS3FullAccess** und wählen Sie diese aus. Klicken Sie auf **Weiter**.
5.  Geben Sie der Rolle einen Namen, zum Beispiel `S3FullAccessRoleConsole`. Optional können Sie eine Beschreibung hinzufügen. Klicken Sie auf **Rolle erstellen**.

-----

## Teil 2: Testen der Rolle mittels `AssumeRole` aus der Kommandozeile

Dieser Schritt simuliert die Übernahme der erstellten Rolle. Sie benötigen hierfür einen Benutzer mit den entsprechenden Berechtigungen, um die `AssumeRole`-Aktion durchführen zu können.

1.  Notieren Sie sich den **Rollen-ARN** Ihrer neu erstellten Rolle aus der IAM-Konsole.
2.  Öffnen Sie ein Terminal oder eine Kommandozeile mit konfiguriertem AWS CLI.
3.  Verwenden Sie den Befehl `aws sts assume-role`, um temporäre Anmeldeinformationen zu erhalten:
    ```bash
    aws sts assume-role --role-arn <IHR_ROLLEN_ARN> --role-session-name S3TestSession
    ```
4.  Nach Ausführung des Befehls erhalten Sie temporäre **AccessKeyId**, **SecretAccessKey** und ein **SessionToken**.
5.  Exportieren Sie diese Informationen als Umgebungsvariablen. Dies ermöglicht Ihnen, Aktionen mit den Berechtigungen der übernommenen Rolle auszuführen:
    ```bash
    export AWS_ACCESS_KEY_ID=<IHR_ACCESS_KEY_ID>
    export AWS_SECRET_ACCESS_KEY=<IHR_SECRET_ACCESS_KEY>
    export AWS_SESSION_TOKEN=<IHR_SESSION_TOKEN>
    ```
6.  Testen Sie den S3-Zugriff, indem Sie versuchen, alle Buckets aufzulisten:
    ```bash
    aws s3 ls
    ```
7.  Nach erfolgreichem Test können Sie die Umgebungsvariablen wieder löschen, um zu den ursprünglichen Benutzerberechtigungen zurückzukehren:
    ```bash
    unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN
    ```

-----

## Teil 3: Anlegen einer IAM-Rolle über das CLI

Dieser Teil zeigt, wie Sie den gleichen Vorgang rein über die Kommandozeile automatisieren können.

1.  Erstellen Sie zunächst eine Vertrauensrichtlinie in einer JSON-Datei, zum Beispiel `trust-policy.json`. Diese Datei legt fest, welche Entitäten die Rolle annehmen dürfen. Ersetzen Sie `<ACCOUNT_ID>` durch die Nummer Ihres Accounts.
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "sts:AssumeRole",
                "Principal": {
                  "AWS": "arn:aws:iam::<ACCOUNT_ID>:root"
                }
            }
        ]
    }
    ```
2.  Verwenden Sie den Befehl `aws iam create-role`, um die Rolle zu erstellen. Geben Sie der Rolle einen anderen Namen, zum Beispiel `S3FullAccessRoleCLI`.
    ```bash
    aws iam create-role --role-name S3FullAccessRoleCLI --assume-role-policy-document file://trust-policy.json
    ```
3.  Weisen Sie der Rolle die Berechtigungsrichtlinie für S3-Vollzugriff zu. Die ARN der **AmazonS3FullAccess**-Richtlinie ist eine systemverwaltete Richtlinie.
    ```bash
    aws iam attach-role-policy --role-name S3FullAccessRoleCLI --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
    ```
4.  Sie haben nun eine neue IAM-Rolle mit S3-Vollzugriff über das CLI erstellt. Sie können diese nun wie in Teil 2 testen.