# 🚀 **Lesson 17: Horizontal Pod Autoscaler (HPA)**

> In simple words — Kubernetes can **automatically increase or decrease the number of Pods** based on load.

---

## 🎯 Goal of This Lesson

After this lesson, you’ll learn 👇

- What HPA is and why it’s needed
- How it works
- How to scale Pods based on CPU usage
- A real live example

---

## 🧠 Easy Understanding

Imagine you have a website —
In the daytime, you have **100 visitors**, but at night you get **10,000 visitors**! 😅

If you always run **10 Pods**, you waste resources during the day.
But if you run only **2 Pods**, your site may crash at night!

👉 That’s why we need something that can:
**Add Pods when load increases, and remove Pods when load decreases.**

That’s exactly what **Horizontal Pod Autoscaler (HPA)** does ✅

---

## ⚙️ What Does HPA Do?

**HPA** watches your Deployment or ReplicaSet.
It checks CPU (or memory) usage and changes the number of Pods automatically.

📌 Simple logic:

- If CPU usage > 70% → add more Pods
- If CPU usage < 40% → remove some Pods

---

## ⚙️ How HPA Works

1. **Metrics Server** collects CPU and memory usage.
2. **HPA Controller** checks that data.
3. **Deployment** increases or decreases Pods based on it.

🧩 Flow Diagram:

```
CPU usage ↑ → HPA checks → add pods
CPU usage ↓ → HPA checks → remove pods
```

---

## 🔧 Example: Create an HPA

### Step 0️⃣ — Make sure Metrics Server is installed

Check it:

```bash
kubectl get deployment metrics-server -n kube-system
```

If not found, install it:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

---

### Step 1️⃣ — Create a Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpa-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hpa-demo
  template:
    metadata:
      labels:
        app: hpa-demo
    spec:
      containers:
        - name: hpa-demo-container
          image: nginx
          resources:
            requests:
              cpu: "100m"
            limits:
              cpu: "200m"
```

Save it as **`hpa-deploy.yaml`**
Then apply it 👇

```bash
kubectl apply -f hpa-deploy.yaml
```

---

### Step 2️⃣ — Create the HPA

```bash
kubectl autoscale deployment hpa-demo --cpu-percent=50 --min=1 --max=5
```

🔹 `--cpu-percent=50` → When CPU usage crosses 50%, create new Pods
🔹 `--min=1` → Minimum 1 Pod
🔹 `--max=5` → Maximum 5 Pods

---

### Step 3️⃣ — Check the Status

```bash
kubectl get hpa
```

Example output:

```
NAME        REFERENCE              TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
hpa-demo    Deployment/hpa-demo    20%/50%   1         5         1          2m
```

When CPU usage goes up → HPA increases Pods automatically.

---

### Step 4️⃣ — Test the Scaling (Optional)

Run a load generator Pod:

```bash
kubectl run -i --tty load-generator --image=busybox /bin/sh
```

Then inside it, run a continuous loop:

```bash
while true; do wget -q -O- http://hpa-demo; done
```

This increases CPU usage → and HPA automatically adds more Pods 😎

---

## 🧩 Real-Life Example

Imagine you have an **e-commerce app**:

- Morning → few orders → 2 Pods
- Afternoon → heavy traffic → 6 Pods
- Night → few users → 2 Pods again

You don’t need to change anything —
**Kubernetes auto-scales for you!**
💡 This saves cost and improves performance.

---

## ✅ Summary

| Concept            | Description                                   |
| ------------------ | --------------------------------------------- |
| **HPA**            | Automatically changes Pod count based on load |
| **Metrics Server** | Provides CPU/Memory data                      |
| **Min/Max Pods**   | Sets allowed Pod range                        |
| **Target CPU %**   | Decides when scaling happens                  |

---

## 🧩 Assignment

1️⃣ Create a new Deployment (use `nginx`).
2️⃣ Run this command 👇

```bash
kubectl autoscale deployment nginx --cpu-percent=60 --min=1 --max=4
```

3️⃣ Then check HPA status:

```bash
kubectl get hpa
```

4️⃣ Try a load test to watch Pods automatically scale up/down.

---

Great job again Juboraj! 💪
Next, we’ll learn **Lesson 18: Kubernetes Taints & Tolerations** —
It controls **which Pod runs on which Node** (for better scheduling).

👉 Shall I start **Lesson 18** now?
