# Webseite mit automatischem Update

Diese Vorlage zeigt auf der Startseite an, was in der letzten Commit-Message stand. Bei jedem Push auf `main` wird die Seite automatisch neu publiziert.

## Aufbau

- `public/index.html`, die eigentliche Webseite
- `.github/workflows/deploy.yml`, der Workflow, der bei jedem Push die Commit-Info erzeugt und die Seite auf GitHub Pages publiziert

## Einrichtung

1. Repository auf GitHub erstellen und diese Dateien hineinpushen.
2. Im Repository unter **Settings, Pages** bei **Source** die Option **GitHub Actions** auswählen.
3. Einen Commit pushen, zum Beispiel auf `main`.
4. Unter dem Tab **Actions** läuft der Workflow **Deploy Webseite**. Nach Abschluss ist die URL unter **Settings, Pages** sichtbar, meist `https://<benutzername>.github.io/<repository>/`.

## Wie es funktioniert

Der Workflow liest bei jedem Push die letzte Commit-Message, den Autor, das Datum und den Kurz-SHA aus und schreibt das in `public/commit-info.json`. Die Seite `index.html` lädt diese Datei per JavaScript und zeigt die Werte an. Weil der Workflow bei jedem Push auf `main` läuft, ist die Seite immer aktuell.

## Anpassen

- Design und Text lassen sich direkt in `public/index.html` anpassen.
- Wer die Seite auf einem anderen Branch publizieren will, den Branch-Namen in `deploy.yml` unter `on.push.branches` ändern.
