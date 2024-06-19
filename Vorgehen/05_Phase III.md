# Phase 3: Implementierung der Basisfunktionen

## Frontend-Implementierung

**Schritt 1: Erstellen der Eingabefelder für Start- und Zielort**
- Erstelle zwei Eingabefelder für Start- und Zielort im Frontend.
- Verwende React für die Implementierung.


```jsx
// src/components/RouteForm.js
import React, { useState } from 'react';

const RouteForm = ({ onCalculate }) => {
    const [start, setStart] = useState('');
    const [end, setEnd] = useState('');

    const handleSubmit = (e) => {
        e.preventDefault();
        onCalculate({ start, end });
    };

    return (
        <form onSubmit={handleSubmit}>
            <div>
                <label>Startort:</label>
                <input type="text" value={start} onChange={(e) => setStart(e.target.value)} />
            </div>
            <div>
                <label>Zielort:</label>
                <input type="text" value={end} onChange={(e) => setEnd(e.target.value)} />
            </div>
            <button type="submit">Route berechnen</button>
        </form>
    );
};

export default RouteForm;
```

**Schritt 2: Implementieren des Buttons zur Routenberechnung**
- Der Button zur Routenberechnung ist im obigen Code integriert. Beim Klicken auf den Button wird die `handleSubmit`-Funktion aufgerufen.

**Schritt 3: Erstellen der UI-Komponenten für die Vorschlagsliste und die Anzeige der Route**
- Füge eine Komponente hinzu, um die berechnete Route anzuzeigen.


```jsx
// src/components/RouteDisplay.js
import React from 'react';

const RouteDisplay = ({ route }) => {
    if (!route) return null;

    return (
        <div>
            <h2>Berechnete Route</h2>
            <pre>{JSON.stringify(route, null, 2)}</pre>
        </div>
    );
};

export default RouteDisplay;
```

**Schritt 4: Hauptkomponente erstellen**
- Erstelle eine Hauptkomponente, die `RouteForm` und `RouteDisplay` integriert.

```jsx
// src/App.js
import React, { useState } from 'react';
import RouteForm from './components/RouteForm';
import RouteDisplay from './components/RouteDisplay';
import axios from 'axios';

const App = () => {
    const [route, setRoute] = useState(null);

    const handleCalculate = async ({ start, end }) => {
        try {
            const response = await axios.post('/api/routes', { start, end });
            setRoute(response.data);
        } catch (error) {
            console.error('Fehler beim Berechnen der Route:', error);
        }
    };

    return (
        <div>
            <h1>Routenberechnung</h1>
            <RouteForm onCalculate={handleCalculate} />
            <RouteDisplay route={route} />
        </div>
    );
};

export default App;
```

## Backend-Implementierung

**Schritt 1: Implementieren des `/routes` Endpoints**
- Erstelle einen Endpoint `/routes`, der die Route zwischen zwei Adressen berechnet.

```javascript
// src/routes/route.js
const express = require('express');
const router = express.Router();
const axios = require('axios');

router.post('/', async (req, res) => {
    const { start, end } = req.body;
    try {
        const response = await axios.post('https://api.openrouteservice.org/v2/directions/driving-car', {
            coordinates: [[start.lng, start.lat], [end.lng, end.lat]]
        }, {
            headers: { 'Authorization': 'your-api-key' }
        });
        res.json(response.data);
    } catch (error) {
        res.status(500).send(error.message);
    }
});

module.exports = router;
```

**Schritt 2: Implementieren des `/addresses` Endpoints**
- Erstelle einen Endpoint `/addresses`, der Adressvorschläge basierend auf einer unvollständigen Adresse zurückgibt.

```javascript
// src/routes/address.js
const express = require('express');
const router = express.Router();
const axios = require('axios');

router.get('/', async (req, res) => {
    const { query } = req.query;
    try {
        const response = await axios.get('https://api.openrouteservice.org/geocode/autocomplete', {
            params: { text: query },
            headers: { 'Authorization': 'your-api-key' }
        });
        res.json(response.data);
    } catch (error) {
        res.status(500).send(error.message);
    }
});

module.exports = router;
```

**Schritt 3: Einrichten der Verbindung zur Datenbank**
- Für diese Phase benötigen wir keine Datenbankanbindung, da die Anforderungen für häufige Suchanfragen entfernt wurden.

**Schritt 4: API-Dokumentation mit Swagger**
- Dokumentiere die Endpunkte mit Swagger.


**Hauptdatei für das Backend:**
```javascript
// src/app.js
const express = require('express');
const app = express();
const routes = require('./routes/route');
const addresses = require('./routes/address');
const swaggerSetup = require('./swagger');

app.use(express.json());

app.use('/api/routes', routes);
app.use('/api/addresses', addresses);

swaggerSetup(app);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server läuft auf Port ${PORT}`);
});
```
