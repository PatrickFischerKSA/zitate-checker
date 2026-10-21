# Zitate-Checker

Ein eigenständig nutzbares Web-Tool, das Zitierweisen in Word-Dokumenten prüft und vereinheitlicht. Die Anwendung läuft vollständig im Browser und benötigt weder Backend noch API-Schlüssel oder ChatGPT-Anmeldung.

## Funktionen

- APA, MLA, Chicago-Fussnoten, eine explizite Harvard-Variante und IEEE.
- Eigene Vorlagen für Textnachweise, erste und spätere Fussnoten sowie Literatureinträge.
- DOCX auswählen, Quellenbasis erfassen, Änderungen einzeln bestätigen und eine Word-Kopie exportieren.
- Echte Word-Fussnoten, Hinweise für Mac und Windows, einstellbare Prüfempfindlichkeit und Prüfbericht.
- Die Originaldatei bleibt erhalten. Dokumente werden nicht auf einen Server übertragen.

## Lokal starten – Mac, Windows und Linux

Voraussetzung: Node.js 24 oder neuer, mit npm.

```sh
npm ci
npm start
```

Anschliessend im Browser `http://localhost:8080` öffnen. Zum Beenden im Terminal `Ctrl+C` drücken. Der Server lauscht ausschliesslich lokal.

```sh
npm test
```

Die Tests prüfen alle Stilvorlagen, DOCX-Export, echte Fussnoten und deren Rückumwandlung, über mehrere Word-Textabschnitte verteilte Nachweise, Literatureinträge und den Erhalt unbestätigter Änderungen. GitHub Actions führt diese Tests bei Pushes und Pull Requests aus.

## Als GitHub-Repository verwenden

1. Den Inhalt dieses Projektordners bzw. des bereitgestellten ZIP-Archivs in ein neues GitHub-Repository übernehmen. Versteckte Dateien und den Ordner `.github` mit übernehmen.
2. Für die erste Übernahme einen eigenen Git-Checkout verwenden. Der Entwicklungsordner kann bereits mit einem anderen Hosting-Remote verbunden sein; diesen nicht versehentlich überschreiben.
3. Unter **Actions** den Workflow **Tests** kontrollieren.

Das vorbereitete ZIP enthält weder `.git` noch `node_modules` oder die anbieterbezogene `.openai`-Konfiguration. Diese Konfiguration wird für GitHub nicht benötigt. Es sind keine Secrets einzurichten.

## GitHub Pages veröffentlichen

**Achtung:** GitHub Pages kann die Website öffentlich zugänglich machen. Die Anwendung selbst hat dort keinen Anmeldeschutz. Eingelesene Dokumente bleiben dennoch im Browser des jeweiligen Nutzers.

1. Im Repository **Settings → Pages → Build and deployment → Source: GitHub Actions** wählen.
2. Unter **Actions → GitHub Pages → Run workflow** die Veröffentlichung bewusst starten.
3. Die Adresse erscheint im abgeschlossenen Deployment und unter **Settings → Pages**.

Der Pages-Workflow ist absichtlich manuell startbar. Änderungen am Repository veröffentlichen die Website nicht automatisch. Die Verfügbarkeit von Pages hängt vom GitHub-Tarif und den Repository-Einstellungen ab.

Es wird ausschliesslich `dist/` veröffentlicht. Relative Asset-Pfade erlauben auch Projektadressen mit einem Unterpfad. Für anderes statisches Hosting ebenfalls den Inhalt von `dist/` bereitstellen. Ein Build-Schritt ist nicht erforderlich; die benötigte ZIP-Bibliothek ist bereits enthalten.

## Projektstruktur

| Pfad | Zweck |
| --- | --- |
| `dist/index.html` | Oberfläche |
| `dist/style.css` | Gestaltung |
| `dist/app.js` | Erkennung, Vorschläge und DOCX-Verarbeitung |
| `dist/vendor/` | Lokal gebündelte ZIP-Bibliothek und deren Lizenz |
| `test.cjs` | Automatisierte Funktionstests |
| `scripts/serve.cjs` | Plattformunabhängiger lokaler Webserver |
| `.github/workflows/` | Tests und optionales GitHub-Pages-Deployment |

