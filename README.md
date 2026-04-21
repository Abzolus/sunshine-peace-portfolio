# Sunshine Peace — Phase A fertig

Dieser Ordner ist das komplette Repo, das du auf GitHub hochlädst. Netlify zieht es von dort und die Seite ist live.

## Was drin ist

```
sunshine-peace/
├── index.html          Die Website selbst (35 KB, keine eingebetteten Bilder mehr)
├── photos.json         Katalog mit 24 Fotos — das bearbeitet der Fotograf via Admin-Panel
├── netlify.toml        Netlify-Konfig (Cache-Header, /admin-Redirect)
├── admin/
│   ├── index.html      Decap-CMS-Einstiegspunkt
│   └── config.yml      Decap-Konfig (Backend, Feldstruktur)
├── assets/
│   ├── logo.png        Nav-Logo (war base64 in der alten index.html)
│   ├── hero.jpg        Hero-Hintergrundbild
│   └── about.jpg       „Über mich"-Bild
└── photos/
    └── photo-01.jpg … photo-24.jpg    Die 24 Galerie-Fotos
```

Gesamtgröße: ca. 3,8 MB (statt 5,2 MB vorher, weil Base64 ~33 % Overhead hat).

## Bevor du das hochlädst — EINE Stelle anpassen

In `admin/config.yml` steht noch nichts repo-spezifisches (git-gateway braucht das nicht explizit), aber stell sicher, dass der **Branch** stimmt. Wenn dein GitHub-Repo als Default-Branch `main` hat, ist alles gut. Bei `master` in Zeile 9 ändern.

## Nächste Schritte — siehe `anleitung-decap-netlify.md`

Ab **Phase B** (GitHub-Upload) geht's weiter. Kurz zur Erinnerung:

1. Neues GitHub-Repo anlegen (privat oder öffentlich, beides geht).
2. Inhalt dieses Ordners (nicht den Ordner selbst — den **Inhalt**!) ins Repo pushen.
3. In Netlify „New site from Git" → das Repo wählen → Deploy.
4. Site Settings → Identity aktivieren → Git Gateway aktivieren.
5. Fotograf per E-Mail einladen (Identity → Invite User).
6. Er klickt Link, setzt Passwort, landet auf `/admin/`, lädt Fotos hoch. Fertig.

## Test lokal

Wenn du `index.html` einfach per Doppelklick öffnest, funktionieren die Bilder **nicht** — weil `fetch('photos.json')` mit `file://` auf Cross-Origin-Fehler läuft. Teste stattdessen mit einem kleinen lokalen Server:

```bash
cd sunshine-peace
python3 -m http.server 8000
# Dann http://localhost:8000 im Browser
```

Auf Netlify läuft alles sauber, weil's per HTTP ausgeliefert wird.

## Was sich ggü. der alten index.html geändert hat

| Bereich | Vorher | Jetzt |
|---|---|---|
| Dateigröße | 5,2 MB (alles inline) | 35 KB HTML + 3,7 MB Bilder getrennt |
| Fotos ändern | HTML-Datei editieren | `photos.json` via Admin-Panel oder direkt in GitHub |
| Sprache Titel | Hardcoded Mapping in JS | Pro Foto `title_de` + `title_ja` in JSON |
| Browser-Cache | Kein Effekt (alles eine Datei) | Bilder 1 Jahr, HTML immer frisch |

Die komplette Design-Ebene (CSS, Layout, Lightbox, Filter, Mobile-Nav, Sprach-Switcher) ist unverändert.
