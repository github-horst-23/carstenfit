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

## OneDrive verbinden

Die Synchronisierung legt `CarstenFit-sync.json` im App-Ordner deines OneDrive ab. Sie verwendet [Microsoft Graph](https://learn.microsoft.com/en-us/graph/onedrive-sharepoint-appfolder) und fordert ausschließlich die delegierte Berechtigung `Files.ReadWrite.AppFolder` an. Microsofts [MSAL-Bibliothek](https://learn.microsoft.com/en-us/entra/msal/javascript/browser/about-msal-browser) führt die Anmeldung per Authorization Code Flow mit PKCE aus und verwaltet die Tokens im Browser.

### Einmalige Microsoft-Einrichtung

1. Im [Microsoft Entra Admin Center](https://entra.microsoft.com/) eine App-Registrierung namens **CarstenFit** erstellen. Als unterstützte Kontotypen **Konten in einem beliebigen Organisationsverzeichnis und persönliche Microsoft-Konten** wählen.
2. Unter **Authentifizierung** eine Plattform **Single-page application (SPA)** hinzufügen. Als Redirect-URI die URL dieser App eintragen, zum Beispiel `http://localhost:8000/index.html` oder die HTTPS-Adresse, unter der du sie hostest.
3. Unter **API-Berechtigungen** für Microsoft Graph die delegierte Berechtigung `Files.ReadWrite.AppFolder` hinzufügen.
4. Die **Anwendungs-ID (Client)** der App-Registrierung kopieren.
5. CarstenFit über `localhost` oder HTTPS öffnen, oben rechts die OneDrive-Verbindung öffnen, die Anwendungs-ID eintragen und **Mit Microsoft anmelden** wählen.

Danach kannst du mit **Jetzt synchronisieren** die Daten auf OneDrive sichern. Auf einem zweiten Gerät dieselbe Anwendungs-ID eintragen, dich mit demselben Microsoft-Konto anmelden und synchronisieren. Bei vorhandener Sicherung kannst du die Verläufe zusammenführen oder eine Seite ersetzen. Zusammenführen bewahrt neue Trainingseinheiten und Übungen; um Löschungen zu übertragen, wähle die Seite, auf der die Löschung bereits durchgeführt wurde.

Die App erhält keinen Client-Secret und speichert keine Microsoft-Passwörter. MSAL verwaltet die Anmeldungstokens lokal im Browser. Die OneDrive-Verbindung braucht Internetzugang; die restliche App bleibt offline nutzbar.
