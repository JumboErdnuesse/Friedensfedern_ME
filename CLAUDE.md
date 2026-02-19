# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Projekt

Statische Website des Vereins "Friedensfedern Mettmann e.V.i.G" (Stadttauben-Tierschutz). Gehostet auf GitHub Pages unter `stadttaubenmettmann.github.io/Homepage/`.

## Entwicklung

Kein Build-System, kein Paketmanager, keine Tests. Rein statisches HTML/CSS/JS.

**Lokal testen:** Beliebiger HTTP-Server im Projektroot, z.B.:
```bash
python3 -m http.server 8000
```
Dateien direkt im Browser oeffnen funktioniert eingeschraenkt (FormSubmit, relative Pfade).

**Deployment:** Push auf `main` Branch -> GitHub Pages.

## Architektur

### Shared Components via JS-Injection
Header und Footer sind KEINE HTML-Includes, sondern werden per `DOMContentLoaded`-Event aus JS-Dateien ins DOM geschrieben:
- `header-content.js` - Injiziert Navigation und Logo in `<header>`
- `footer-content.js` - Injiziert Footer-Inhalt in `<footer>`

Jede HTML-Seite laedt diese via `<script src="header-content.js"></script>` innerhalb des jeweiligen Tags.

### Datengetriebene Inhalte
News und Events werden als globale JS-Arrays definiert und von `app.js` dynamisch gerendert:
- `news-data.js` - Array `newsItems[]` mit News-Artikeln
- `events-data.js` - Array `eventItems[]` mit Veranstaltungen

`app.js` erkennt anhand vorhandener DOM-Selektoren (`.news-preview`, `.news-section`, `.events-grid`), welche Inhalte auf der aktuellen Seite gerendert werden sollen.

### Zentrale Logik (app.js)
`app.js` ist die Hauptdatei und steuert:
- Dynamisches Content-Rendering (News, Events)
- Modal-System (Info-Modals + Event-Detail-Modals)
- FAQ-Akkordeon
- Aktive Navigation-Markierung
- Mobiles Hamburger-Menu
- Anker-Navigation mit Smooth-Scroll

### Kontaktseite-spezifische Scripts
Nur auf `kontakt.html` aktiv:
- `contact.js` - URL-Parameter-Auswertung (`?betreff=Mitgliedschaft`) zum Vorausfuellen des Kontaktformulars
- `bank-copy.js` - Clipboard-Kopier-Funktionen fuer Bankdaten
- `form-validation.js` - Telefonnummer-Validierung und Submit-Animation

### Kontaktformular
Nutzt den externen Dienst **FormSubmit** (`formsubmit.co`) - kein eigenes Backend. Weiterleitung nach Absenden auf `danke.html`.

## Seiten

| Datei | Zweck |
|---|---|
| `index.html` | Startseite mit Hero, Highlight-Cards, News-Vorschau, Info-Modals |
| `ueber-uns.html` | Vereinsvorstellung |
| `aktuelles.html` | Vollstaendige News und Events |
| `kontakt.html` | Kontaktformular, Bankdaten, FAQ |
| `impressum.html` | Impressum |
| `datenschutz.html` | Datenschutzerklaerung |
| `danke.html` | Bestaetigungsseite nach Formular-Absenden |

## Konventionen

- Sprache: Deutsch (Inhalte und Code-Kommentare)
- Styling: Alles in einer einzigen `style.css` (kein Preprocessor)
- Fonts: Google Fonts (Montserrat, Open Sans)
- Icons: Font Awesome 6.0
- Modal-Pattern: Overlay mit `modal-show` Klasse, schliesst per Escape/Overlay-Klick/X-Button
- Navigation-IDs: `nav-home`, `nav-about`, `nav-news`, `nav-contact`
