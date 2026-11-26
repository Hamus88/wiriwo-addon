# Wiriwo – Einrichtung auf Home Assistant

Wiriwo läuft komplett auf deinem Home-Assistant-Rechner: App, Datenbank und Fotos.
Von unterwegs erreichst du sie über Tailscale unter einer festen Adresse wie
`https://wiriwo.<dein-netz>.ts.net`.

## 1. Tailscale vorbereiten (einmalig, ca. 5 Minuten)

Am besten am Computer unter <https://login.tailscale.com/admin> anmelden:

1. **DNS** → „MagicDNS“ muss eingeschaltet sein, darunter **HTTPS Certificates** → „Enable HTTPS“.
2. **Access controls** → im Editor diesen Block ergänzen bzw. vorhandenen `nodeAttrs` erweitern:
   ```json
   "nodeAttrs": [
     { "target": ["autogroup:member"], "attr": ["funnel"] }
   ]
   ```
   Speichern. (Nur nötig, wenn die App ohne Tailscale-App erreichbar sein soll.)
3. **Settings → Keys** → „Generate auth key…“ → *Reusable* aus, *Ephemeral* aus → „Generate“.
   Den Schlüssel (`tskey-auth-…`) kopieren. Er wird nur beim ersten Start gebraucht.

## 2. App (Add-on) installieren

1. Home Assistant → **Einstellungen → Apps** → **App installieren** → oben rechts ⋮ → **Repositories**.
2. Diese Adresse eintragen: `https://github.com/Hamus88/wiriwo-addon` → Hinzufügen.
3. „Wiriwo“ erscheint im Store → **Installieren**.
4. Reiter **Konfiguration**: Tailscale-Schlüssel eintragen, Gerätename `wiriwo` lassen → Speichern.
5. Reiter **Info** → **Starten**. Im Reiter **Protokoll** steht nach ca. einer Minute:
   `Wiriwo ist öffentlich erreichbar: https://wiriwo.….ts.net`

Diese Adresse auf den Handys öffnen und über „Teilen → Zum Home-Bildschirm“ als App ablegen.

## 3. E-Mail für „Passwort vergessen“ (optional)

Ohne Mail-Einstellungen steht der Code im Add-on-Protokoll. Mit iCloud:

- SMTP-Server: `smtp.mail.me.com`, Port `587`
- Benutzer: deine iCloud-Adresse
- Passwort: ein **app-spezifisches Passwort** (appleid.apple.com → Anmeldung & Sicherheit)
- Absender: deine iCloud-Adresse

## 4. Familienkalender (optional)

In Wiriwo unter **Mehr → Kalender**. Für iCloud brauchst du ein app-spezifisches Passwort:
account.apple.com → Anmelden und Sicherheit → App-spezifische Passwörter → „+“.
Wiriwo liest den Kalender nur. Steht ein Kindername im Termin („Philipp Fußball“),
erscheint der Termin in der Spalte des Kindes, sonst gilt er für die ganze Familie.
Das Passwort liegt in der Wiriwo-Datenbank und damit auch in den Backups.

## Sicherheit

- Nur die App ist im Internet erreichbar, Home Assistant selbst bleibt privat.
- Neue Konten gibt es nur mit Einladung aus Wiriwo (außer beim allerersten Konto).
- Die Daten liegen im Add-on und sind in den normalen Home-Assistant-Backups enthalten.
  Während des Backups wird Wiriwo kurz angehalten, damit die Datenbank sauber gesichert wird.

## Daten aus Floot übernehmen

Datenbank-Export als `restore.sql` in den Ordner `share/wiriwo/` legen (z. B. per Samba-Add-on)
und Wiriwo neu starten. Die Datei wird einmalig eingespielt.
