# 🚀 Lesson 5: Pod in Kubernetes (Simple Explanation + Real-life Example)

## 🧠 1️⃣ What is a Pod?

A **Pod** is the smallest **deployable unit** in Kubernetes.  
That means — whenever you run an app in Kubernetes, it runs **inside a Pod**.  
A Pod can contain **one or more containers**.

---

## 📦 Real-life Example (Restaurant Analogy 🍽️)

Imagine you ordered a **lunch box** 🍱

Inside it, you have rice, lentils, curry, and salad —  
everything is served together.

Similarly, a **Pod** in Kubernetes may contain:

- One or more containers (e.g., backend + logger)
- They can communicate locally with each other
- All live inside the same “lunch box” (Pod)

---

## ⚙️ 2️⃣ Why Do We Need Pods?

In Docker, you run containers **individually**.  
But in Kubernetes, containers are **grouped inside Pods** — so that Kubernetes can easily manage them.

👉 Kubernetes **tracks Pods**, not containers.

So, Kubernetes says:

> “I need 3 Nginx Pods.”  
> Each Pod will run one Nginx container inside.

---

## 🧩 3️⃣ Pod Structure (Internally)

A Pod contains:

- One or more Containers
- Shared Network (same IP, port space)
- Shared Storage (volume)
- Metadata (labels, name, namespace)

# ⚙️ Kubernetes Pod Commands — Complete Guide

---

## 🧩 1️⃣ Pod Creation Commands

### ✅ Create a Pod (direct command)

```bash
kubectl run my-nginx --image=nginx --port=80
```

### 🧾 Create a Pod from YAML

```bash
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx
spec:
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 80

```

### run the yml file => kubectl apply -f pod.yaml
