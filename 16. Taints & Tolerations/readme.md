# ⚙️ **Lesson 16: Taints & Tolerations — Control Which Pods Run on Which Nodes**

## 🎯 Goal of This Lesson

After this lesson, you’ll understand 👇

- What Taint and Toleration are
- Why they are needed
- How they work
- Real-life examples
- Practical YAML setup

## 🧠 Easy Understanding

Imagine your cluster has 3 nodes 👇

| Node  | Type     | Description                   |
| ----- | -------- | ----------------------------- |
| Node1 | Normal   | Any Pod can run here          |
| Node2 | GPU Node | Only AI/ML Pods should run    |
| Node3 | DB Node  | Only Database Pods should run |

Now if Kubernetes randomly sends any Pod to any Node —
your **Database Pod may go to the GPU Node**, which is wrong 😅

👉 That’s why we use **Taints and Tolerations** —
to tell Kubernetes **which Node should accept which Pod**.

## ⚙️ Definition

| Concept                    | Meaning                                          |
| -------------------------- | ------------------------------------------------ |
| **Taint (Node level)**     | Node says “I don’t want some Pods to run on me.” |
| **Toleration (Pod level)** | Pod says “I can tolerate that Taint.”            |

## 🧩 Simple Example

You add a taint to Node3 👇

```bash
kubectl taint nodes node3 key=value:NoSchedule
```

This means:

> Node3 will not take any Pod **unless that Pod tolerates this taint.**

## ⚙️ Create a Pod That Can Tolerate

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tolerant-pod
spec:
  tolerations:
    - key: "key"
      operator: "Equal"
      value: "value"
      effect: "NoSchedule"
  containers:
    - name: nginx
      image: nginx
```

✅ This Pod **can run on Node3** because it has the matching toleration.

## 🎯 Different Types of Effects

| Effect               | Description                                                |
| -------------------- | ---------------------------------------------------------- |
| **NoSchedule**       | Pod will **not** be scheduled on that Node.                |
| **PreferNoSchedule** | Kubernetes **tries to avoid** scheduling Pod there (soft). |
| **NoExecute**        | If Pod already exists there, Kubernetes **removes it**.    |

## 🔧 Practical Example: Protect Database Node

Let’s protect your **Database Node** so only DB Pods can run there.

**Step 1️⃣: Add Taint**

```bash
kubectl taint nodes db-node db=true:NoSchedule
```

**Step 2️⃣: Create a Pod That Can Tolerate**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mysql-db
spec:
  tolerations:
    - key: "db"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
  containers:
    - name: mysql
      image: mysql:5.7
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: "root123"
```

👉 Only this Pod can now run on the tainted DB Node.

---

## 🧩 Real-Life Example

Let’s say your company has 3 nodes:

- Node 1 → Handles Web traffic
- Node 2 → Runs Databases
- Node 3 → Handles Logging & Monitoring

You want:

- DB Pods → only on Node 2
- Logging Pods → only on Node 3
- Normal Pods → on Node 1

➡️ You can do this easily with **Taints and Tolerations** ✅

---

## 📊 Useful Commands

- See taints on all nodes:

  ```bash
  kubectl describe node <node-name>
  ```

- Remove a taint:

  ```bash
  kubectl taint nodes <node-name> key=value:NoSchedule-
  ```

---

## ✅ Summary

| Concept        | Level  | Meaning                                 |
| -------------- | ------ | --------------------------------------- |
| **Taint**      | Node   | Says “Don’t run specific Pods on me”    |
| **Toleration** | Pod    | Says “I can handle this Node’s Taint”   |
| **Effect**     | Action | NoSchedule, PreferNoSchedule, NoExecute |

---

## 🧩 Assignment

1️⃣ Choose a Node (for example, `node1`).
2️⃣ Add a taint:

```bash
kubectl taint nodes node1 special=true:NoSchedule
```

3️⃣ Create a Pod named `special-pod` with a matching toleration so it can run on that Node.
4️⃣ Check where the Pod is running:

```bash
kubectl get pods -o wide
```

---

Excellent work Juboraj! 💪
Next, we’ll learn **Lesson 19: Node Affinity & Pod Affinity/Anti-Affinity** —
where you’ll control **which Pods should or shouldn’t run together** or **on which Node**.

👉 Shall I start Lesson 19 now?
