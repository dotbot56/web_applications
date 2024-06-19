### [Web Applications Assignment](https://github.com/fhirter/webengineering/blob/main/Assignments/02_WebApplications.md)


# Gruppenarbeit Web-Engineering "Web-Applikationen"

## Kompetenzen

Die Studierenden

- können eine einfache Single Page Applikation mit einem gängigen Framework erstellen, können die Anwendung mit
  Komponenten strukturieren und Templating anwenden.
- können einfache REST Schnittstellen serverseitig implementieren und clientseitig abfragen.
- können Anwendungen mit Datenbankanbindung realisieren.
- können Anwendungen mit automatisierten Akzeptanztests testen.

## Anforderungen Informatik Applikationsentwicklung und Systemtechnik

Es soll eine Webanwendung entwickelt werden die zwei Orte via Eingabefeldern aufnimmt und die Route zwischen diesen
Orten berechnet und anzeigt.

- Beschreibe die Funktionalität (User Interface) mit automatisierten Akzeptanztests
- Nutze folgende Karten-API Endpoints:
    - [api.openrouteservice.org/geocode/search](https://api.openrouteservice.org/geocode/search)
    - [api.openrouteservice.org/v2/directions/](https://api.openrouteservice.org/v2/directions/)
- Erstelle eine HTML-Seite mit zwei Eingabefeldern für Start- und Zielort sowie einem Button der die Routenberechnung
  startet.
- Erstelle die Anwendung als Single Page Application mit einem gängigen Frontend-Framework.
- Implementiere ein einfaches aber ansprechendes Layout.

## Zusätzlich Informatik Applikationsentwicklung

Die Frontend-Applikation soll nicht direkt mit der Karten-API kommunizieren, sondern die Daten von einer eigenen API
laden.

Die Adressfelder sollen geeignete Vorschläge zur Vervollständigung der Adressen machen.

Die häufigsten 10 Suchanfragen sollen angezeigt werden.

- Nutze zusätzlich folgenden API
  Endpoint: [api.openrouteservice.org/geocode/autocomplete](https://api.openrouteservice.org/geocode/autocomplete)
- Beschreibe die Funktionalität (API) mit automatisierten Akzeptanztests und Teste sämtlichen **eigenen** Code mit
  Unit-Tests.
- Speichere die häufigsten Routen-Suchanfragen lokal im Browser und zeige dem Nutzer die meistgesuchten Routen.
- Entwickle eine REST-API ([Richardson Maturity Level 2](https://en.wikipedia.org/wiki/Richardson_Maturity_Model)) mit zwei Endpoints. Dokumentiere diese mit Swagger:
    - `/routes`: Anhand von zwei Adressen wird eine Route aufgelöst.
    - `/addresses`: Anhand von einer (unvollständigen) Adresse werden Adress-Vorschläge zurückgegeben.
- Nutze für die Frontend-Applikation diese API.
- Diskutiere, wie die implementierte API die REST Prinzipien (Client Server, Stateless, Cache, Uniform Interface,
  Layered System) umsetzt.

## Bewertungsraster

- Frontend Applikation
    - Sinnvolle Strukturierung (Vanilla JS und Framework-Komponenten) (10)
    - Auslesen der Benutzereingaben (10)
    - Laden der Daten von der API (10)
    - Anzeigen der Daten (10)
- HTML und CSS
    - Semantisch korrektes HTML, wenige, sinnvolle Klassen und Selektoren (10)
    - Keine Fehler und Warnungen auf https://validator.w3.org und https://jigsaw.w3.org/css-validator/ (10)
- Akzeptanz- und Unit-Tests
    - Vollständigkeit (10)
    - Verständlichkeit (10)
- REST API
    - Swagger Dokumentation (10)
    - Diskussion REST (10)
    - Korrekte Verwendung der HTTP-Methoden (10)
    - Korrekte HTTP Statuscodes (success & failure) (10)

Total mögliche Punkte Applikationsentwicklung: 120
Total mögliche Punkte Systemtechnik: 80

**Bewertung**: `<erreichte Punkte>/<total mögliche Punkte>*5+1`. Gerundet auf 0.1.

### Punkteskala

Die Vergabe der Punkte geschieht gemäss folgender Skala:

| Punkte | Bezeichnung         |
|--------|---------------------|
| 0      | nicht vorhanden     |
| 1      | kaum vorhanden      |
| 2      | deutlich ungenügend |
| 3      | ungenügend          |
| 4      | knapp genügend      |
| 5      | genügend            |
| 6      | deutliche Mängel    |
| 7      | grössere Mängel     |
| 8      | kleine Mängel       |
| 9      | kleinste Mängel     |
| 10     | tadellos            |