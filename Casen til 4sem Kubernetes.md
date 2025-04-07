# Uge 15-18: Projektperiode (4 uger)

## Case: “KubeShop – en container-baseret webshop”

### Scenario:

I er et lille udviklerhold, der skal opsætte en container-baseret webshop. Forretningen sælger specialvarer online (valgfrit emne: kaffebønner, T-shirts, gadgets mv.).

Løsningen skal driftes i et Kubernetes-cluster og bestå af en samlet applikation med en front-end og en back-end, der håndterer både visning af produkter og simple købstransaktioner.

Frontend eksponeres udadtil via en NodePort service i Minikube, som giver adgang til applikationen gennem Minikubes IP-adresse og en specifik port.

Frontend eksponeres udadtil via en NodePort service i Minikube, som giver adgang til applikationen gennem Minikubes IP-adresse og en specifik port.

---

### 1. **Application Layer**

- En front-end (fx React, Vue eller en simpel HTML/JS-løsning), som henter og viser data.
- En samlet back-end (fx i .NET Core, Java Spring Boot eller Node.js), der håndterer produktvisning, kundeoplysninger og køb.

### 2. **Database**

- Det anbefales at bruge en ekstern database (fx hostet lokalt eller i cloud).
- Hvis I vælger at køre en database i Minikube, bør det være som en simpel container uden clustering eller replication.
- Undgå Stateful Sets og avanceret storage – det er for komplekst til Minikube og ikke nødvendigt for dette projekt.

### 3. **Kubernetes Deployment**

- Opsætning af Deployments og Services.
- Opsætning af automatisk skalering med HPA (Horizontal Pod Autoscaler).

### 4. **CI/CD Pipeline**

- Pipeline der automatisk bygger, tester og udruller jeres applikation i Kubernetes.

---

## Bonusopgaver:

- Simpel JWT-baseret login til fx admin eller brugerpanel.
- Prometheus og Grafana til overvågning.
- Mulighed for centraliseret logging (ELK/EFK).
- Analyse og monitorering med **Datadog** (kræver GitHub Student-konto):
  - Ansøg om GitHub Student Developer Pack: https://education.github.com/pack
  - Når du er godkendt, ansøg om gratis Datadog-konto her: https://www.datadoghq.com/education/ (kræver GitHub-login).
  - Følg Datadogs vejledning til Kubernetes-integration: https://docs.datadoghq.com/agent/kubernetes/?tab=helm

---

## Projektkrav:

**Afleveringskrav:**
- Der skal uploades to filer til Wiseflow:
  1. En rapport (PDF) på 5–10 sider, som beskriver projektet og indeholder systemarkitekturen.
  2. En zip-fil med jeres YAML-filer til Kubernetes (og evt. Dockerfiles).

- I skal kunne dokumentere og demonstrere jeres API-endpoints, fx med Postman.
  - Lav en Postman Collection med eksempler på kald til jeres API (GET, POST, PUT, DELETE).
  - Brug evt. Postmans testfunktioner til at validere respons.
  - Medsend en kort API-dokumentation, der beskriver:
    - Tilgængelige endpoints (URL, metode, formål)
    - Input/output (fx body, headers, statuskoder)
- I skal kunne demonstrere hele flowet: fra kode til containerimage til udrulning i Kubernetes.
- Applikationen skal være tilgængelig via browseren gennem Minikube (fx via NodePort).
- I skal kunne vise, hvordan I har konfigureret skalering med HPA (valgfrit, hvis det er nået).
- I skal levere en kort dokumentation (eller præsentation), der beskriver arkitektur, teknologier og best practices.

---

## Minikube Cheatsheet (til udvikling og test)

```bash
# Start Minikube
minikube start

# Aktivér metrics-server (for HPA)
minikube addons enable metrics-server

# Åbn Minikube Dashboard (valgfrit)
minikube dashboard

# Find IP og port til NodePort service
minikube service [service-navn] --url

# Tjek status
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
                   +---------------------+
                   |  Webshop Backend    |
                   +---------------------+
                           |
                           v
                   +------------------+
                   |     Database     |
                   +------------------+

(Frontend kommunikerer med backend via NodePort service, som eksponeres i Minikube)
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
  1. En rapport på 5-10 sider (skal indeholde systemarkitekturen).
  2. YAML-fil(er) til Kubernetes deployment.
  3. Eventuelle Dockerfile(s).
-

