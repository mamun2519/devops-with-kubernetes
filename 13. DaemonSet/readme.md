# ⚙️ **Lesson 13: DaemonSet — Run One Pod per Node**

## 🎯 Goal of This Lesson

After this lesson, you will understand 👇

- What is a **DaemonSet** and why we use it
- Difference between **DaemonSet**, **Deployment**, and **StatefulSet**
- How it runs **one Pod on every Node**
- Real-life examples like Prometheus Node Exporter, Fluentd, etc.
- YAML example with explanation

---

## 🧠 1️⃣ The Problem — Some Pods must run on every Node!

Let’s say your cluster has 3 Nodes:

```
node1
node2
node3
```

You want one **log collector** or **monitoring agent** to run on each Node.
If you use a Deployment, Kubernetes will place Pods randomly.
You can’t be sure that **each Node has one Pod**.

👉 That’s why we use **DaemonSet**!

---

## 💡 2️⃣ What is a DaemonSet?

🧩 **DaemonSet** makes sure that **every Node runs one copy** of a specific Pod.

✅ When a new Node joins the cluster — DaemonSet automatically adds a Pod there.
🗑️ When a Node is removed — that Pod is also removed.

---

## 🔹 3️⃣ Example — Nginx DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemon
spec:
  selector:
    matchLabels:
      app: nginx-daemon
  template:
    metadata:
      labels:
        app: nginx-daemon
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

---

### 🧩 Explanation

| Part              | Meaning                                      |
| ----------------- | -------------------------------------------- |
| `kind: DaemonSet` | This is not a Deployment or StatefulSet      |
| `selector`        | Tells which Pods belong to this DaemonSet    |
| `template`        | Defines how each Pod will look               |
| Pod count         | One Pod runs on **every Node** automatically |

---

## 🔧 4️⃣ Check DaemonSet in Action

First, check your Nodes:

```bash
kubectl get nodes
```

Now apply the DaemonSet:

```bash
kubectl apply -f nginx-daemon.yaml
```

Then see Pods with Node info:

```bash
kubectl get pods -o wide
```

Output will look like 👇

```
NAME                 NODE
nginx-daemon-abc1    node1
nginx-daemon-xyz2    node2
nginx-daemon-qwe3    node3
```

👉 See?
One Pod is running on each Node ✅

---

## 🧰 5️⃣ Real-Life Examples of DaemonSet

DaemonSet is used for system-level tools that need to run on all Nodes.

| Tool                            | Purpose                           |
| ------------------------------- | --------------------------------- |
| **Fluentd / Filebeat**          | Collect logs from each Node       |
| **Prometheus Node Exporter**    | Collect system metrics            |
| **Kube-proxy**                  | Handles networking on each Node   |
| **CNI plugin (Calico / Weave)** | Sets up networking                |
| **Security agents**             | Monitor Nodes for security issues |

---

## 📦 6️⃣ DaemonSet vs Deployment

| Feature             | Deployment               | DaemonSet                 |
| ------------------- | ------------------------ | ------------------------- |
| Pod count           | Fixed (e.g., 3 replicas) | One Pod per Node          |
| Use case            | App workloads            | System / Monitoring tasks |
| When new Node added | No new Pod               | New Pod auto added        |
| Example             | Nginx web app            | Node Exporter, Fluentd    |

---

## 🧩 Assignment (Your Task)

1. Create a DaemonSet using this YAML 👇

   ```yaml
   apiVersion: apps/v1
   kind: DaemonSet
   metadata:
     name: node-monitor
   spec:
     selector:
       matchLabels:
         app: node-monitor
     template:
       metadata:
         labels:
           app: node-monitor
       spec:
         containers:
           - name: monitor
             image: busybox
             command:
               [
                 "sh",
                 "-c",
                 "while true; do echo Node $(hostname) running; sleep 10; done",
               ]
   ```

2. Check all Pods:

   ```
   kubectl get pods -o wide
   ```

3. Make sure you see **one Pod per Node** ✅

4. If you add a new Node — a new Pod will start automatically there!

---

## 💬 Real-Life Analogy (Easy Example)

Think about a **factory** 🏭
You want **one security guard (Pod)** for each **machine (Node)**.
If a new machine is added, a new guard goes there automatically.
That system is like a **DaemonSet** 👮‍♂️

---

## ✅ Summary

Today you learned:

- What a DaemonSet is
- When and why to use it
- YAML structure
- Real-world usage (monitoring, logging, etc.)

---

🔥 Next Lesson:
**Lesson 15 — Job & CronJob: One-Time and Scheduled Tasks in Kubernetes**

Do you want me to start **Lesson 15** now?
