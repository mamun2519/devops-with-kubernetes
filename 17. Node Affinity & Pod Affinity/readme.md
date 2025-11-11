# 🎯 **Lesson 17: Node Affinity & Pod Affinity / Anti-Affinity**

## 🧠 What You’ll Learn

- What is Node Affinity and Pod Affinity
- Why they are used
- How to control where Pods will run
- Real-life examples
- Practical YAML examples

## ⚙️ Why We Need This

Normally, Kubernetes automatically decides which **Pod** will run on which **Node**.

But sometimes you need more control — for example:

| Situation                                | What You Need     |
| ---------------------------------------- | ----------------- |
| Database Pod should only run on SSD Node | Node Affinity     |
| Web Pod should run near Database Pod     | Pod Affinity      |
| Two Pods should not run on same Node     | Pod Anti-Affinity |

## ⚙️ Node Affinity — Control Which Node Pod Will Run On

Node Affinity lets you tell Kubernetes:

> “My Pod should only run on specific Nodes.”

You do this using **labels** on Nodes and **rules** inside the Pod.

### 🧩 Step 1️⃣: Add Label to Node

Let’s say you want to label a Node that has SSD disk:

```bash
kubectl label nodes node1 disktype=ssd
```

### 🧩 Step 2️⃣: Add Node Affinity in Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-demo
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: disktype
                operator: In
                values:
                  - ssd
  containers:
    - name: nginx
      image: nginx
```

👉 This Pod will **only** run on nodes with the label `disktype=ssd`.

### ⚙️ Types of Node Affinity

| Type                                                | Meaning                              |
| --------------------------------------------------- | ------------------------------------ |
| **requiredDuringSchedulingIgnoredDuringExecution**  | Must match (hard rule)               |
| **preferredDuringSchedulingIgnoredDuringExecution** | Try to match if possible (soft rule) |

## 💡 Real-Life Example

Your cluster:

- Node A = SSD Node (database)
- Node B = HDD Node (normal)

You want your MySQL Pod to run **only on SSD Node** → Use **Node Affinity** ✅

## ⚙️ Pod Affinity & Pod Anti-Affinity

These are for **Pod-to-Pod relationships** — not Node-based.

### 🧩 Pod Affinity — Pods Want to Stay Together

You can make Pods run near other Pods for faster communication.

Example: Web Pod near Database Pod 👇

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
  labels:
    app: web
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: app
                operator: In
                values:
                  - db
          topologyKey: "kubernetes.io/hostname"
  containers:
    - name: nginx
      image: nginx
```

👉 This Web Pod will run **close to** any Pod labeled `app=db`.

### 🧩 Pod Anti-Affinity — Pods Stay Separate

If you want Pods to run on **different Nodes** (for high availability):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: app
                    operator: In
                    values:
                      - web
              topologyKey: "kubernetes.io/hostname"
      containers:
        - name: nginx
          image: nginx
```

👉 Kubernetes will schedule each Pod on **different Nodes**.

## 🔍 Summary Table

| Concept               | Use                            | Example                    |
| --------------------- | ------------------------------ | -------------------------- |
| **Node Affinity**     | Control which Node Pod runs on | DB Pod only on SSD Node    |
| **Pod Affinity**      | Keep Pods close together       | Web Pod near DB Pod        |
| **Pod Anti-Affinity** | Keep Pods apart                | Web Pods on separate Nodes |

---

## 🧩 Real-Life Recap

| Scenario                       | Solution          |
| ------------------------------ | ----------------- |
| Database Pod on SSD Node       | Node Affinity     |
| Web + DB together              | Pod Affinity      |
| Web replicas on separate Nodes | Pod Anti-Affinity |

---

## 🧠 Assignment

1️⃣ Label one Node as `role=db`

```bash
kubectl label nodes node1 role=db
```

2️⃣ Create a Pod that runs only on `role=db` Node using **Node Affinity**.
3️⃣ Create a Web Pod that stays near the DB Pod using **Pod Affinity**.
4️⃣ Create a Web Deployment with 3 replicas that must run on **different Nodes** using **Pod Anti-Affinity**.

---

## ✅ You Did It!

Awesome Juboraj! 🔥
Now you understand how Kubernetes **schedules and controls Pods smartly.**

Next, we’ll move to the **Security & Monitoring Phase** —
starting with 👇
**Lesson 20: Service Account & RBAC (Role-Based Access Control)**

👉 Shall we start Lesson 20?
