# 1. Pod
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
---
# 2. Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-deploy
  template:
    metadata:
      labels:
        app: nginx-deploy
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
---
# 3. StatefulSet
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
spec:
  serviceName: "nginx"
  replicas: 2
  selector:
    matchLabels:
      app: nginx-ss
  template:
    metadata:
      labels:
        app: nginx-ss
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
---
# 4. DaemonSet
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
spec:
  selector:
    matchLabels:
      app: nginx-ds
  template:
    metadata:
      labels:
        app: nginx-ds
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
---
# 5. Job
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-job
spec:
  template:
    spec:
      containers:
      - name: nginx
        image: busybox
        command: ["sh", "-c", "echo Hello from Job && sleep 10"]
      restartPolicy: Never
  backoffLimit: 3
---
# 6. CronJob
apiVersion: batch/v1
kind: CronJob
metadata:
  name: nginx-cronjob
spec:
  schedule: "*/1 * * * *"  # Kører hvert minut
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: nginx
            image: busybox
            command: ["sh", "-c", "echo Hello from CronJob && date"]
          restartPolicy: OnFailure
