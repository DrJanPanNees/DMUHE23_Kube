# Kubernetes Minikube Øvelser

## **Øvelse 1: Opsætning af Minikube og kubectl**
**Metadata:**
- **Læringsmål:** Introduktion til Minikube, grundlæggende kubectl-kommandoer, og verifikation af en fungerende Kubernetes-klynge.
- **Relevans:** Grundlæggende forståelse af Kubernetes-administration i et lokalt udviklingsmiljø.

### **Instruktioner:**
1. Start Minikube:
   ```bash
   minikube start
   ```
2. Verificér Kubernetes-klyngen:
   ```bash
   kubectl cluster-info
   kubectl get nodes
   ```
3. Vis versioner af klienten og serveren:
   ```bash
   kubectl version
   ```

### **Spørgsmål til refleksion:**
- Hvilke komponenter består en Kubernetes-klynge af?
- Hvad gør `kubectl get nodes`?

---

## **Øvelse 2: Oprettelse af en simpel pod**
**Metadata:**
- **Læringsmål:** Forstå hvordan en Pod fungerer, og hvordan man interagerer med den.
- **Relevans:** Pods er den mindste enhed i Kubernetes og danner grundlaget for alle deployment-strukturer.

### **Instruktioner:**
1. Opret en pod direkte fra kommandolinjen:
   ```bash
   kubectl run mypod --image=nginx
   ```
2. Tjek pod-status:
   ```bash
   kubectl get pods
   ```
3. Beskriv pod’en for at se detaljer:
   ```bash
   kubectl describe pod mypod
   ```
4. Hent logs fra pod’en:
   ```bash
   kubectl logs mypod
   ```
5. Slet pod’en:
   ```bash
   kubectl delete pod mypod
   ```

### **Spørgsmål til refleksion:**
- Hvad sker der, hvis pod’en crasher?
- Hvordan kan man inspicere fejl i en pod?

---

## **Øvelse 3: Deployment af MongoDB**
**Metadata:**
- **Læringsmål:** Lære at deploye en database i Kubernetes ved hjælp af YAML-konfiguration.
- **Relevans:** At forstå hvordan databasesystemer som MongoDB kan håndteres i Kubernetes.

### **Instruktioner:**
1. Opret en `mongodb-deployment.yaml` fil med følgende indhold:
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
2. Deploy MongoDB i Kubernetes:
   ```bash
   kubectl apply -f mongodb-deployment.yaml
   ```
3. Verificér deployment:
   ```bash
   kubectl get deployments
   kubectl get pods
   ```

### **Spørgsmål til refleksion:**
- Hvorfor bruger vi et Deployment fremfor en enkelt Pod?
- Hvordan kan vi sikre, at MongoDB fortsætter med at køre, selv hvis en Pod fejler?

---

## **Øvelse 4: Opsætning af Secrets til MongoDB**
**Metadata:**
- **Læringsmål:** Lære at gemme følsomme data i Kubernetes.
- **Relevans:** Beskyttelse af credentials og følsomme data er en essentiel del af moderne systemer.

### **Instruktioner:**
1. Opret en Secret med credentials:
   ```bash
   kubectl create secret generic mongodb-secret \
     --from-literal=mongo-root-username=mongouser \
     --from-literal=mongo-root-password=password
   ```
2. Bekræft at secret’en er oprettet:
   ```bash
   kubectl get secrets
   kubectl describe secret mongodb-secret
   ```

### **Spørgsmål til refleksion:**
- Hvad er forskellen mellem en Secret og en ConfigMap?
- Hvorfor gemmes værdierne i Base64-kodet format?

---

## **Øvelse 5: Oprettelse af en Service til MongoDB**
**Metadata:**
- **Læringsmål:** Forstå netværkskommunikation mellem Pods i Kubernetes.
- **Relevans:** Services bruges til at forbinde Pods og eksterne systemer.

### **Instruktioner:**
1. Opret en `mongodb-service.yaml` fil:
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
2. Deploy service:
   ```bash
   kubectl apply -f mongodb-service.yaml
   ```
3. Verificér at service kører:
   ```bash
   kubectl get services
   ```

### **Spørgsmål til refleksion:**
- Hvad er forskellen på en ClusterIP, NodePort og LoadBalancer service?
- Hvordan kan vi tilgå MongoDB fra en anden pod?

---

## **Øvelse 6: Deployment af Mongo Express UI**
**Metadata:**
- **Læringsmål:** Lære at forbinde en frontend-applikation til en database i Kubernetes.
- **Relevans:** Mongo Express bruges til at visualisere MongoDB-data.

### **Instruktioner:**
1. Opret en `mongo-express-deployment.yaml` fil:
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
           - name: ME_CONFIG_MONGODB_ADMINUSERNAME
             valueFrom:
               secretKeyRef:
                 name: mongodb-secret
                 key: mongo-root-username
           - name: ME_CONFIG_MONGODB_ADMINPASSWORD
             valueFrom:
               secretKeyRef:
                 name: mongodb-secret
                 key: mongo-root-password
           - name: ME_CONFIG_MONGODB_SERVER
             value: "mongodb-service"
   ```
2. Deploy Mongo Express:
   ```bash
   kubectl apply -f mongo-express-deployment.yaml
   ```
3. Eksponér Mongo Express med en NodePort service:
   ```bash
   kubectl expose deployment mongo-express --type=NodePort --port=8081
   ```
4. Find URL’en til Mongo Express:
   ```bash
   minikube service mongo-express --url
   ```

### **Spørgsmål til refleksion:**
- Hvorfor bruger vi en Secret til at autentificere Mongo Express?
- Hvordan kan vi sikre, at Mongo Express altid kører?