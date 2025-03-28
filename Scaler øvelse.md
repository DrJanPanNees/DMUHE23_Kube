# Kubernetes Øvelser

## Øvelse 1: Skalering af en applikation i Kubernetes

**Sværhedsgrad:** Begynder  
**Estimeret tid:** 10 minutter

### Introduktion

I denne øvelse vil du lære at skalere en deployment i Kubernetes ved hjælp af `kubectl scale`-kommandoen og observere load balancing i aktion.

### Forudsætning: Opret en deployment

Før du kan skalere applikationen, skal du have en deployment kørende. Denne opgave forudsætter, at du har en kørende Minikube-klynge og adgang til `kubectl`-kommandolinjeværktøjet. Du opretter deployment'en med følgende kommando, som opretter en simpel Node.js-baseret applikation:

```sh
kubectl create deployment kubernetes-bootcamp --image=gcr.io/k8s-minikube/kubernetes-bootcamp:v1
```

Eksponer derefter deployment som en service, så den kan tilgås udefra via en tilfældig tildelt port (NodePort-type). Dette trin er nødvendigt for at teste kommunikationen med applikationen og se effekten af skalering og opdateringer:

```sh
kubectl expose deployment kubernetes-bootcamp --type=NodePort --port=8080
```

### Trin 1: Skalering af en deployment

1. **Liste over deployments:**  
En deployment i Kubernetes definerer, hvordan applikationens pods skal oprettes og vedligeholdes. Med følgende kommando kan du se eksisterende deployments i dit cluster og kontrollere, hvor mange replikaer der er aktive, samt om de kører korrekt.
   ```sh
   kubectl get deployments
   ```
   Outputtet bør ligne:
   ```
   NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
   kubernetes-bootcamp   1/1     1            1           11m
   ```

2. **Skalér deployment til 4 replikaer:**
   ```sh
   kubectl scale deployments/kubernetes-bootcamp --replicas=4
   ```

3. **Bekræft skaleringen:**
   ```sh
   kubectl get deployments
   ```

4. **Liste over pods:**  
Når du lister pods med `-o wide`, får du udvidet information, bl.a. hvilke noder pods kører på, deres interne IP-adresser og hvilken container image de bruger. Dette giver bedre overblik, især når du arbejder med flere replikaer eller vil observere fordeling på tværs af noder.
   ```sh
   kubectl get pods -o wide
   ```

### Trin 2: Load Balancing

1. **Find den eksponerede IP og port:**  
Kubernetes tildeler automatisk en NodePort til din service. Ved at beskrive servicen kan du finde denne port samt IP-adressen til din Minikube-instans. Dette er vigtigt for at kunne teste din applikation udefra og observere, hvordan forespørgsler distribueres mellem pods.
   ```sh
   kubectl describe services/kubernetes-bootcamp
   ```

2. **Sæt en miljøvariabel for NODE_PORT:**  
Ved hjælp af `kubectl` og Go-templates kan du udtrække NodePort-værdien direkte fra service-definitionen. Dette gemmes i en miljøvariabel, så du nemt kan genbruge portnummeret i efterfølgende `curl`-kommandoer. Det gør det også lettere at automatisere test eller scriptbaserede kald til din applikation.  
For at kunne sende HTTP-forespørgsler til din applikation fra din lokale maskine, skal du kende den port, som Kubernetes har tildelt din service. Denne port gemmer vi i en miljøvariabel kaldet `NODE_PORT`, så vi nemt kan bruge den i efterfølgende kommandoer. Brug følgende kommandoer:
   ```sh
   export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
   echo "Service kører på port: $NODE_PORT"
   ```

3. **Test load balancing ved at sende flere forespørgsler:**  
Ved at sende gentagne HTTP-forespørgsler til applikationen kan du observere, hvordan trafikken bliver fordelt mellem de forskellige pods. Dette demonstrerer Kubernetes' indbyggede load balancing-mekanisme, hvor hver forespørgsel potentielt rammer en ny pod via service-ressourcen.
   ```sh
   curl $(minikube ip):$NODE_PORT
   ```

### Trin 3: Skalering ned

1. **Skalér deployment ned til 2 replikaer:**  
Når du reducerer antallet af replikaer i din deployment, vil Kubernetes automatisk terminere de overskydende pods, så det samlede antal matcher det angivne. Dette er nyttigt for at spare ressourcer eller simulere ændringer i efterspørgsel.
   ```sh
   kubectl scale deployments/kubernetes-bootcamp --replicas=2
   ```

2. **Bekræft skaleringen:**
   ```sh
   kubectl get deployments
   ```

3. **Liste over pods:**
   ```sh
   kubectl get pods -o wide
   ```

[Se original tutorial (Module 5)](https://minikube.sigs.k8s.io/docs/tutorials/kubernetes_101/module5/)

---

## Øvelse 2: Opdatering af en applikation i Kubernetes

**Sværhedsgrad:** Begynder  
**Estimeret tid:** 10 minutter

### Introduktion

I denne øvelse vil du lære at opdatere en allerede deployet applikation i Kubernetes ved hjælp af `kubectl set image` og hvordan man ruller en opdatering tilbage med `kubectl rollout undo`.

### Trin 1: Opdatering af applikationens version

1. **Liste over deployments:**
   ```sh
   kubectl get deployments
   ```

2. **Opdater image til version 2:**  
I Kubernetes kan du opdatere en deployment til at bruge et andet container-image uden nedetid. Dette gøres ved at angive det nye image med `kubectl set image`. Her opdaterer vi til version 2 af applikationen, hvilket vil medføre, at nye pods spinnes op med det nye image, mens de gamle fjernes automatisk.
```sh
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/k8s-minikube/kubernetes-bootcamp:v2
```

3. **Bekræft opdateringen:**
   ```sh
   kubectl get pods
   ```

### Trin 2: Verificering af opdateringen

1. **Find den eksponerede IP og port:**
   ```sh
   kubectl describe services/kubernetes-bootcamp
   ```

2. **Sæt en miljøvariabel for NODE_PORT:**
   ```sh
   export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
   echo NODE_PORT=$NODE_PORT
   ```

3. **Test den opdaterede applikation:**
   ```sh
   curl $(minikube ip):$NODE_PORT
   ```

### Trin 3: Tilbageførsel af en opdatering

1. **Opdater til en ikke-eksisterende version (v10):**
   ```sh
   kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/k8s-minikube/kubernetes-bootcamp:v10
   ```

2. **Bekræft problemet:**
   ```sh
   kubectl get pods
   ```

3. **Rul tilbage til forrige version:**
   ```sh
   kubectl rollout undo deployments/kubernetes-bootcamp
   ```

4. **Bekræft tilbageførslen:**
   ```sh
   kubectl get pods
   ```

[Se original tutorial (Module 6)](https://minikube.sigs.k8s.io/docs/tutorials/kubernetes_101/module6/)

