# Aufgabe 2: Erste CI CD Pipeline

**Schritte:**

## Repository erstellen

- Erstellen Sie ein neues öffentliches GitHub-Repository

## Lokale React Anwendung erstellen

- Clone das Repo auf deinen lokalen PC und öffne das Verzeichnis in VS Code
- Erstelle ein neues React Projekt `npx create-react-app .`

## GitHub Actions Workflow erstellen

- In deinem GitHub-Repository erstelle ein Verzeichnis `.github/workflows`.

## YAML-Datei erstellen

- In diesem Verzeichnis erstelle eine neue YAML-Datei, z.B. `ci-cd.yaml`.

## GitHub Actions Workflow konfigurieren

- Füge folgenden Inhalt in die `ci-cd.yaml` Datei ein:

```yaml
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: npm run build --if-present
```

## Let's try it

- Um die Ausführung der Github Action zu testen committe und push deinen Code
```bash
git add .
git commit -m 'init commit'
git push 
```

## Tests

- Ändere den Code so ab, dass nach dem build auch alle unit tests ausgeführt werden
- Teste deine Änderungen

## Recherche

Find durch Online Recherche heraus, was in dem yaml File passiert.

1. Bei welchen Events wird die Action ausgeführt?
    Bei jedem push-Event auf den main-Zweig. Das bedeutet, dass jedes Mal, 
    wenn die Änderungen auf den main-Zweig in den  GitHub-Repository gepusht werden, wird die Action gestartet.
    Bei jedem pull_request-Event auf den main-Zweig. Das bedeutet, dass die Action jedes Mal ausgeführt wird, wenn ein Pull Request erstellt, synchronisiert (wenn neue Commits zu dem Branch hinzugefügt werden, von dem der Pull Request stammt) oder geschlossen wird, der Änderungen auf den main-Zweig vorschlägt

2. Auf welchem System läuft die Action?
    Die GitHub Action, die Sie definiert haben, wird auf einem ubuntu-latest System ausgeführt.

3. Was macht folgende Zeile `uses: actions/checkout@v2`?
    Die actions/checkout Aktion wird verwendet, um das GitHub-Repository in den Arbeitsbereich des Runners zu klonen. Dies ermöglicht es dem Workflow, auf den Quellcode des Repositories zuzugreifen und weitere Schritte auszuführen.
    
    Die @v2 gibt die Version der Aktion an, die verwendet werden soll. In diesem Fall wird die Version 2 der actions/checkout Aktion verwendet. Die Verwendung einer bestimmten Version stellt sicher, dass der Workflow konsistent bleibt, auch wenn neue Versionen der Aktion veröffentlicht werden.
    
4. Was macht folgende Zeile?
```yaml
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
```
    Der Name des Schritts wird auf "Use Node.js" gesetzt, gefolgt von der gewählten Node.js-Version, die aus der Matrix-Konfiguration stammt. Die Matrix-Konfiguration ermöglicht es, den Workflow mit verschiedenen Node.js-Versionen auszuführen.

    Die actions/setup-node Aktion wird verwendet, um die angegebene Node.js-Version auf dem Runner zu konfigurieren. Diese Aktion sucht nach der angegebenen Node.js-Version im Cache der Runner-Tools und fügt die erforderlichen Binärdateien zum PATH hinzu. Dadurch wird sichergestellt, dass die gewünschte Node.js-Version für den Rest des Jobs verwendet wird

    Die node-version Eingabe wird mit dem Wert der Node.js-Version aus der Matrix-Konfiguration gesetzt. Dadurch wird die gewünschte Version an die actions/setup-node Aktion übergeben, um sie zu installieren und zu konfigurieren