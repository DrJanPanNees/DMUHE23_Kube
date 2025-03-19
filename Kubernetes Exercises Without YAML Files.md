# Kubernetes Exercises Without YAML Files

## **Exercise 1: Setting Up Minikube and Inspecting the Cluster**
**Goal:** Understand how to start Minikube, check the cluster status, and view available nodes.

### **Instructions**
1. Start Minikube:
   ```bash
   minikube start
   ```
2. Check the version of Kubernetes running:
   ```bash
   kubectl version --short
   ```
3. Get cluster information:
   ```bash
   kubectl cluster-info
   ```
4. List available nodes in the cluster:
   ```bash
   kubectl get nodes
   ```

### **Reflection Questions**
- What is Minikube, and why do we use it for local Kubernetes learning?
- What does `kubectl cluster-info` tell you?

---

## **Exercise 2: Creating and Managing Pods Without YAML**
**Goal:** Learn how to create and inspect pods using `kubectl run` instead of YAML.

### **Instructions**
1. Create a simple pod running an **Nginx** container:
   ```bash
   kubectl run my-nginx --image=nginx --port=80
   ```
2. Verify that the pod was created:
   ```bash
   kubectl get pods
   ```
3. Describe the pod to see its details:
   ```bash
   kubectl describe pod my-nginx
   ```
4. Fetch logs from the pod:
   ```bash
   kubectl logs my-nginx
   ```
5. Access the pod’s shell:
   ```bash
   kubectl exec -it my-nginx -- /bin/sh
   ```
6. Delete the pod:
   ```bash
   kubectl delete pod my-nginx
   ```

### **Reflection Questions**
- How does using `kubectl run` compare to applying a YAML file?
- What kind of information can you get from `kubectl describe pod`?

---

## **Exercise 3: Creating and Using Namespaces**
**Goal:** Learn how to organize workloads using namespaces.

### **Instructions**
1. Create three namespaces:
   ```bash
   kubectl create namespace database-namespace
   kubectl create namespace team1-namespace
   kubectl create namespace team2-namespace
   ```
2. Verify namespaces exist:
   ```bash
   kubectl get namespaces
   ```
3. Run an **Nginx** pod in `team1-namespace`:
   ```bash
   kubectl run nginx-team1 --image=nginx --namespace=team1-namespace
   ```
4. Run an **Nginx** pod in `team2-namespace`:
   ```bash
   kubectl run nginx-team2 --image=nginx --namespace=team2-namespace
   ```
5. Verify that the pods are running in their correct namespaces:
   ```bash
   kubectl get pods -n team1-namespace
   kubectl get pods -n team2-namespace
   ```
6. Delete the namespaces (and their contents) when finished:
   ```bash
   kubectl delete namespace database-namespace
   kubectl delete namespace team1-namespace
   kubectl delete namespace team2-namespace
   ```

### **Reflection Questions**
- What is the benefit of using namespaces in Kubernetes?
- How do namespaces help in managing multi-team workloads?

---

## **Exercise 4: Working with Secrets in Kubernetes**
**Goal:** Store and retrieve sensitive data using Kubernetes **Secrets**.

### **Instructions**
1. Create a secret that stores a MongoDB username and password:
   ```bash
   kubectl create secret generic mongodb-secret \
     --from-literal=mongo-user=admin \
     --from-literal=mongo-password=supersecurepassword
   ```
2. List all secrets:
   ```bash
   kubectl get secrets
   ```
3. Describe the secret to see its details:
   ```bash
   kubectl describe secret mongodb-secret
   ```
4. Delete the secret:
   ```bash
   kubectl delete secret mongodb-secret
   ```

### **Reflection Questions**
- Why should secrets be used instead of environment variables?
- What happens when you try to `kubectl get secrets`—can you see the actual values?

---

## **Exercise 5: ConfigMaps and Environment Variables**
**Goal:** Store non-sensitive configuration data and use it inside a Kubernetes pod.

### **Instructions**
1. Create a ConfigMap to store application settings:
   ```bash
   kubectl create configmap app-config \
     --from-literal=APP_ENV=production \
     --from-literal=DEBUG_MODE=false
   ```
2. Verify that the ConfigMap was created:
   ```bash
   kubectl get configmaps
   ```
3. Describe the ConfigMap:
   ```bash
   kubectl describe configmap app-config
   ```
4. Delete the ConfigMap:
   ```bash
   kubectl delete configmap app-config
   ```

### **Reflection Questions**
- How do ConfigMaps differ from Secrets?
- In what situations would you use ConfigMaps?

---

## **Exercise 6: Creating and Managing Deployments**
**Goal:** Learn how to use `kubectl` to create and scale a deployment.

### **Instructions**
1. Create a deployment with **3 replicas**:
   ```bash
   kubectl create deployment my-deployment --image=nginx --replicas=3
   ```
2. Verify the deployment:
   ```bash
   kubectl get deployments
   ```
3. List all the pods created by the deployment:
   ```bash
   kubectl get pods
   ```
4. Scale the deployment to 5 replicas:
   ```bash
   kubectl scale deployment my-deployment --replicas=5
   ```
5. Delete the deployment:
   ```bash
   kubectl delete deployment my-deployment
   ```

### **Reflection Questions**
- How does a deployment differ from a standalone pod?
- What happens when you scale a deployment?

---

## **Exercise 7: Exposing a Deployment via a Service**
**Goal:** Create a Kubernetes **Service** to expose an application.

### **Instructions**
1. Create a deployment:
   ```bash
   kubectl create deployment webapp --image=nginx
   ```
2. Expose the deployment as a service:
   ```bash
   kubectl expose deployment webapp --type=NodePort --port=80
   ```
3. Verify that the service exists:
   ```bash
   kubectl get services
   ```
4. Get the URL of the service (Minikube only):
   ```bash
   minikube service webapp --url
   ```
5. Delete the service:
   ```bash
   kubectl delete service webapp
   ```
6. Delete the deployment:
   ```bash
   kubectl delete deployment webapp
   ```

### **Reflection Questions**
- What is the purpose of a Kubernetes service?
- Why do we use `NodePort` in this case?
