# 🧭 Øvelsestitel: Arbejde med ReplicaSets og Horizontal Pod Autoscaling (HPA) i Kubernetes

## 🎯 Mål:
- Forstå hvordan ReplicaSets fungerer og hvordan Kubernetes selvhelbreder.
- Lære at skalere Pods manuelt og automatisk med HPA.
- Reflektere over fordele og ulemper ved automatisk skalering.

---

## 📽️ Forberedelse (ca. 15 min.)

### 1. **Se videoen (10:00)**
**"How a Replica Set Works? | Kubernetes Tutorial 2023"**  
👉 [Link til video](https://www.youtube.com/watch?v=iC-WxZGhFqs)

> 💡 *Fokusér på:*
> - Hvad en ReplicaSet er
> - Hvordan Pods styres og selvhelbredes
> - Hvordan skalering fungerer

### 2. **Læsestof (5-10 min.)**
Læs følgende sider i dokumentationen:
- [ReplicaSets](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
- [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)

---

## 🔧 Praktisk Øvelse

> 👨‍🔬 Du skal arbejde i et fungerende Kubernetes-miljø med `kubectl` installeret og adgang til en klynge (f.eks. Minikube eller en Docker-baseret løsning).

---

### 🛠️ Del 1: Opret en ReplicaSet

1. **Opret filen `nginx-replicaset.yaml`** med følgende indhold:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

2. **Deploy ReplicaSet:**

```bash
kubectl apply -f nginx-replicaset.yaml
```

3. **Verificér de kørende Pods:**

```bash
kubectl get pods
```

---

### 📈 Del 2: Skalering af ReplicaSet

4. **Skalér op til 5 Pods:**

```bash
kubectl scale rs nginx-replicaset --replicas=5
```

5. **Tjek fordeling på noder:**

```bash
kubectl get pods -o wide
```

> 💬 *Diskutér kort: Hvordan fordeles Pods? Er de spredt ud? Hvorfor/hvorfor ikke?*

---

### ♻️ Del 3: Selvhelbredelse

6. **Slet én Pod manuelt:**

```bash
kubectl delete pod <pod-navn>
```

7. **Observer at ReplicaSet automatisk genskaber den manglende Pod:**

```bash
kubectl get pods
```

> 💬 *Diskutér: Hvad ville ske, hvis der blev slettet 3 Pods? Hvad hvis en node fejler?*

---

### 🤖 Del 4: Introduktion til Horizontal Pod Autoscaling

8. **Tilføj metrics-server til klyngen** (hvis ikke allerede installeret):

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

9. **Opret en Deployment i stedet for ReplicaSet (HPA virker kun på Deployments):**

```bash
kubectl create deployment nginx --image=nginx
```

10. **Eksponer Deployment som service:**

```bash
kubectl expose deployment nginx --port=80 --type=NodePort
```

11. **Tilføj CPU-request (kræves for HPA):**

```bash
kubectl patch deployment nginx --patch '{
  "spec": {
    "template": {
      "spec": {
        "containers": [{
          "name": "nginx",
          "resources": {
            "requests": {
              "cpu": "100m"
            }
          }
        }]
      }
    }
  }
}'
```

12. **Opret HPA for nginx:**

```bash
kubectl autoscale deployment nginx --cpu-percent=50 --min=1 --max=10
```

13. **Observer HPA:**

```bash
kubectl get hpa
```

---

### 🔥 Bonus: Simulér belastning

> Dette kræver en ekstra container eller værktøj til at generere load (f.eks. `kubectl run -i --tty load-generator --image=busybox /bin/sh` og kør `while true; do wget -q -O- http://<nginx-service>; done`)

---

## 💬 Refleksionsspørgsmål

1. Hvad er fordelene ved at bruge en ReplicaSet fremfor en enkelt Pod?
2. Hvilke situationer gør HPA særligt nyttig?
3. Hvilke udfordringer kan opstå ved automatisk skalering?

---

## ✅ Læringsmål opfyldt?

- [ ] Jeg ved hvordan en ReplicaSet fungerer og hvordan Kubernetes genskaber Pods.
- [ ] Jeg kan skalere Pods manuelt og automatisk.
- [ ] Jeg kan forklare forskellen mellem ReplicaSet og HPA.
- [ ] Jeg har prøvet at oprette og arbejde med både ReplicaSet og HPA.