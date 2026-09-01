# Webseite mit automatischem Update

Erstellt von [Eljuanina](https://github.com/Eljuanina)

**Live-Webseite,** [https://eljuanina.github.io/sample-page/](https://eljuanina.github.io/sample-page/)

Diese Vorlage zeigt auf der Startseite an, was in der letzten Commit-Message stand, und testet zusätzlich bei jedem Deployment, ob die Anthropic API erreichbar ist. Bei jedem Push auf `main` wird die Seite automatisch neu publiziert.

## Aufbau

- `public/index.html`, die eigentliche Webseite
- `public/commits.json`, wird bei jedem Lauf automatisch erzeugt, enthält die letzten Commits
- `public/claude-test.json`, wird bei jedem Lauf automatisch erzeugt, enthält die Testantwort von Claude
- `.github/workflows/deploy.yml`, der Workflow, der bei jedem Push die Commit-Info und den Claude-Test erzeugt und die Seite auf GitHub Pages publiziert

## Einrichtung

1. Repository auf GitHub erstellen und diese Dateien hineinpushen.
2. Im Repository unter **Settings, Pages** bei **Source** die Option **GitHub Actions** auswählen.
3. Einen Anthropic API Key in der Anthropic Console erstellen, unter **Not linked (legacy key)**, nicht identity-linked, damit er ohne zusätzliche Workspace-ID funktioniert.
4. Diesen Key im Repository unter **Settings, Secrets and variables, Actions** als neues Secret mit dem Namen `ANTHROPIC_API_KEY` speichern. Der Key steht dann verschlüsselt nur den Workflows dieses Repos zur Verfügung, auch bei einem öffentlichen Repository bleibt er nicht einsehbar.
5. Einen Commit pushen, zum Beispiel auf `main`.
6. Unter dem Tab **Actions** läuft der Workflow **Deploy Webseite**. Nach Abschluss ist die URL unter **Settings, Pages** sichtbar, in diesem Fall [https://eljuanina.github.io/sample-page/](https://eljuanina.github.io/sample-page/).

## Wie es funktioniert

Der Workflow liest bei jedem Push die letzte Commit-Message, den Autor, das Datum und den Kurz-SHA aus und schreibt das in `public/commits.json`. Zusätzlich schickt er eine kurze Anfrage an die Anthropic API mit dem Modell `claude-haiku-4-5-20251001`, dem günstigsten verfügbaren Modell, und schreibt die Antwort in `public/claude-test.json`. Falls das Secret fehlt oder die Anfrage fehlschlägt, wird stattdessen eine Fehlermeldung in diese Datei geschrieben, statt dass der Workflow abbricht.

Die Seite `index.html` lädt beide Dateien per JavaScript und zeigt die Werte an. Weil der Workflow bei jedem Push auf `main` läuft, sind Commit-Liste und Claude-Test immer aktuell.

## Anpassen

- Design und Text lassen sich direkt in `public/index.html` anpassen.
- Wer die Seite auf einem anderen Branch publizieren will, den Branch-Namen in `deploy.yml` unter `on.push.branches` ändern.
- Wer ein anderes Modell testen will, den Wert bei `"model"` im Schritt **Claude-API testen** in `deploy.yml` ändern.

## Ohne neuen Commit erneut deployen

Falls sich nur ein Secret geändert hat, zum Beispiel ein neuer API Key, reicht ein neuer Commit nicht unbedingt. Im Tab **Actions** lässt sich der letzte Lauf von **Deploy Webseite** öffnen und über **Re-run all jobs** erneut ausführen, ganz ohne neuen Push.
