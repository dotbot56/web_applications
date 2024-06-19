# Phase 4: Erweiterung und Integration

## 1. Adressvorschläge

**Schritt 1: Implementieren der Adressvervollständigung im Frontend**
- Fügen Sie eine Funktion hinzu, die während der Benutzereingabe Vorschläge aus der API abruft.

**Code:**
```jsx
// src/services/addressService.js
import axios from 'axios';

export const getAddressSuggestions = async (query) => {
    try {
        const response = await axios.get(`/api/addresses`, { params: { query } });
        return response.data;
    } catch (error) {
        console.error('Error fetching address suggestions:', error);
        return [];
    }
};
```

**Komponente:**
```jsx
// src/components/AddressInput.js
import React, { useState } from 'react';
import { getAddressSuggestions } from '../services/addressService';

const AddressInput = ({ label, value, onChange }) => {
    const [suggestions, setSuggestions] = useState([]);

    const handleInputChange = async (e) => {
        const query = e.target.value;
        onChange(query);
        if (query.length > 2) {
            const results = await getAddressSuggestions(query);
            setSuggestions(results.features);
        } else {
            setSuggestions([]);
        }
    };

    return (
        <div>
            <label>{label}</label>
            <input type="text" value={value} onChange={handleInputChange} />
            <ul>
                {suggestions.map((suggestion, index) => (
                    <li key={index} onClick={() => onChange(suggestion.properties.label)}>
                        {suggestion.properties.label}
                    </li>
                ))}
            </ul>
        </div>
    );
};

export default AddressInput;
```

**Integration in die Hauptkomponente:**
```jsx
// src/App.js
import React, { useState } from 'react';
import AddressInput from './components/AddressInput';
import RouteDisplay from './components/RouteDisplay';
import axios from 'axios';

const App = () => {
    const [start, setStart] = useState('');
    const [end, setEnd] = useState('');
    const [route, setRoute] = useState(null);

    const handleCalculate = async () => {
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
            <AddressInput label="Startort:" value={start} onChange={setStart} />
            <AddressInput label="Zielort:" value={end} onChange={setEnd} />
            <button onClick={handleCalculate}>Route berechnen</button>
            <RouteDisplay route={route} />
        </div>
    );
};

export default App;
```

## 2. Routenberechnung

**Schritt 1: Implementieren der Routenberechnung im Frontend**
- Verwenden Sie die oben genannte `handleCalculate`-Funktion in der `App.js`-Komponente.

**Schritt 2: Integrieren des `/routes` Endpoints**
- Die Integration ist bereits im `handleCalculate`-Beispielcode enthalten.

#### 3. Suchanfragen speichern und anzeigen

**Backend: Speichern der häufigsten Suchanfragen**

**Code:**
```javascript
// src/models/SearchQuery.js
const mongoose = require('mongoose');

const searchQuerySchema = new mongoose.Schema({
    start: String,
    end: String,
    count: { type: Number, default: 1 }
});

searchQuerySchema.index({ start: 1, end: 1 }, { unique: true });

const SearchQuery = mongoose.model('SearchQuery', searchQuerySchema);

module.exports = SearchQuery;
```

**Erweitern des `/routes` Endpoints:**
```javascript
// src/routes/route.js
const express = require('express');
const router = express.Router();
const axios = require('axios');
const SearchQuery = require('../models/SearchQuery');

router.post('/', async (req, res) => {
    const { start, end } = req.body;
    try {
        const response = await axios.post('https://api.openrouteservice.org/v2/directions/driving-car', {
            coordinates: [[start.lng, start.lat], [end.lng, end.lat]]
        }, {
            headers: { 'Authorization': 'your-api-key' }
        });

        // Speichern der Suchanfrage
        const query = await SearchQuery.findOneAndUpdate(
            { start, end },
            { $inc: { count: 1 } },
            { upsert: true, new: true }
        );

        res.json(response.data);
    } catch (error) {
        res.status(500).send(error.message);
    }
});

module.exports = router;
```

**Häufigste Suchanfragen Endpoint:**
```javascript
// src/routes/searchQueries.js
const express = require('express');
const router = express.Router();
const SearchQuery = require('../models/SearchQuery');

router.get('/', async (req, res) => {
    try {
        const queries = await SearchQuery.find().sort({ count: -1 }).limit(10);
        res.json(queries);
    } catch (error) {
        res.status(500).send(error.message);
    }
});

module.exports = router;
```

**Frontend: Anzeigen der meistgesuchten Routen**

**Code:**
```jsx
// src/services/searchService.js
import axios from 'axios';

export const getMostSearchedRoutes = async () => {
    try {
        const response = await axios.get('/api/searchQueries');
        return response.data;
    } catch (error) {
        console.error('Error fetching search queries:', error);
        return [];
    }
};
```

**Komponente zur Anzeige der meistgesuchten Routen:**
```jsx
// src/components/MostSearchedRoutes.js
import React, { useEffect, useState } from 'react';
import { getMostSearchedRoutes } from '../services/searchService';

const MostSearchedRoutes = () => {
    const [routes, setRoutes] = useState([]);

    useEffect(() => {
        const fetchData = async () => {
            const data = await getMostSearchedRoutes();
            setRoutes(data);
        };
        fetchData();
    }, []);

    return (
        <div>
            <h2>Meistgesuchte Routen</h2>
            <ul>
                {routes.map((route, index) => (
                    <li key={index}>
                        {route.start} - {route.end} ({route.count} Mal gesucht)
                    </li>
                ))}
            </ul>
        </div>
    );
};

export default MostSearchedRoutes;
```

**Integration in die Hauptkomponente:**
```jsx
// src/App.js
import React, { useState } from 'react';
import AddressInput from './components/AddressInput';
import RouteDisplay from './components/RouteDisplay';
import MostSearchedRoutes from './components/MostSearchedRoutes';
import axios from 'axios';

const App = () => {
    const [start, setStart] = useState('');
    const [end, setEnd] = useState('');
    const [route, setRoute] = useState(null);

    const handleCalculate = async () => {
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
            <AddressInput label="Startort:" value={start} onChange={setStart} />
            <AddressInput label="Zielort:" value={end} onChange={setEnd} />
            <button onClick={handleCalculate}>Route berechnen</button>
            <RouteDisplay route={route} />
            <MostSearchedRoutes />
        </div>
    );
};

export default App;
```