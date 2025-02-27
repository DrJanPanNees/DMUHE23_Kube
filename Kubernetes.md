# Kubernetes Øvelser med MiniKube og Docker Desktop

## Øvelse 1: Installation af MiniKube på Docker Desktop

**Meta-data:**
- **Formål:** Forstå hvordan MiniKube opsættes til lokal Kubernetes-udvikling.
- **Relevans:** Kubernetes anvendes i produktionsmiljøer, men MiniKube giver en let måde at eksperimentere lokalt.
- **Forventet resultat:** MiniKube er installeret og kører en lokal Kubernetes-klynge.

### Instruktioner
1. Installer Docker Desktop, hvis det ikke allerede er installeret.
2. Aktiver Kubernetes i Docker Desktop via *Settings > Kubernetes*.
3. Installer MiniKube ved at køre følgende kommando:
   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   ```
   *(For Windows-brugere, brug Chocolatey eller Scoop til at installere MiniKube.)*
4. Start MiniKube med Docker som driver:
   ```bash
   minikube start --driver=docker
   ```
5. Bekræft installationen:
   ```bash
   kubectl get nodes
   ```
   Hvis en node vises, er MiniKube korrekt installeret.

---

## Øvelse 2: Liste og beskrivelse af ressourcer

**Meta-data:**
- **Formål:** Lære at inspicere Kubernetes-ressourcer.
- **Relevans:** At kunne overvåge og forstå systemets tilstand er kritisk for fejlfinding.
- **Forventet resultat:** En liste af namespaces, pods og services samt detaljer om en specifik pod.

### Instruktioner
1. Liste alle namespaces:
   ```bash
   kubectl get namespaces
   ```
2. Liste alle pods:
   ```bash
   kubectl get pods
   ```
3. Liste alle services:
   ```bash
   kubectl get svc
   ```
4. Beskriv en specifik pod:
   ```bash
   kubectl describe pod [pod-name]
   ```
   *(Udskift `[pod-name]` med en faktisk pod fra trin 2.)*

---

## Øvelse 3: Oprettelse og anvendelse af ressourcer

**Meta-data:**
- **Formål:** Få praktisk erfaring med at oprette og anvende Kubernetes-ressourcer.
- **Relevans:** At kunne definere og anvende ressourcer er afgørende for udvikling og drift.
- **Forventet resultat:** En ny pod kører i Kubernetes.

### Instruktioner
1. Opret en YAML-fil (`nginx-pod.yaml`) med følgende indhold:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginx-pod
   spec:
     containers:
     - name: nginx
       image: nginx:latest
       ports:
       - containerPort: 80
   ```
2. Anvend ressourcen:
   ```bash
   kubectl apply -f nginx-pod.yaml
   ```
3. Bekræft oprettelsen:
   ```bash
   kubectl get pods
   ```
4. Beskriv pod’en:
   ```bash
   kubectl describe pod nginx-pod
   ```

---

## Øvelse 4: Udførelse af kommandoer i en pod

**Meta-data:**
- **Formål:** Lære hvordan man interagerer med en kørende pod.
- **Relevans:** Nødvendigt for fejlfinding og vedligeholdelse af applikationer.
- **Forventet resultat:** En bash-session åbnes i en Nginx-container.

### Instruktioner
1. Liste kørende pods:
   ```bash
   kubectl get pods
   ```
2. Kør en interaktiv shell i pod’en:
   ```bash
   kubectl exec -it nginx-pod -- /bin/bash
   ```
3. Inde i bash-terminalen, kør:
   ```bash
   ls /usr/share/nginx/html
   ```
4. Afslut sessionen:
   ```bash
   exit
   ```

---

## Øvelse 5: Sletning af ressourcer

**Meta-data:**
- **Formål:** Forstå hvordan man fjerner Kubernetes-ressourcer.
- **Relevans:** At kunne rydde op i miljøet er vigtigt for at undgå unødvendige ressourceforbrug.
- **Forventet resultat:** Den tidligere oprettede pod bliver slettet.

### Instruktioner
1. Slet pod’en:
   ```bash
   kubectl delete pod nginx-pod
   ```
2. Bekræft sletningen:
   ```bash
   kubectl get pods
   ```
   *(Pod’en bør ikke længere være til stede.)*

---

## Øvelse 6: Udskrivning af logs fra en pod

**Meta-data:**
- **Formål:** Lære at tilgå logs fra en kørende applikation.
- **Relevans:** Logs er afgørende for at fejlsøge og overvåge systemet.
- **Forventet resultat:** Logfiler fra en pod vises i terminalen.

### Instruktioner
1. Hvis en Nginx-pod stadig kører, se dens logs:
   ```bash
   kubectl logs nginx-pod
   ```
2. Hvis pod’en er slettet, opret den igen og gentag trin 1.

---

## Øvelse 7: Modificering af kubeconfig-filer

**Meta-data:**
- **Formål:** Forstå kubeconfig og hvordan man skifter mellem Kubernetes-klynger.
- **Relevans:** Ved arbejde med flere Kubernetes-miljøer er det nødvendigt at kunne skifte kontekst.
- **Forventet resultat:** Den aktive kubeconfig-konfiguration vises og kan ændres.

### Instruktioner
1. Se den aktuelle kontekst:
   ```bash
   kubectl config current-context
   ```
2. Liste alle tilgængelige konfigurationer:
   ```bash
   kubectl config get-contexts
   ```
3. Hvis flere konfigurationer er tilgængelige, skift til en anden kontekst:
   ```bash
   kubectl config use-context [context-name]
   ```
   *(Udskift `[context-name]` med en tilgængelig kontekst fra trin 2.)*

---

Disse øvelser giver et godt grundlag for at lære Kubernetes ved hjælp af MiniKube og Docker Desktop. De dækker essentielle aspekter som installation, ressourcestyring, fejlfinding og kubeconfig-administration.

