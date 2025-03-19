# Kubernetes Øvelser med MiniKube og Docker Desktop

## Introduktion
Kubernetes er et open-source container-orkestreringssystem, der bruges til at automatisere deployment, skalering og administration af applikationer. For at lære Kubernetes lokalt bruger vi MiniKube, som opretter en enkelt-node Kubernetes-klynge på din computer.

Disse øvelser vil guide dig igennem de vigtigste operationer i Kubernetes og forklare, hvorfor hvert trin er nødvendigt.

---

## Øvelse 1: Installation af MiniKube på Docker Desktop

**Meta-data:**
- **Formål:** Forstå hvordan MiniKube opsættes til lokal Kubernetes-udvikling.
- **Relevans:** Kubernetes anvendes i produktionsmiljøer, men MiniKube giver en let måde at eksperimentere lokalt.
- **Forventet resultat:** MiniKube er installeret og kører en lokal Kubernetes-klynge.

### **Hvorfor gør vi dette?**
MiniKube giver en nem måde at køre en Kubernetes-klynge lokalt, så vi kan eksperimentere og teste uden en cloud-server.

### **Instruktioner**
1. Installer Docker Desktop, hvis det ikke allerede er installeret.
2. Aktiver Kubernetes i Docker Desktop via *Settings > Kubernetes*.
3. Installer MiniKube:
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
   minikube status
   kubectl get nodes
   ```
   Hvis en node vises, er MiniKube korrekt installeret.

### **Refleksion:**
- Hvorfor tror du, at MiniKube kører en Kubernetes-klynge på en enkelt node?
- Hvad betyder det, at vi bruger Docker som driver?

---

## Øvelse 2: Liste og beskrivelse af ressourcer

**Meta-data:**
- **Formål:** Lære at inspicere Kubernetes-ressourcer.
- **Relevans:** At kunne overvåge og forstå systemets tilstand er kritisk for fejlfinding.
- **Forventet resultat:** En liste af namespaces, pods og services samt detaljer om en specifik pod.

### **Hvorfor gør vi dette?**
Når vi arbejder med Kubernetes, er det vigtigt at kunne se, hvilke ressourcer der kører, så vi kan forstå systemets tilstand og fejlfinde eventuelle problemer.

### **Instruktioner**
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

### **Refleksion:**
- Hvilken type information får du fra `kubectl describe pod`?
- Hvordan kan dette hjælpe dig med at fejlfinde en kørende applikation?

---

## Øvelse 3: Oprettelse og anvendelse af ressourcer

**Meta-data:**
- **Formål:** Få praktisk erfaring med at oprette og anvende Kubernetes-ressourcer.
- **Relevans:** At kunne definere og anvende ressourcer er afgørende for udvikling og drift.
- **Forventet resultat:** En ny pod kører i Kubernetes.

### **Hvorfor gør vi dette?**
Kubernetes definerer ressourcer gennem YAML-filer, hvilket gør det nemt at versionere og reproducere applikationsopsætninger.

### **Instruktioner**
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

### **Refleksion:**
- Hvad betyder `apiVersion: v1` i YAML-filen?
- Hvorfor specificerer vi en containerPort i pod-definitionen?

---

## Øvelse 4: Udførelse af kommandoer i en pod

**Meta-data:**
- **Formål:** Lære hvordan man interagerer med en kørende pod.
- **Relevans:** Nødvendigt for fejlfinding og vedligeholdelse af applikationer.
- **Forventet resultat:** En bash-session åbnes i en Nginx-container.

### **Hvorfor gør vi dette?**
Når en applikation ikke virker som forventet, er det nyttigt at kunne tilgå containerens terminal for at tjekke konfigurationer og fejlsøge.

### **Instruktioner**
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

### **Refleksion:**
- Hvornår vil du bruge `kubectl exec` i praksis?
- Hvad er forskellen mellem `kubectl logs` og `kubectl exec`?

---

## Øvelse 5: Sletning af ressourcer

**Meta-data:**
- **Formål:** Forstå hvordan man fjerner Kubernetes-ressourcer.
- **Relevans:** At kunne rydde op i miljøet er vigtigt for at undgå unødvendige ressourceforbrug.
- **Forventet resultat:** Den tidligere oprettede pod bliver slettet.

### **Hvorfor gør vi dette?**
Ubrugte ressourcer kan forbruge unødig plads og CPU-kraft. Kubernetes håndterer automatisk ressourcestyring, men det er vigtigt at kunne fjerne overflødige pods og services.

### **Instruktioner**
1. Slet pod’en:
   ```bash
   kubectl delete pod nginx-pod
   ```
2. Bekræft sletningen:
   ```bash
   kubectl get pods
   ```

### **Refleksion:**
- Hvad sker der, hvis du prøver at tilgå en slettet pod?
- Hvordan kan du sikre, at en deployment automatisk genstarter en pod?

---

Denne opdaterede version giver mere kontekst og refleksionsopgaver, der hjælper de studerende med at forstå *hvorfor* de gør hvert trin.
