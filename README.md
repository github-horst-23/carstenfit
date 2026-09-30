# CarstenFit

Eine kleine, installierbare Trainings-PWA für Carstens persönlichen Gebrauch. Trainingsplan und Verlauf bleiben im Browser auf dem jeweiligen Gerät.

## Starten

Die Dateien über einen lokalen Webserver oder einen statischen Webhost mit HTTPS bereitstellen. Für Installation und Offline-Nutzung braucht der Browser einen sicheren Kontext (localhost zählt dazu). Danach die Seite im Browser öffnen und „Zum Home-Bildschirm“ bzw. „App installieren“ auswählen.

Es gibt keine Build- oder Paketinstallation. Der Service Worker legt die App-Dateien für die Offline-Nutzung im Cache ab.

## Funktionen

- Wochentrainingsplan mit bearbeitbaren Einheiten und Übungen
- Übungskatalog mit den zuletzt verwendeten Satzwerten
- Protokoll pro Arbeitssatz: Gewicht und Wiederholungen, plus Notizen
- Trainingshistorie mit Einheiten, Arbeitssätzen und Volumen
- Auswertung von Kraftverlauf und Trainingshäufigkeit
- Spotify-Player für einen gespeicherten Playlist-, Album- oder Titel-Link
- JSON-Backup exportieren und wieder importieren

Die Daten werden in `localStorage` gespeichert. Ein Backup auf ein anderes Gerät muss manuell exportiert und importiert werden. Spotify-Inhalte laufen im offiziellen Spotify-Embed-Player; dessen Wiedergabe braucht eine Internetverbindung.

## Google Drive verbinden

CarstenFit speichert `CarstenFit-sync.json` im privaten `appDataFolder` von Google Drive. Dieser Ordner wird von Google für App-Daten verwaltet und ist nicht Teil deiner normalen Drive-Dateiliste. Die App fordert nur den OAuth-Bereich `drive.appdata` an. [Google-Dokumentation zum App-Datenordner](https://developers.google.com/workspace/drive/api/guides/appdata)

### Einmalige Google-Einrichtung

1. Öffne die [Google Cloud Console](https://console.cloud.google.com/), erstelle ein Projekt und aktiviere darin die **Google Drive API**.
2. Richte die OAuth-Zustimmung ein. Für den persönlichen Gebrauch kannst du die App auf **Testing** lassen und dein Google-Konto als Testnutzer hinzufügen.
3. Erstelle unter **APIs & Services → Credentials** eine **OAuth Client ID** vom Typ **Web application**.
4. Füge bei **Authorized JavaScript origins** den Ursprung der App ein. Beispiele: `https://DEIN-NAME.github.io` (ohne Repository-Pfad) oder `http://localhost:8000`. Eine Redirect-URI wird für diesen Browser-Token-Ablauf nicht benötigt.
5. Kopiere die Client-ID, die auf `.apps.googleusercontent.com` endet, und trage sie in CarstenFit oben rechts ein. Wähle **Google Drive verbinden** und erteile die angefragte Berechtigung.

Die OAuth-Client-ID ist öffentlich für Browser-Apps bestimmt; einen Client-Secret braucht CarstenFit nicht. Google gibt ein kurzlebiges Zugriffstoken aus, das nur im Arbeitsspeicher der geöffneten App liegt. Wenn es abläuft oder du die App neu öffnest, starte die Verbindung erneut über einen Knopfdruck. Zum Abgleich auf weiteren Geräten dieselbe Client-ID und dasselbe Google-Konto verwenden. Bei vorhandener Sicherung kannst du Trainingsdaten zusammenführen oder eine Version ersetzen. Zusammenführen bewahrt Einträge; für Löschungen wähle die Version, in der sie enthalten sind.
