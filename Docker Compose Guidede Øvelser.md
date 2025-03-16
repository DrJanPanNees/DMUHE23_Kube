# Docker Compose: Guidede Øvelser

## **Indledning**
Dette dokument indeholder en række guidede øvelser i Docker Compose, baseret på undervisningsmaterialet. Hver øvelse indeholder metadata for at forklare formål, læringsmål og relevans.

---

## **Øvelse 1: Opsætning af en simpel Docker Compose-fil**

### **Metadata**
- **Formål**: Introduktion til Docker Compose og orkestrering af flere containere.
- **Læringsmål**:
  - Forstå strukturen af en `docker-compose.yml`-fil.
  - Lære hvordan man definerer en simpel webserver og database.
- **Relevans**: Evnen til at definere services i YAML er essentielt for DevOps og cloud deployment.

### **Opgave**
1. Opret en ny mappe:
   ```sh
   mkdir docker-compose-demo
   cd docker-compose-demo
   ```

2. Opret en **docker-compose.yml**-fil:
   ```yaml
   version: '3.8'

   services:
     web:
       image: nginx
       ports:
         - "8080:80"

     database:
       image: mysql:8
       environment:
         MYSQL_ROOT_PASSWORD: rootpassword
         MYSQL_DATABASE: mydb
         MYSQL_USER: user
         MYSQL_PASSWORD: password
       ports:
         - "3306:3306"
   ```

3. Start applikationen:
   ```sh
   docker-compose up -d
   ```

4. Tjek, at containerne kører:
   ```sh
   docker ps
   ```

5. Åbn en browser og gå til `http://localhost:8080`.

6. Stop og slet containerne:
   ```sh
   docker-compose down
   ```

---

## **Øvelse 2: Tilføj en Node.js API**

### **Metadata**
- **Formål**: Demonstrere integration mellem en applikation og en database.
- **Læringsmål**:
  - Forstå hvordan services interagerer.
  - Lære at bruge `depends_on`.
- **Relevans**: Almindelig opsætning for webapplikationer.

### **Opgave**
1. Opret en mappe til app'en:
   ```sh
   mkdir app
   cd app
   ```

2. Opret en **server.js**-fil:
   ```js
   const express = require('express');
   const app = express();

   app.get('/', (req, res) => {
       res.send('Hello from Node.js running in Docker!');
   });

   app.listen(3000, () => {
       console.log('Server running on port 3000');
   });
   ```

3. Opret en **Dockerfile**:
   ```Dockerfile
   FROM node:18
   WORKDIR /app
   COPY . .
   RUN npm install express
   CMD ["node", "server.js"]
   ```

4. Opdater **docker-compose.yml**:
   ```yaml
   services:
     app:
       build: ./app
       ports:
         - "3000:3000"
       depends_on:
         - database
   ```

5. Start systemet igen:
   ```sh
   docker-compose up --build -d
   ```

6. Besøg `http://localhost:3000`.

---

## **Øvelse 3: Tilføj Persistent Storage**

### **Metadata**
- **Formål**: Forstå brugen af Docker Volumes.
- **Læringsmål**:
  - Lære at gemme data i volumener.
- **Relevans**: Databaser i produktion kræver persistens.

### **Opgave**
1. Opdater **docker-compose.yml**:
   ```yaml
   volumes:
     db_data:
   
   services:
     database:
       volumes:
         - db_data:/var/lib/mysql
   ```

2. Start igen:
   ```sh
   docker-compose up -d
   ```

3. Stop og start containeren og tjek, at data stadig er der.

---

## **Øvelse 4: Brug af .env filer**

### **Metadata**
- **Formål**: Separere konfiguration fra kode.
- **Læringsmål**:
  - Forstå værdien af miljøfiler.
- **Relevans**: Best practice i DevOps.

### **Opgave**
1. Opret en `.env`-fil:
   ```env
   MYSQL_ROOT_PASSWORD=rootpassword
   MYSQL_DATABASE=mydb
   MYSQL_USER=user
   MYSQL_PASSWORD=password
   ```

2. Opdater **docker-compose.yml**:
   ```yaml
   environment:
     MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
   ```

3. Start igen:
   ```sh
   docker-compose up --env-file .env -d
   ```

---

## **Bonus Opgave: Load Balancing med Nginx**

### **Metadata**
- **Formål**: Demonstrere hvordan Nginx kan bruges til load balancing.
- **Læringsmål**:
  - Lære at konfigurere Nginx som en reverse proxy.
  - Lære at sætte flere instanser op af en service.
- **Relevans**: Load balancing er afgørende for skalerbare applikationer.

### **Opgave**
1. Opret en **nginx.conf**:
   ```nginx
   upstream backend {
       server app1:3000;
       server app2:3000;
   }
   server {
       listen 80;
       location / {
           proxy_pass http://backend;
       }
   }
   ```

2. Opdater **docker-compose.yml**:
   ```yaml
   services:
     app1:
       build: ./app
     app2:
       build: ./app
     nginx:
       image: nginx
       volumes:
         - ./nginx.conf:/etc/nginx/nginx.conf
       ports:
         - "8080:80"
   ```

3. Start systemet:
   ```sh
   docker-compose up -d
   ```

4. Test load balancing ved at lave flere requests.

---

## **Afslutning**
Disse øvelser giver praktisk erfaring med Docker Compose og inkluderer en bonusopgave for avancerede studerende.
