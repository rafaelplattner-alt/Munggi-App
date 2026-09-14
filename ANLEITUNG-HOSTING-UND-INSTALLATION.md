# Munggi - App (KOCOA) als installierte App – ohne App Store, ohne Adminrechte

Dieses Paket macht die «Munggi - App» (KOCOA) als **installierbare Web-App (PWA)** verfügbar –
auf **iPad** (über Safari) und auf **Windows/Bundeslaptop** (über Edge). Beides
funktioniert ohne App Store, ohne Entwicklerkonto und ohne Adminrechte.

Die einzige Voraussetzung: Die Dateien müssen **einmal über eine
https-Adresse** erreichbar sein. Danach installiert sich jeder Nutzer die App
selbst, und sie läuft anschliessend auch offline.

---

## Inhalt des Pakets

```
kocoa-web/
├── index.html                 die KOCOA-App
├── manifest.webmanifest       macht sie installierbar (Name, Icon, Vollbild)
├── sw.js                      Service Worker → Offline-Betrieb
├── icon-180.png               Icon für iPad-Home-Bildschirm
├── icon-192.png / icon-512.png  Icons für Manifest / Edge
└── ANLEITUNG-HOSTING-UND-INSTALLATION.md   diese Datei
```

Alle Dateien gehören **in denselben Ordner** auf dem Server (nicht umbenennen,
nicht in Unterordner verteilen – die Namen sind in der App fest referenziert).

---

## Schritt 1: Bereitstellen über https

Es genügt ein Ort, der statische Dateien über **https** ausliefert. Möglich sind
z. B.:

- ein interner Webserver (IIS, Apache, nginx) – Ordnerinhalt hineinlegen, fertig
- eine SharePoint-/Intranet-Bibliothek, die HTML direkt ausliefert
- jeder andere statische https-Webspace, den deine Organisation freigibt

Wichtig ist nur:
- Adresse beginnt mit **https://** (nicht http, nicht file://) – sonst
  registriert sich der Service Worker nicht und die Installation entfällt.
- `sw.js` wird als JavaScript ausgeliefert und `manifest.webmanifest` als
  Textdatei (die meisten Server tun das automatisch; nur bei Fehlern anpassen).

Danach ist die App unter z. B. `https://intranet.example/kocoa/` erreichbar.

---

## Schritt 2a: Installation auf dem iPad (Safari)

1. Adresse in **Safari** öffnen (nicht in einem anderen Browser – nur Safari
   kann auf iOS/iPadOS Web-Apps installieren).
2. Einmal vollständig laden lassen (damit der Offline-Speicher greift).
3. Auf das **Teilen-Symbol** tippen (Quadrat mit Pfeil nach oben).
4. **„Zum Home-Bildschirm"** wählen → Namen bestätigen.
5. Das KOCOA-Icon erscheint auf dem Home-Bildschirm. Ab jetzt startet die App
   im **Vollbild ohne Safari-Leiste** und funktioniert offline.

Offline-Karten werden wie gewohnt **in der App** heruntergeladen (Offline-Paket-
Funktion). Tipp: Projekte zusätzlich über „Projekt speichern" als Datei sichern –
iOS kann bei sehr knappem Speicher Web-App-Daten auslagern.

---

## Schritt 2b: Installation auf dem Bundeslaptop (Edge)

1. Adresse in **Microsoft Edge** öffnen und laden lassen.
2. Menü **„…"** (oben rechts) → **Apps** → **„Diese Website als App
   installieren"**. (Alternativ erscheint rechts in der Adressleiste ein
   kleines Installations-Symbol.)
3. Die App bekommt ein eigenes Fenster und einen Startmenü-Eintrag – **ganz
   ohne Adminrechte**, weil Edge das pro Benutzer einrichtet und keine fremde
   `.exe` ausgeführt wird.

Damit ist die App auf dem gesperrten Gerät als „installierte" Anwendung
nutzbar, obwohl EXE-Dateien dort blockiert sind.

---

## Neue App-Version einspielen

1. Neue `index.html` auf dem Server ersetzen.
2. In **`sw.js`** die Zeile `const CACHE = 'kocoa-v1-...'` hochzählen
   (z. B. `kocoa-v2-...`). Das ist wichtig, damit die installierten Geräte die
   neue Version laden statt der alten aus dem Offline-Speicher.
3. Beim nächsten Start (online) aktualisieren sich die Geräte automatisch.

Der Build-Stempel unten in der Koordinatenleiste zeigt, welche Version läuft.

---

## Häufige Stolpersteine

| Symptom | Ursache / Lösung |
|---|---|
| „Zum Home-Bildschirm" fehlt / kein Vollbild | Nicht in Safari geöffnet, oder Adresse ist nicht https |
| Edge zeigt kein „Als App installieren" | Seite über http statt https geladen, oder `manifest.webmanifest` wird nicht ausgeliefert |
| App startet offline nicht | Beim ersten Mal war das Gerät nicht online → einmal mit Netz öffnen |
| Nach Update alte Version | `CACHE`-Zahl in `sw.js` wurde nicht erhöht |
| Karten bleiben leer | Firewall/Proxy blockiert die Kartenserver (z. B. wmts.geo.admin.ch) – bzw. Offline-Pakete in der App laden |

---

Falls kein interner https-Ort verfügbar ist: melde dich – dann finden wir eine
Alternative (z. B. eine sehr kleine, offiziell freigegebene Bereitstellung oder
den offiziellen Freigabeweg über die IT).

---

## Stand dieses Pakets

Build **T100-2026-08-20** – enthält u.a.: Tab «Führungsunterlagen» (Führungsraster mit PPTX-Export, SpotMap-Geländetaufe inkl. 3D), Rechtsklick-/Langdruck-Werkzeugrad, Fixieren-Funktion, Offline-Paket «Wegnetze & Vektordaten» (Routen-Folgen, Isochronen und Scans offline), Offline-Zoombegrenzung sowie Maskottchen Munggi.
