# Arbeitsregeln

## Ton

- Knapp. Sag, was zu sagen ist, dann Schluss. Kein Vorgeplänkel, keine
  Zusammenfassung des gerade Getanen, kein „gute Frage“, kein Wiederholen der
  Aufgabe.
- Keine Füll-Adjektive (robust, nahtlos, mächtig, umfassend, produktionsreif).
  Knapp sagen, was der Code tut, nicht wie gut er ist. Nicht paraphrasieren, was
  die nächsten Zeilen tun. Stattdessen das WARUM und WIE erklären, wenn das dem
  Verständnis wirklich hilft.
- Docs und READMEs: was es ist, wie man es nutzt, was es bereitstellt. Sonst
  nichts.
- Commit-Nachrichten: conventional-commit, Imperativ, möglichst einzeilig. Den
  Scope richtig wählen — Release-Tooling routet unter Umständen darüber. Breaking
  Changes bekommen ein `!` (`feat(api)!: …`) oder einen `BREAKING CHANGE:`-Footer.
  Betreffzeile ≤ 72 Zeichen, Imperativ („add“, „fix“, nicht „added“, „fixes“).
  Body auf 72 Zeichen umbrechen.
- Kleine, fokussierte Commits bevorzugen. Release-Tooling leitet Versionssprünge
  und Changelog oft aus den Commit-Betreffzeilen ab.
- Keine Ticket-Nummern in Code, Commits oder Docs.
- Kommentare erklären das *Warum*, nicht das *Was*. Code-Kommentare benennen die
  Absicht oder eine Einschränkung, die der Code nicht zeigen kann. Kommentare
  löschen, die den Code nur wiederholen.
- Kommentare und Docs immer als Ganzes betrachten. Nie nur anhängen. Im Kontext
  prüfen und auf den faktischen Stand bringen. Im Zweifel im Code recherchieren.
  Veraltete und aus dem Kontext gefallene Verweise entfernen, ebenso frühere
  Beobachtungen, Schilderungen von Situationen, die zu einer früheren Änderung
  führten, Maschinennamen oder -adressen sowie jede Vermutung über die
  nachgelagerte Nutzung dieses Repos und seiner Artefakte — abgesehen von
  gültigen, aktuellen Beispielen.
- Auf ein anderes Repository oder Projekt nur verweisen, wenn dessen Zustand der
  unmittelbare Grund für die Änderung ist (ein Dependency-Bump, ein eingespielter
  Fix, ein an eine veröffentlichte Version gebundener API-Vertrag). Kontext für
  Reviewer, Dank oder Querverweise gehören in den PR-Thread oder ein Issue, nicht
  in den Commit.
- Deklarative Fakten schreiben. Keine Personalpronomen („ich“, „wir“, „du“).
  Keine Leseransprache: kein „beachte, dass…“, „wie man sieht…“, „wir haben uns
  entschieden…“, „das sollte helfen…“. Die Regel gilt für Dokumentation, die
  ein Artefakt beschreibt. Ausgenommen sind die Lab-Texte unter `labs/`, siehe
  unten.
- Nicht erzählen. Keine Historie, was zuerst versucht wurde, was scheiterte oder
  welche Alternativen erwogen wurden.
- Keine Füll-Verben ohne Konkretes. „Aufräumen“, „verbessern“, „refactoren“
  allein sagen nichts; entweder die tatsächliche Änderung benennen oder die Zeile
  weglassen.
- Keine Checklisten, keine „Summary“-/„Test plan“-Abschnitte, keine
  Marketing-Sprache, keine Emojis.

## Lab-Anleitungen

Alles unter `labs/` ist Lehrmaterial und hebt die Pronomen- und
Leseransprache-Regel auf.

- **Gesiezt.** Durchgängig „Sie“ und „Ihr“, nie „du“ oder „ihr“. Die Anrede
  liegt bei 569 zu 1 gegen Duzen; eine geduzte Zeile fällt sofort auf.
- Imperativische Anweisung im Sie-Stil: „Melden Sie sich an“, „Wählen Sie unter
  Laufzeit den Wert Python 3.13“. Kein „man kann“, kein Passiv.
- Ein Lab beginnt mit ein bis zwei Sätzen, die sagen, was darin entsteht und
  welcher Dienst geübt wird.
- Der Weg durch die AWS Console wird als Klickpfad beschrieben, Schritt für
  Schritt, mit dem Beschriftungstext der Console in Anführungszeichen
  („Funktion erstellen“, „Direktes Anfügen von Richtlinien“). Die Console ist
  auf Deutsch; deutsche Beschriftungen verwenden.
- Nach jedem Schritt, der etwas sichtbar verändert, ein Screenshot. Dateinamen
  sind dreistellig nummeriert und beschreiben den Inhalt
  (`004-iam-policy.png`).
- Dienstnamen, CLI-Kommandos, Policies und Ressourcennamen in Backticks:
  `aws iam attach-user-policy`, `AmazonS3FullAccess`, `s3-full-access-user`.
