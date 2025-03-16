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

## **Øvelse 4: Brug af config.env filer**

### **Metadata**
- **Formål**: Separere konfiguration fra kode.
- **Læringsmål**:
  - Forstå værdien af miljøfiler.
- **Relevans**: Best practice i DevOps.

### **Opgave**
1. Opret en `config.env`-fil:
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
   docker-compose up --env-file config.env -d
   ```

---

## **Bonus Opgave: Udarbejd Dit Eget Docker Compose Setup**

### **Metadata**
- **Formål**: Udfordre de studerende til at anvende deres viden til at designe deres egen løsning.
- **Læringsmål**:
  - Anvende Docker Compose til at bygge et funktionelt setup.
  - Strukturere services, netværk og persistens baseret på behov.
- **Relevans**: Selvstændig problemløsning er en vigtig del af DevOps-arbejde.

### **Opgave**
1. Design en `docker-compose.yml`-fil, der indeholder mindst tre services, f.eks.:
   - En applikation (kan være Node.js, Python, PHP eller andet efter eget valg).
   - En database (MySQL, PostgreSQL, MongoDB etc.).
   - En reverse proxy eller en cache (Nginx, Redis, eller Traefik).

2. Implementer `config.env`-filer til at håndtere konfiguration.

3. Brug `volumes` til at sikre persistens for databasen.

4. Start dit setup og verificér, at services kører korrekt med `docker ps` og `docker logs`.

5. Dokumentér din løsning med en kort README.md-fil, der forklarer din arkitektur og brug.

---
