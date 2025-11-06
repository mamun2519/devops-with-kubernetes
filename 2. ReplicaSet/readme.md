# 🚀 Lesson 4: What is a ReplicaSet in Kubernetes?

## 🍔 Real-life Analogy (Burger Shop Example)

Imagine you run a **Burger Shop** 🍔  
You have one cook who makes all the burgers.  
One day, that cook gets sick — the shop stops working 😞

What will you do?  
➡️ You’ll hire **multiple cooks** (say, 3 cooks).

Now, if one cook doesn’t show up — the other two keep the shop running ✅

This is exactly what **ReplicaSet** does in **Kubernetes**.

## 🧠 Definition

A **ReplicaSet** is a **Kubernetes controller**  
that ensures a specified number of **Pods** are **always running** in the cluster.

## 🔹 Example Scenario

Let’s say you tell Kubernetes:

> “I want 3 Nginx Pods.”

Then the ReplicaSet tells Kubernetes —  
“No matter what happens, keep **3 Pods** running at all times.”

🧠 If one Pod crashes → ReplicaSet **creates a new Pod**  
🧠 If there are extra Pods → ReplicaSet **deletes the extra ones**

---

## 🧾 YAML Example: `replicaset.yaml`

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
          image: nginx:latest
          ports:
            - containerPort: 80
```

## 🔍 Explanation

| Field           | Meaning                                                    |
| --------------- | ---------------------------------------------------------- |
| **replicas: 3** | Ensures 3 Pods are always running                          |
| **selector**    | Defines which Pods the ReplicaSet will manage              |
| **template**    | Defines the Pod structure (metadata + spec)                |
| **containers**  | The actual containers (e.g., nginx) to run inside each Pod |

## 🧪 Commands to Apply and Verify

### ✅ Create ReplicaSet

```bash
kubectl apply -f replicaset.yaml
```

### 🔍 Check running ReplicaSets

```bash
kubectl get rs
```

### 🔹 Check Pods created by the ReplicaSet

```bash
kubectl get pods -l app=nginx
```

### 📖 Describe details

```bash
kubectl describe rs nginx-replicaset
```

### 🧹 Delete ReplicaSet (and its Pods)

```bash
kubectl delete -f replicaset.yaml
```

## 🎯 Summary

| Concept               | Description                                           |
| --------------------- | ----------------------------------------------------- |
| **ReplicaSet**        | Ensures a fixed number of identical Pods              |
| **Main Job**          | Auto-create or delete Pods to match the desired count |
| **Real-life analogy** | Multiple cooks to keep the shop running               |
| **Key advantage**     | Self-healing and fault-tolerant apps                  |

👨‍💻 **Author:** Juboraj Islam Mamun
📚 **Repository:** `k8s-notes-and-practice`
🏷️ **Tags:** `#kubernetes #replicaset #devops #containers`