- CLI-Aufgaben nennen den Befehl und die entscheidenden Parameter, aber nicht
  die fertige Zeile. Der Aufruf soll selbst zusammengesetzt werden.
- Jedes Lab endet mit einem Test, der beweist, dass es funktioniert hat, und
  optional mit einer `## Zusatzaufgabe` für Schnelle.
- Praxisabweichungen als Blockzitat direkt unter der Einleitung kennzeichnen,
  wenn das Lab bewusst einen veralteten Weg geht (IAM-User statt Identity
  Center).
- `> [!tip]`-Callouts für Nebenwege, die nicht zum Lösungsweg gehören.
- Knapp auf Satzebene gilt weiterhin: keine Füll-Adjektive, kein Marketing,
  keine Zusammenfassung des Abschnitts darüber.

## Vor dem Abschluss

- Lint, Tests und Build des Projekts für alles Berührte ausführen.
- `pre-commit run --all-files` laufen lassen und alle Befunde beheben.
- Nicht „fertig“ behaupten, ohne die Prüfung ausgeführt zu haben. Belege vor
  Behauptungen.
- Alle TODO-Marker entfernen, die du in deiner Sitzung hinzugefügt hast, und
  nacharbeiten — oder dem Nutzer sagen, dass ein Follow-up nötig ist. Alle Marker
  und Verweise auf deine eigene Aufgabenliste oder historische Arbeitsschritte
  (P2, P3a, Item 1, Task A usw.) samt ihrer Erzählung entfernen. Wenn wirklich
  etwas offen bleibt, dem Nutzer außerhalb von Code, Docs, Markdown, Kommentaren,
  PR-Beschreibungen, Commit-Nachrichten oder allem anderen in diesem Repo und
  seiner angeschlossenen Pipeline Bescheid geben.

## Aufbau dieses Repos

Reine Übungssammlung für die AWS-Schulungen der GFU Cyrus. Keine Folien, kein
Anwendungscode, kein Build. Alles liegt unter `labs/`, ein Verzeichnis je
Dienst: `beanstalk`, `cli`, `cloudformation`, `cloudwatch`, `ec2`, `ecs`,
`iam`, `lambda`, `s3`.

Zwei Layouts nebeneinander:

- **Ein Lab je Dienst.** `labs/<dienst>/README.md` enthält die vollständige
  Anleitung; bei `ec2`, `ecs`, `iam` und `s3` liegt sie in nummerierten
  Unterordnern (`labs/iam/01-iam-users/README.md`).
- **Aufgeteilt.** `beanstalk`, `cloudformation`, `cloudwatch` und `lambda`
  trennen in `README.md` (ein Absatz zum Dienst plus Links), `aufgabe.md`
  (Teilnehmeraufgabe) und `demo.md` (Ablauf der Live-Demo des Trainers).

Ein neues Lab folgt dem Layout des Dienstes, unter dem es liegt.

## Fallstricke dieses Repos

- **Der Bilderordner heißt nicht überall gleich.** `ec2`, `ecs`, `iam` und `s3`
  legen Screenshots in `images/`, `beanstalk`, `cloudwatch` und `lambda` in
  `pictures/`. Beim Anlegen den Nachbarn im selben Dienst prüfen, nicht raten.
- **Bildpfade laufen leicht auseinander.** In `labs/lambda/aufgabe.md` zeigt
  der Alt-Text `001-basic-settings.png` auf die Datei `010-basic-settings.png`.
  Nach jeder Änderung an Screenshots prüfen, dass Alt-Text und Pfad dieselbe
  Datei meinen.
- **`labs/cloudformation/lösungen` trägt einen Umlaut im Pfad.** Nicht
  umbenennen, ohne alle Verweise mitzuziehen.
- **Die CI kann nur linten.** `.gitlab-ci.yml` bindet ausschließlich die drei
  Linter-Komponenten ein (`lychee-lint`, `yaml-lint`, `markdown-lint`). Kein
  PDF-Build, kein Release, kein Deploy. Ein Commit hier erzeugt keine Version
  und kein Artefakt — der Scope der Commit-Nachricht routet nichts.
- **Die Linter-Einstellungen stehen doppelt**, in `.gitlab-ci.yml` über die
  Komponenten und lokal in `.pre-commit-config.yaml`. Beide synchron halten,
  wenn sich Regeln ändern.
- **Es gibt keine `.markdownlint.json`.** Der Linter läuft auf Defaults. Neue
  Prosa an der Zeilenbreite der bestehenden Labs ausrichten (rund 80 Zeichen).
- Die Labs sprechen die deutsche AWS Console an. Ändert AWS eine Beschriftung,
  stimmen Klickpfad und Screenshot nicht mehr überein; beides zusammen
  aktualisieren.
- **Die CI läuft auf zwei Plattformen.** `.gitlab-ci.yml` bindet die
  GitLab-Komponenten ein, `.github/workflows/ci.yml` ruft `lint.yml` aus
  `it-erben/ci`. Auf GitHub prüft die Pipeline bei Pull Requests und
  bei Pushes auf `main`.
