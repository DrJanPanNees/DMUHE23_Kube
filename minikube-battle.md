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

---

### 🪑 Ryd op
```bash
kubectl delete service carrom-app
kubectl delete deployment carrom-app
minikube stop
```

---


