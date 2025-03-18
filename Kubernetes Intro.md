**Titel: Introduktion til Kubernetes og dets Funktioner**

---

# **Slide 1: Velkommen til Kubernetes**
**Agenda:**
- Hvad er Kubernetes?
- Hvorfor bruge Kubernetes?
- Grundlæggende koncepter og funktioner
- Forberedelse til øvelserne

**Intro:**
Kubernetes er en open-source platform, der automatiserer udrulning, skalering og drift af containeriserede applikationer.

---

# **Slide 2: Hvad er Kubernetes?**
- Et orkestreringsværktøj til containere (Docker, Containerd, etc.).
- Håndterer deployment, skalering og vedligeholdelse af applikationer.
- Understøtter automatisk failover og self-healing af applikationer.

---

# **Slide 3: Hvorfor bruge Kubernetes?**
- **Automatisering:** Automatiserer deployment og administration af applikationer.
- **Skalering:** Kan skaleres op og ned baseret på trafikbelastning.
- **Fejltolerance:** Overvåger og genstarter fejlende containere automatisk.
- **Bærbarhed:** Applikationer kan køre ensartet på tværs af miljøer (lokalt, cloud, hybrid).

---

# **Slide 4: Kubernetes Arkitektur**
Kubernetes består af flere nøglekomponenter:
- **Master Node:** Kontrollerer og administrerer klyngen.
  - **API Server**: Kommunikationspunkt for alle kommandoer.
  - **Controller Manager**: Overvåger tilstanden af pods og noder.
  - **Scheduler**: Fordeler workloads til de rigtige noder.
- **Worker Nodes:** Kører applikationerne.
  - **Kubelet:** Agent, der håndterer pods på en node.
  - **Kube Proxy:** Håndterer netværkstrafik til og fra pods.
  - **Container Runtime:** F.eks. Docker, der kører selve containerne.

## **kubectl – Kubernetes' CLI-værktøj**
- **kubectl** er kommando-linje værktøjet til at interagere med Kubernetes.
- Bruges til at administrere pods, services, deployments og andre ressourcer.
- Eksempler:
  - **Opret en pod:** `kubectl run mypod --image=nginx`
  - **Vis aktive pods:** `kubectl get pods`
  - **Se detaljer om en pod:** `kubectl describe pod mypod`
  - **Slet en pod:** `kubectl delete pod mypod`

---

# **Slide 5: Kubernetes Objekter**
De vigtigste objekter i Kubernetes:
- **Pods:** Den mindste deploybare enhed i Kubernetes.
- **Deployments:** Automatiserer oprettelse og skalering af pods.
- **Services:** Eksponerer pods internt eller eksternt.
- **ConfigMaps & Secrets:** Gemmer konfigurationsdata og følsomme oplysninger.
- **Namespaces:** Opdeling af ressourcer i separate miljøer.

---

# **Slide 6: Kubernetes i Praksis – Grundlæggende kubectl Kommandoer**
- Start en Minikube-klynge:
  ```bash
  minikube start
  ```
- Tjek klyngeinformation:
  ```bash
  kubectl cluster-info
  kubectl get nodes
  ```
- Deploy en simpel Nginx container:
  ```bash
  kubectl run mypod --image=nginx
  ```
- Tjek status på Pods:
  ```bash
  kubectl get pods
  ```
- Beskriv en Pod:
  ```bash
  kubectl describe pod mypod
  ```
- Hent logs fra en Pod:
  ```bash
  kubectl logs mypod
  ```
- Slet en Pod:
  ```bash
  kubectl delete pod mypod
  ```

---

# **Slide 7: Deployment af en Database – MongoDB Case**
- Vi opretter en MongoDB Deployment:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: mongodb
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: mongodb
    template:
      metadata:
        labels:
          app: mongodb
      spec:
        containers:
        - name: mongodb
          image: mongo:latest
          ports:
          - containerPort: 27017
  ```
- Vi eksponerer databasen som en service:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: mongodb-service
  spec:
    selector:
      app: mongodb
    ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
  ```

---

# **Slide 8: Opsætning af Secrets**
- Kubernetes Secrets gemmer følsomme data som database-brugernavne og adgangskoder:
  ```bash
  kubectl create secret generic mongodb-secret \
    --from-literal=mongo-root-username=mongouser \
    --from-literal=mongo-root-password=password
  ```
- Vi kan referere til disse secrets i vores deployment YAML:
  ```yaml
  env:
  - name: MONGO_INITDB_ROOT_USERNAME
    valueFrom:
      secretKeyRef:
        name: mongodb-secret
        key: mongo-root-username
  - name: MONGO_INITDB_ROOT_PASSWORD
    valueFrom:
      secretKeyRef:
        name: mongodb-secret
        key: mongo-root-password
  ```

---

# **Slide 9: Adgang til Mongo Express**
- Mongo Express UI giver en web-interface til MongoDB.
- Deployment YAML:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: mongo-express
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: mongo-express
    template:
      metadata:
        labels:
          app: mongo-express
      spec:
        containers:
        - name: mongo-express
          image: mongo-express
          ports:
          - containerPort: 8081
          env:
          - name: ME_CONFIG_MONGODB_SERVER
            value: "mongodb-service"
  ```
- Eksponer Mongo Express:
  ```bash
  kubectl expose deployment mongo-express --type=NodePort --port=8081
  ```
- Find URL’en:
  ```bash
  minikube service mongo-express --url
  ```

---

# **Slide 10: Konklusion og Forberedelse til Øvelserne**
- Kubernetes hjælper med automatisering af containeriserede applikationer.
- Vi har set hvordan:
  - Pods, Deployments og Services fungerer.
  - Hvordan Secrets beskytter følsomme oplysninger.
  - Hvordan vi eksponerer MongoDB med en UI.
- **Forberedelse:**
  - Sørg for, at Minikube er installeret.
  - Øv de grundlæggende kommandoer.
  - Læs YAML-filerne for at forstå opbygningen.

**Lad os komme i gang med øvelserne! 🚀**

