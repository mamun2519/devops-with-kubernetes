# 🧱 **Lesson 11: Kubernetes StatefulSet — Managing Stateful Apps (like Databases)**

## 🎯 Goal of This Lesson

After this lesson, you will understand 👇

- What is a StatefulSet and why we use it
- Difference between **Stateful** and **Stateless** apps
- How StatefulSet gives Pods fixed names and storage
- What is a **Headless Service** and why it is needed
- Example: MySQL StatefulSet with YAML

## 🧠 1️⃣ Stateless vs Stateful — Easy Example

👉 Example 1: You run 3 Pods for a web server (like **nginx**).
Each Pod is the same, no data sharing is needed.
If you delete one Pod, nothing bad happens.

➡️ This is called a **Stateless Application**
Examples: Nginx, Frontend apps, API servers.

👉 Example 2: You run a **MySQL database**.
If you delete the Pod and restart it, all data is gone 😭
Because the database needs the **same name and same storage**.

➡️ This is a **Stateful Application**.

## 💡 2️⃣ What is a StatefulSet?

🧩 **StatefulSet** is like a Deployment,
but it gives each Pod a **fixed name and fixed storage**.

That means:

- Each Pod gets a fixed name (like `mysql-0`, `mysql-1`)
- Each Pod has its own storage (PVC)
- Even if Pod is deleted, data stays safe

---

## 🔹 Example — MySQL StatefulSet

Let’s create it step by step 👇

---

### Step 1️⃣: Create Headless Service

Headless Service = service with **no load balancer**,
used only to give Pods **DNS names**.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
    - port: 3306
      name: mysql
```

👉 `clusterIP: None` means it’s **Headless**.

Now each Pod will have DNS like:

```
mysql-0.mysql
mysql-1.mysql
```

---

### Step 2️⃣: Create StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 2
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: "rootpass"
          volumeMounts:
            - name: data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

---

### 📘 What’s Happening Here

| Item                 | Explanation                          |
| -------------------- | ------------------------------------ |
| `replicas: 2`        | Two MySQL Pods will be created       |
| Pod names            | `mysql-0`, `mysql-1`                 |
| Storage              | Each Pod has its own PVC             |
| Headless Service     | Gives DNS for each Pod               |
| VolumeClaimTemplates | Creates a separate disk for each Pod |

---

## 🔧 Step 3️⃣: Check StatefulSet

```bash
kubectl get statefulset
kubectl get pods
kubectl get pvc
```

Output:

```
NAME      READY   AGE
mysql     2/2     1m
```

PVCs:

```
NAME             STATUS
data-mysql-0     Bound
data-mysql-1     Bound
```

👉 Even if you delete Pods, data stays safe.
When Pods restart, they get the same data ✅

---

## 🔁 StatefulSet vs Deployment

| Feature            | Deployment         | StatefulSet                               |
| ------------------ | ------------------ | ----------------------------------------- |
| Pod name           | Random             | Fixed (pod-0, pod-1)                      |
| Storage            | Shared / Temporary | Separate per Pod                          |
| Data after restart | Lost               | Saved                                     |
| Use case           | Web apps, APIs     | Databases, Caches (MySQL, MongoDB, Redis) |

---

## 🧰 Real-Life Example

If you run **MongoDB ReplicaSet**, **Kafka**, or **RabbitMQ** clusters —
you must use **StatefulSet**, because each node needs its own data and name.

---

## 🧩 Assignment (Your Task)

1. Create a **Headless Service** named `mysql`

2. Use the above YAML to run the `mysql` StatefulSet

3. Check using:

   ```
   kubectl get pods -l app=mysql
   kubectl get pvc
   ```

4. Delete a Pod and test if data stays:

   ```
   kubectl delete pod mysql-0
   ```

Check data in `/var/lib/mysql`.

---

💬 Tip: StatefulSet creates Pods **one by one** (not all together).
`mysql-0` must be ready before `mysql-1` starts — for stability.

---

✅ You Learned:

- Difference between Stateful and Stateless
- How StatefulSet works
- Headless Service and PVC concept

---

🚀 Next Lesson:
**Lesson 14 — DaemonSet: Run One Pod per Node (like monitoring or logging agents)**

Do you want me to start **Lesson 14** now?
