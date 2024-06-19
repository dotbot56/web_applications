# Phase 5: Tests und Validierung

## 1. Unit-Tests

**Unit-Tests für die Frontend-Komponenten**
- Verwenden von Jest und React Testing Library für Unit-Tests der Frontend-Komponenten.

**Code:**
```jsx
// src/components/AddressInput.test.js
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import AddressInput from './AddressInput';

test('renders AddressInput component', () => {
    render(<AddressInput label="Startort:" value="" onChange={() => {}} />);
    const inputElement = screen.getByLabelText(/Startort:/i);
    expect(inputElement).toBeInTheDocument();
});

test('calls onChange when input changes', () => {
    const handleChange = jest.fn();
    render(<AddressInput label="Startort:" value="" onChange={handleChange} />);
    const inputElement = screen.getByLabelText(/Startort:/i);
    fireEvent.change(inputElement, { target: { value: 'Berlin' } });
    expect(handleChange).toHaveBeenCalledWith('Berlin');
});
```

**Unit-Tests für die Backend-Funktionen**
- Verwenden von Jest für Unit-Tests der Backend-Funktionen.

**Code:**
```javascript
// src/routes/route.test.js
const request = require('supertest');
const express = require('express');
const route = require('./route');
const app = express();

app.use(express.json());
app.use('/api/routes', route);

test('POST /api/routes should calculate route', async () => {
    const response = await request(app)
        .post('/api/routes')
        .send({ start: { lng: 13.405, lat: 52.52 }, end: { lng: 13.381, lat: 52.53 } });

    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('routes');
});
```

## 2. Akzeptanztests

**Schreiben und Ausführen von Akzeptanztests zur Validierung der UI-Funktionalitäten**
- Verwenden von Cypress für Akzeptanztests.

**Code:**
```javascript
// cypress/integration/app.spec.js
describe('Route Calculation App', () => {
    it('should display the title', () => {
        cy.visit('/');
        cy.contains('h1', 'Routenberechnung');
    });

    it('should allow user to calculate a route', () => {
        cy.visit('/');
        cy.get('input[aria-label="Startort:"]').type('Berlin');
        cy.get('input[aria-label="Zielort:"]').type('München');
        cy.contains('button', 'Route berechnen').click();
        cy.contains('Berechnete Route');
    });
});
```

## 3. Integrationstests

**Testen der Integration zwischen Frontend und Backend**
- Sicherstellen, dass das Frontend korrekt mit dem Backend kommuniziert und die API-Endpunkte korrekt funktionieren.

**Code:**
```javascript
// src/integration/integration.test.js
const request = require('supertest');
const express = require('express');
const route = require('./routes/route');
const address = require('./routes/address');
const app = express();

app.use(express.json());
app.use('/api/routes', route);
app.use('/api/addresses', address);

test('GET /api/addresses should return suggestions', async () => {
    const response = await request(app)
        .get('/api/addresses')
        .query({ query: 'Ber' });

    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('features');
});

test('POST /api/routes should return a calculated route', async () => {
    const response = await request(app)
        .post('/api/routes')
        .send({ start: { lng: 13.405, lat: 52.52 }, end: { lng: 13.381, lat: 52.53 } });

    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('routes');
});
```