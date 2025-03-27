## Kubernetes Øvelse: CSSBattle Carrom

### 🌟 Formål

Deploy en simpel webapp inspireret af CSSBattle #2 ved brug af Minikube og Kubernetes. 
Denne øvelse giver praktisk erfaring med pods, deployments og services.

---

### 🔧 Forudsætninger

- Minikube er installeret
- Docker er installeret
- Kubectl virker sammen med Minikube

---

### 🔧 Trin-for-trin guide

#### 1. Start Minikube
```bash
minikube start
```

---

#### 2. Opret projektmappe
```bash
mkdir carrom-app
cd carrom-app
```

---

#### 3. Opret `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Carrom Challenge</title>
  <style>
    body {
      background-color: #62374e;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .square {
      width: 50px;
      height: 50px;
      background-color: #fdc57b;
      position: absolute;
    }
    .top-left { top: 50px; left: 50px; }
    .top-right { top: 50px; right: 50px; }
    .bottom-left { bottom: 50px; left: 50px; }
    .bottom-right { bottom: 50px; right: 50px; }
  </style>
</head>
<body>
  <div class="square top-left"></div>
  <div class="square top-right"></div>
  <div class="square bottom-left"></div>
  <div class="square bottom-right"></div>
</body>
</html>
```

---

#### 4. Opret Dockerfile
```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

---

#### 5. Byg Docker image

> 💡 **Hvad gør `eval $(minikube docker-env)`?**
>
> Denne kommando sætter din terminal op til at bruge Minikube’s interne Docker-engine. På den måde bliver dine Docker-images direkte tilgængelige for Kubernetes i Minikube. Uden denne kommando kan Minikube ikke finde de billeder du bygger lokalt.
>
> Nulstil med:
> ```bash
> eval $(minikube docker-env -u)
> ```

```bash
eval $(minikube docker-env)
docker build -t carrom-app .
```

---

#### 6. Opret og deploy med Kubernetes
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: carrom-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: carrom
  template:
    metadata:
      labels:
        app: carrom
    spec:
      containers:
      - name: carrom
        image: carrom-app
        ports:
        - containerPort: 80
```

```bash
kubectl apply -f deployment.yaml
```

---

#### 7. Eksponer applikationen
```bash
kubectl expose deployment carrom-app --type=NodePort --port=80
minikube service carrom-app --url
```
> Åbn URL'en i din browser.

> 💡 **Bemærk:** Dette er ikke helt det samme som Docker's `-p 8001:80`. Kubernetes vælger som standard en tilfældig port mellem 30000–32767. 
> Hvis du vil angive præcis hvilken port Kubernetes skal bruge på din node (som i Docker), skal du skrive en Service manuelt og tilføje `nodePort`, fx:
> ```yaml
> ports:
>   - port: 80
>     targetPort: 80
>     nodePort: 30001
> ```
> Derefter kan du tilgå din app via `http://<minikube-ip>:30001`

---

### 🧽 Ryd op
```bash
kubectl delete service carrom-app
kubectl delete deployment carrom-app
minikube stop
```

---

### 🚀 Klar til GitHub

Du kan gemme denne fil som `README.md` i dit projekt og uploade den til GitHub som dokumentation.

---

### 🏆 Bonus: CSSBattle Race Challenge

Hvis du underviser et hold, kan du tilføje konkurrence:

1. **Opdel i hold eller par.**
2. Giv dem en *ufuldstændig* version af `index.html` (fx mangler farver, placeringer eller nogle `div`s).
3. Første hold, der får layoutet til at ligne CSSBattle #2 mest muligt – vinder.

> Brug evt. skærmdeling til at sammenligne resultater.
> Overvej at give point for hurtighed, præcision og kreativitet.

**Eksempel på halvfærdig `index.html` til konkurrencen:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSSBattle Bonus</title>
  <style>
    body {
      background-color: white; /* skal ændres */
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .square {
      width: 50px;
      height: 50px;
      /* mangler farve */
      position: absolute;
    }
    .top-left {
      top: 50px;
      left: 50px;
    }
    /* mangler top-right, bottom-left, bottom-right */
  </style>
</head>
<body>
  <div class="square top-left"></div>
  <!-- mangler tre kvadrater -->
</body>
</html>
```

Denne battle tilføjer ekstra motivation og engagement til øvelsen!

