# Phase 1: Planung und Anforderungsanalyse

## 1. Anforderungsanalyse
- **Ziele und Anforderungen des Projekts:**
  - Erstellen einer Single Page Application (SPA), die Routen zwischen zwei Orten berechnet und anzeigt.
  - Die Anwendung soll mit einem gängigen Frontend-Framework implementiert werden.
  - Implementierung einer REST-API zur Kommunikation zwischen Frontend und Backend.



- **Funktionale Anforderungen:**
  - Eingabefelder für Start- und Zielort.
  - Button zur Routenberechnung.
  - Anzeige der Route auf der Karte.
  - Adressvorschläge basierend auf Benutzereingaben.



- **Nicht-funktionale Anforderungen:**
  - Responsives und benutzerfreundliches Design.
  - Performante API-Aufrufe.
  - Sicherer Umgang mit API-Schlüsseln.


## 2. Technologie-Stack festlegen
- **Frontend:**
  - Framework: React.js
  - Bibliotheken: Axios (für API-Anfragen), React Router (für Navigation), Bootstrap oder Tailwind CSS (für das Layout)

- **Backend:**
  - Framework: Node.js mit Express
  - Bibliotheken: Axios (für externe API-Anfragen), Swagger (für API-Dokumentation)

- **Datenbank:**
  - MongoDB (für flexible und skalierbare Speicherung) oder PostgreSQL (für relationale Datenbankanforderungen)

- **Testing:**
  - Jest (für Unit-Tests im Backend)
  - React Testing Library (für Unit-Tests im Frontend)
  - Cypress (für Akzeptanztests)

- **API-Dokumentation:**
  - Swagger

## 3. Projektstruktur
- **Frontend:**
  - Verzeichnisstruktur:
    ```
    /src
      /components
      /services
      /pages
      /styles
      /utils
      App.js
      index.js
    ```

- **Backend:**
  - Verzeichnisstruktur:
    ```
    /src
      /controllers
      /models
      /routes
      /services
      app.js
      server.js
    ```

