# 🚀 Units-Task

A simple and resilient mini-project showcasing a Node.js backend integrated with a highly available MongoDB Replica Set in Kubernetes. Built with auto-scaling and continuous availability in mind!

## 🏗 Architecture 

![Architecture Diagram](Assets/Architecture%20Digaram.png)

## 🛠️ Setup Steps

1. **Deploy all resources** to your Kubernetes cluster (this includes the backend deployment, MongoDB stateful set, auto-scaler, and networking rules):
   ```bash
   kubectl apply -f k8s/
   ```
2. **Wait for the MongoDB replica set** to initialize (handled by the init-job) and the backend pods to become ready.
   ```bash
   kubectl get pods -w
   ```

## 🧪 Testing & Validation Commands

Validate the system's High Availability (HA) and Horizontal Pod Autoscaling (HPA) using the following scenarios.

### 1. Check Baseline Status
Ensure your persistent volumes and autoscaler are correctly provisioned:
```bash
kubectl get pvc
kubectl get hpa
```

### 2. High Availability (HA) Test
Verify that the database layer can survive a node failure gracefully.
- Delete the secondary MongoDB pod:
  ```bash
  kubectl delete pod mongo-1
  ```
- Send a request to your application's endpoint:
  **`GET /posts` still returns all records** seamlessly without any downtime!

### 3. Load Testing & Auto-scaling
Trigger a load test using [`hey`](https://github.com/rakyll/hey) to observe the HPA scaling up the backend pods.
- Run a 60-second test with 20 concurrent connections:
  ```bash
  hey -z 60s -c 20 <URL>
  ```
- In a separate terminal window, watch the HPA dynamically scale the replica count:
  ```bash
  kubectl get hpa -w
  ```