## Datenschutz und Grenzen

Die Verarbeitung von DOCX und Quellenangaben erfolgt lokal. Beim Neuladen gehen diese Daten verloren. Schriftarten werden von Google Fonts geladen; dabei entsteht eine Verbindung zu Google, aber es werden keine Dokumentinhalte übertragen.

Die erste Version unterstützt einfache Buchquellen mit einer Person als Autor:in. Quellenmetadaten müssen bestätigt eingegeben werden. Die Vorlagen orientieren sich an den vorgegebenen Buchbeispielen und sind keine vollständige Implementierung sämtlicher Stilhandbücher. Harvard ist eine Stilfamilie. IEEE-Nummerierung und Länderangabe müssen kontrolliert werden.

Mehrere Autor:innen, Artikel, Internetquellen, Endnoten, komplexe Sammelnachweise, Kopf-/Fusszeilen und dynamische Literaturfelder benötigen manuelle Prüfung. Es gibt keine Garantie vollständiger Erkennung und keine inhaltliche Quellenprüfung. Fehlende Angaben werden nicht erfunden.

Mac und Windows nutzen dieselbe DOCX-Fussnotenstruktur. Die Plattformauswahl gibt Hinweise zur Nachkontrolle. Abweichende Seitenumbrüche und Schriftdarstellungen können nicht verhindert werden. Automatisierte XML-/ZIP-Tests wurden durchgeführt; native Word-Tests auf beiden Betriebssystemen stehen aus.

## Lizenzstatus

Für den eigenen Anwendungscode ist noch keine Open-Source-Lizenz festgelegt (`UNLICENSED`). Das Projekt kann im eigenen Repository verwaltet werden; vor einer Freigabe zur allgemeinen Nachnutzung eine passende Lizenz festlegen. Die enthaltene Drittbibliothek fflate hat eine eigene MIT-Lizenz unter `dist/vendor/fflate-LICENSE.txt`.

GitHub-Pages-Konfiguration: [offizielle GitHub-Anleitung für eigene Workflows](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).

## Schulvorlage aus dem Theorieblatt

Die zusätzliche Schaltfläche **Theorieblatt** setzt den mitgelieferten schulischen Fussnotenstil um. Das Original steht unter `dist/docs/theorieblatt-zitierregeln.pdf`; die Regelhilfe ist direkt bei der Stilwahl aufklappbar.

- Bücher: Nachname, Vorname: Titel. Verlag. Auflage, Ort Jahr. S. …
- Spätere Nennungen: Nachname: bestätigter Kurztitel (sonst voller Titel). S. …
- «Sinngemäss · vgl.» wird pro Änderung gewählt; eine Änderung der Zitatform hebt ihre Bestätigung wieder auf.
- Internetquellen: Quellentyp Internetquelle wählen und vollständige URL sowie echtes Abrufdatum erfassen. Einfache URL-Nachweise werden als echte Fussnoten vorgeschlagen. Das Abrufdatum wird auf Kalendergültigkeit geprüft, aber nicht eigenständig recherchiert.
- Zitatgrenzen, Auslassungen und Fussnotenpositionen müssen kontrolliert werden. Der Checker verschiebt Nachweispositionen nicht anhand einer geratenen Interpretation des Textes.
- Das Theorieblatt definiert kein Literaturverzeichnis. Die dafür angebotene Vorlage ist eine ausdrücklich gekennzeichnete Ableitung. Mehrere Autor:innen, etwa im Frenzel-Beispiel des Blatts, benötigen weiterhin manuelle Prüfung.

Die Einschränkung bezüglich Internetquellen oben gilt für die fünf allgemeinen Stilvorlagen. Die Theorieblatt-Vorlage unterstützt zusätzlich die dort beschriebene einfache URL-Fussnote.
