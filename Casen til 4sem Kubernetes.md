# Uge 15-18: Projektperiode (4 uger)

## Case: “KubeShop – en container-baseret webshop med microservices”

### Scenario:
I er et lille udviklerhold, der skal opsætte en container-baseret webshop. Forretningen sælger specialvarer online (valgfrit emne: kaffebønner, T-shirts, gadgets mv.).

Løsningen skal driftes i et Kubernetes-cluster og bestå af en række microservices, der sammen udgør webshoppen. Systemet skal minimum bestå af følgende services:

- **Produktservice**: Håndterer visning og administration af produkter.
- **Kundeservice**: Håndterer kundeoplysninger og login (hvis relevant).
- **Ordreservice**: Håndterer ordrer og købstransaktioner.

Derudover skal der være en samlet front-end, som kommunikerer med disse services via HTTP-kald. Kommunikation mellem services foregår direkte via REST.

Ingress benyttes som API gateway, så både brugere og systemet har adgang til de nødvendige endpoints udefra.

---

### 1. **Application Layer**
- En front-end (fx React, Vue eller en simpel HTML/JS-løsning), som henter og viser data fra de forskellige services.
- Tre back-end microservices, fx i .NET Core, Java Spring Boot eller Node.js:
  - Produktservice
  - Kundeservice
  - Ordreservice

### 2. **Database**
- Kørsel af en database (PostgreSQL, MySQL eller lign.) som container.
- Persistent storage til databasen (volumes).

### 3. **Kubernetes Deployment**
- Opsætning af Deployments og Services.
- Opsætning af automatisk skalering med HPA (Horizontal Pod Autoscaler).
- RBAC og netværkspolitikker som minimum for at sikre clusteret.

### 4. **CI/CD Pipeline**
- Pipeline der automatisk bygger, tester og udruller jeres applikation i Kubernetes.

---

## Bonusopgaver:
- Simpel JWT-baseret login til fx admin eller brugerpanel.
- Implementering af Ingress for ekstern adgang til applikationen.
- Prometheus og Grafana til overvågning.
- Mulighed for centraliseret logging (ELK/EFK).

---

## Projektkrav:
- I skal kunne dokumentere og demonstrere jeres API-endpoints, fx med Postman.
  - Lav en Postman Collection med eksempler på kald til jeres API (GET, POST, PUT, DELETE).
  - Brug evt. Postmans testfunktioner til at validere respons.
  - Medsend en kort API-dokumentation, der beskriver:
    - Tilgængelige endpoints (URL, metode, formål)
    - Input/output (fx body, headers, statuskoder)
    - Evt. autentificering, hvis anvendt (JWT).
- I skal kunne demonstrere hele flowet: fra kode til containerimage til udrulning i Kubernetes.
- Applikationen skal være tilgængelig via en webadresse (Ingress, hvis valgt).
- I skal præsentere, hvordan I håndterer skaleringsscenarier.
- I skal levere en kort dokumentation (eller præsentation), der beskriver arkitektur, teknologier og best practices.

---

## Minikube Cheatsheet (til udvikling og test)

```bash
# Start Minikube
minikube start

# Aktivér Ingress-controller
minikube addons enable ingress

# Aktivér metrics-server (for HPA)
minikube addons enable metrics-server

# Åbn Minikube Dashboard (valgfrit)
minikube dashboard

# Kør tunnel for at eksponere Ingress lokalt
minikube tunnel

# Tjek status
kubectl get ingress
kubectl get pods
kubectl get svc
kubectl get hpa
```

**Tips:**
- Tilføj Ingress hostnavn til din `/etc/hosts` fil med IP'en fra `minikube ip`
- Pods skal definere `resources.requests` og `resources.limits`, før HPA virker

## Overblik over arkitektur

```
                   [ Brugerens Browser ]
                           |
                           v
                    +------------------+
                    |     Frontend     |
                    +------------------+
                           |
                           v
                    +--------------+
                    |   Ingress    |   (API Gateway)
                    +--------------+
                      |    |    |
           ------------    |    -------------
          |                |                 |
          v                v                 v
+----------------+  +----------------+  +----------------+
|  Produktservice|  |  Kundeservice  |  |  Ordreservice  |
+----------------+  +----------------+  +----------------+
       |                    |                   |
       v                    v                   v
+------------+      +------------+       +-------------+
|  Database  |      |  Database  |       |  Database   |
+------------+      +------------+       +-------------+

   (Frontend henter og sender data til microservices via Ingress)
```

## Forslag til forløb i projektperioden:

**Eksamen afholdes i uge 23, torsdag og fredag.**

### Uge 15:
- Planlægning og opsætning af grundlæggende infrastruktur (repo, Dockerfiles, docker-compose lokalt, hvis nødvendigt).

### Uge 16:
- Opsætning af Kubernetes-manifester (deployments, services), konfiguration af secrets, volumes osv.

### Uge 17:
- Implementering af CI/CD pipeline, test og udrulning til cluster.
- Implementering af Ingress og evt. overvågning og logging (bonus).

### Uge 18:
- Aflevering af projekt senest kl. 12:00 på Wiseflow:
  1) En rapport på 5-10 sider (skal indeholde systemarkitekturen).
  2) YAML-fil(er) til Kubernetes deployment.
  3) Eventuelle Dockerfile(s).
- 

