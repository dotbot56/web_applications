# Phase 6: Deployment und Wartung

## 1. Deployment

1. **Deployment-Setup auf Ubuntu-VM**

    - **Backend-Deployment:**
        - Öffnen Sie ein Terminal und verbinden Sie sich mit der Ubuntu-VM.

        ```bash
        ssh your-username@your-server-ip
        ```

        - Navigieren Sie zum Backend-Projektverzeichnis und installieren Sie alle Abhängigkeiten.

        ```bash
        cd path/to/route-calculator-backend
        npm install
        ```

        - Starten Sie das Backend mit einem Prozessmanager wie PM2, um sicherzustellen, dass es im Hintergrund läuft und bei Systemneustarts neu startet.

        ```bash
        npm install -g pm2
        pm2 start src/app.js --name "route-calculator-backend"
        pm2 startup
        pm2 save
        pm2 status
        ```

    - **Frontend-Deployment:**
        - Navigieren Sie zum Frontend-Projektverzeichnis und erstellen Sie den Produktions-Build.

        ```bash
        cd path/to/route-calculator
        npm install
        npm run build
        ```

        - Konfigurieren Sie Nginx, um den Build-Ordner als statische Dateien zu dienen.

        ```bash
        sudo nano /etc/nginx/sites-available/default
        ```

        - Bearbeiten Sie die Nginx-Konfigurationsdatei wie folgt:

        ```nginx
        server {
            listen 80;
            server_name your_domain_or_ip;

            location / {
                root /path/to/route-calculator/build;
                try_files $uri /index.html;
            }

            location /api/ {
                proxy_pass http://localhost:3001;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
            }
        }
        ```

        - Nginx neu starten:

        ```bash
        sudo systemctl restart nginx
        ```

2. **Code auf GitHub hochladen**

    - **Backend:**
        - Initialisieren eines Git-Repositorys im Backend-Projektverzeichnis und Pushen des Codes nach GitHub.

        ```bash
        cd path/to/route-calculator-backend
        git init
        git remote add origin https://github.com/dotbot56/web_applications.git
        git add .
        git commit -m "Initial commit - Backend"
        git push -u origin main
        ```

    - **Frontend:**
        - Initialisieren eines Git-Repositorys im Frontend-Projektverzeichnis und Pushen des Codes nach GitHub.

        ```bash
        cd path/to/route-calculator
        git init
        git remote add origin https://github.com/dotbot56/web_applications.git
        git add .
        git commit -m "Initial commit - Frontend"
        git push -u origin main
        ```

## 2. Dokumentation

1. **Erstellung einer Benutzer- und Entwicklerdokumentation**

    - **Benutzerdokumentation:**
        - Erstellen Sie eine README-Datei im Projektverzeichnis mit Anweisungen zur Verwendung der Anwendung.

        ```markdown
        # Routenberechnung App

        ## Verwendung

        1. Geben Sie den Startort und den Zielort ein.
        2. Klicken Sie auf "Route berechnen".
        3. Die berechnete Route wird angezeigt.

        ## Deployment

        Diese Anwendung ist auf einer Ubuntu-VM in Proxmox gehostet und nutzt Nginx als Webserver.

        ## Entwicklerdokumentation

        ### Installation

        1. Klonen Sie das Repository: `git clone https://github.com/dotbot56/web_applications.git`
        2. Installieren Sie die Abhängigkeiten im Backend und Frontend: `npm install`
        3. Starten Sie das Backend: `pm2 start src/app.js --name "route-calculator-backend"`
        ```


2. **Pflege der API-Dokumentation mit Swagger**

    - Stellen Sie sicher, dass die API-Dokumentation mit Swagger aktuell ist und alle Endpunkte beschreibt.
    - Fügen Sie ggf. Beispiele für Anfragen und Antworten hinzu.

    ```bash
    // src/swagger.js
    const swaggerJsDoc = require('swagger-jsdoc');
    const swaggerUi = require('swagger-ui-express');

    const swaggerOptions = {
        swaggerDefinition: {
            info: {
                title: 'Route Calculation API',
                version: '1.0.0',
                description: 'API zur Berechnung von Routen'
            }
        },
        apis: ['src/routes/*.js']
    };

    const swaggerDocs = swaggerJsDoc(swaggerOptions);

    module.exports = (app) => {
        app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocs));
    };
    ```