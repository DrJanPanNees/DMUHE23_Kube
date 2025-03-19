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

### **Test That Something Works**
- Run an **Nginx** pod:
  ```bash
  kubectl run my-nginx --image=nginx --port=80
  ```
- Expose the pod as a service:
  ```bash
  kubectl expose pod my-nginx --type=NodePort --port=80 --target-port=80
  ```
- Open the service in Minikube:
  ```bash
  minikube service my-nginx --url
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
6. Expose the pod as a service:
   ```bash
   kubectl expose pod my-nginx --type=NodePort --port=80 --target-port=80
   ```
7. Open the service in Minikube:
   ```bash
   minikube service my-nginx --url
   ```
8. Delete the pod:
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
   kubectl run nginx-team1 --image=nginx --namespace=team1-namespace --port=80
   ```
4. Expose the pod as a service:
   ```bash
   kubectl expose pod nginx-team1 --namespace=team1-namespace --type=NodePort --port=80 --target-port=80
   ```
5. Open the service in Minikube:
   ```bash
   minikube service nginx-team1 --namespace=team1-namespace --url
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
4. Expose the secret to an **Nginx** pod and test:
   ```bash
   kubectl run my-nginx --image=nginx --port=80
   kubectl expose pod my-nginx --type=NodePort --port=80 --target-port=80
   minikube service my-nginx --url
   ```
5. Delete the secret:
   ```bash
   kubectl delete secret mongodb-secret
   ```

### **Reflection Questions**
- Why should secrets be used instead of environment variables?
- What happens when you try to `kubectl get secrets`—can you see the actual values?

---

This update ensures that every exercise includes **running a pod, exposing it, and verifying it works** by accessing it in a browser. 🚀 Let me know if you need any further refinements!