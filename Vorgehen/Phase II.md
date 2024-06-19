### Phase 2: Setup und Initialisierung (angepasst)

## 1. Projektstruktur und Setup

1. **WebStorm-Projekt einrichten**:
   - Öffnen von WebStorm und Erstellen eines neuen Projekts.
   - Sicherstellen, dass die erforderlichen Plugins für React.js, Node.js und Datenbankintegration in WebStorm installiert sind. Dies kann über die Einstellungen unter `File -> Settings -> Plugins` überprüft und installiert werden.


2. **Git in WebStorm konfigurieren**:
   - Initialisieren eines lokalen Git-Repositorys:
     ```bash
     git init
     ```
   - Erstellen eines neuen Repositorys auf GitHub und Hinzufügen als Remote-Repository:
     ```bash
     git remote add origin https://github.com/dotbot56/web_applications.git
     ```

3. **.gitignore-Datei erstellen**:
   - Erstellen einer `.gitignore`-Datei im Hauptverzeichnis des Projekts, um unnötige Dateien vom Git-Tracking auszuschließen:
     ```text
     node_modules/
     .env
     ```

## 2. Frontend und Backend Initialisierung

1. **Frontend Setup:**
   - Initialisieren eines neuen Projekts mit Create React App:
     ```bash
     npx create-react-app route-calculator
     cd route-calculator
     npm install axios react-router-dom bootstrap
     ```
   - Öffnen des Projekts in WebStorm und Sicherstellen, dass alles korrekt eingerichtet ist.

2. **Backend Setup:**
   - Initialisieren eines neuen Projekts mit Express:
     ```bash
     mkdir route-calculator-backend
     cd route-calculator-backend
     npm init -y
     npm install express axios swagger-ui-express
     ```
   - Öffnen des Projekts in WebStorm und Sicherstellen, dass alles korrekt eingerichtet ist.

3. **Ordnerstruktur einrichten:**

   **Frontend:**
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

   **Backend:**
   ```
   /src
     /controllers
     /models
     /routes
     /services
     app.js
     server.js
   ```

## 3. Deployment-Setup

1. **Ubuntu-VM in Proxmox einrichten:**
   - Erstellen einer neuen VM in Proxmox mit Ubuntu als Betriebssystem.
   - Sicherstellen, dass die VM in der DMZ (Demilitarized Zone) eingerichtet ist, um sie sicher von anderen Netzwerken zu trennen.


2. **Nginx und Node.js installieren:**
   - Auf der Ubuntu-VM Nginx installieren:
     ```bash
     sudo apt update
     sudo apt install nginx
     ```
   - Node.js und npm installieren:
     ```bash
     curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
     sudo apt install -y nodejs
     ```

3. **Projekt auf die Ubuntu-VM kopieren:**
   - Lokales Projektverzeichnis mit GitHub synchronisieren:
     ```bash
     git add .
     git commit -m "Initial commit"
     git push origin main
     ```
   - Auf der Ubuntu-VM das Projekt klonen:
     ```bash
     git clone https://github.com/dotbot56/web_applications.git
     cd web_applications
     cd route-calculator
     npm install
     cd ../route-calculator-backend
     npm install
     ```

4. **Nginx konfigurieren:**
   - Nginx konfigurieren, um das Frontend zu bedienen und Anfragen an das Backend weiterzuleiten.
   - Beispielkonfiguration:
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

5. **Backend starten:**
   - Das Backend auf Port 3001 starten:
     ```bash
     cd route-calculator-backend
     node src/app.js
     ```

