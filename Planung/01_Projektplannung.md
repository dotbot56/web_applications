# Projektplanung

## Phase 1: Planung und Anforderungsanalyse
1. **Anforderungsanalyse:**
    - Sammeln und Dokumentieren aller Anforderungen.
    - Erstellen eines Anforderungsdokuments.

2. **Technologie-Stack festlegen:**
    - Frontend: React.js oder Vue.js
    - Backend: Node.js mit Express
    - Datenbank: MongoDB oder PostgreSQL
    - Testing: Jest, React Testing Library, Cypress
    - API-Dokumentation: Swagger

3. **Projektstruktur:**
    - Definieren der Projektstruktur und der Verzeichnisstruktur für Frontend und Backend.

## Phase 2: Setup und Initialisierung
1. **Initialisierung des Frontend-Projekts:**
    - Erstellen eines neuen Projekts (z.B. mit Create React App).
    - Einrichten der Grundstruktur.
    - Installieren der benötigten Bibliotheken (Axios, React Router etc.).

2. **Initialisierung des Backend-Projekts:**
    - Erstellen eines neuen Projekts mit Express.
    - Einrichten der Grundstruktur.
    - Installieren der benötigten Bibliotheken (Axios, Swagger etc.).

3. **Datenbank-Setup:**
    - Einrichten der Datenbank (z.B. MongoDB Atlas, PostgreSQL).
    - Erstellen der notwendigen Datenbank-Schemas und Tabellen.

## Phase 3: Implementierung der Basisfunktionen
1. **Frontend-Implementierung:**
    - Erstellen der Eingabefelder für Start- und Zielort.
    - Implementieren des Buttons zur Routenberechnung.
    - Erstellen der UI-Komponenten für die Vorschlagsliste und die Anzeige der häufigsten Suchanfragen.

2. **Backend-Implementierung:**
    - Implementieren des `/routes` Endpoints.
    - Implementieren des `/addresses` Endpoints.
    - Einrichten der Verbindung zur Datenbank.

3. **API-Dokumentation:**
    - Dokumentieren der API-Endpunkte mit Swagger.

## Phase 4: Erweiterung und Integration
1. **Adressvorschläge:**
    - Implementieren der Adressvervollständigung im Frontend.
    - Integrieren des `/addresses` Endpoints zur Abfrage von Adressvorschlägen.

2. **Routenberechnung:**
    - Implementieren der Routenberechnung im Frontend.
    - Integrieren des `/routes` Endpoints zur Berechnung und Anzeige der Route.

3. **Suchanfragen speichern und anzeigen:**
    - Implementieren der Speicherung der häufigsten Suchanfragen in der Datenbank.
    - Anzeigen der meistgesuchten Routen im Frontend.

## Phase 5: Tests und Validierung
1. **Unit-Tests:**
    - Schreiben von Unit-Tests für die Frontend-Komponenten.
    - Schreiben von Unit-Tests für die Backend-Funktionen.

2. **Akzeptanztests:**
    - Schreiben und Ausführen von Akzeptanztests zur Validierung der UI-Funktionalitäten.

3. **Integrationstests:**
    - Testen der Integration zwischen Frontend und Backend.
    - Sicherstellen, dass alle API-Endpunkte korrekt funktionieren.

## Phase 6: Deployment
1. **Deployment:**
    - Deployment des Backends auf einem Server (Proxmox).
    - Deployment des Frontends (z.B. Netlify, Vercel).

2. **Dokumentation:**
    - Erstellung einer Benutzerdokumentation.
    - Pflege der API-Dokumentation mit Swagger.



