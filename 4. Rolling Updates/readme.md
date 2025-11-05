## 😎 Rolling Update in Kubernetes

### 🧠 Part 1: The Problem — What happens when you update the old way?

Imagine you run a **Burger Shop** 🍔
You have **three cooks** (meaning 3 Pods).
One day, you want to try a **new burger recipe** (meaning a new app version to deploy).

If you **fire all your old cooks at once** and **hire new ones**,
your shop will be **closed for a while** ❌ —
👉 This is called **downtime**.

In a **production server**, downtime is a big problem!

### 💡 Solution — Rolling Update

**Kubernetes Deployment** handles this smartly.

A **Rolling Update** means:
Old pods don’t die all at once —
they are replaced **one by one** with new pods
so that your service **never stops** ✅

### 🔹 Visualization

| Time | State                                           |
| ---- | ----------------------------------------------- |
| t0   | 3 old pods are running                          |
| t1   | 1 old pod terminated, 1 new pod started         |
| t2   | 2nd old pod terminated, another new pod started |
| t3   | All new pods ready, old pods completely stopped |

🟢 **Result:** Zero downtime update 🚀

## 🧩 Part 2: Rolling Update — Practical Example

Let’s say your **deployment.yaml** file looks like this:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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
          image: nginx:1.25
          ports:
            - containerPort: 80
```

### 1️⃣ Apply the deployment

```bash
kubectl apply -f deployment.yaml
```

### 2️⃣ Check running pods

```bash
kubectl get pods
```

### 3️⃣ Update to a new version (e.g., nginx:1.26)

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.26
```

### 4️⃣ Check rollout status

```bash
kubectl rollout status deployment/nginx-deployment
```

You’ll notice — pods are being **recreated one by one**.
Old pods **don’t die** until the new ones are **Ready** ✅
