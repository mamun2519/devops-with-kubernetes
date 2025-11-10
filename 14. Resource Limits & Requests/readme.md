**Lesson 14: Kubernetes Resource Limits & Requests** —

# 🎯 Goal of This Lesson

After this lesson, you will know 👇

- What are **Resource Requests & Limits**
- Why we need them
- How CPU and Memory are managed
- Practical example with YAML

## 🧠 Why Do We Need This?

Imagine your cluster has many Pods —
Some do little work, some do a lot.

If one Pod **uses too much CPU or RAM**,
other Pods may **not get enough resources** 😣
and the system can become slow or even crash!

➡️ That’s why Kubernetes has **resource control** —
using `requests` and `limits`.

## ⚙️ What is a Resource Request?

**Request** means — the **minimum resource a Pod needs to start**.

Example:

> “This Pod needs at least 200m CPU and 256Mi memory.”

Kubernetes scheduler will check if a Node has enough free resources.
If not, the Pod will not start.

🧩 YAML Example:

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "256Mi"
```

- `200m` = 0.2 CPU (20% of 1 CPU core)
- `256Mi` = 256 MB RAM

## ⚡ What is a Resource Limit?

**Limit** means — the **maximum resource a Pod can use**.

Example:

> “This Pod can use max 500m CPU and 512Mi memory.”

🧩 YAML Example:

```yaml
resources:
  limits:
    cpu: "500m"
    memory: "512Mi"
```

## 🔄 Full Pod Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-demo
spec:
  containers:
    - name: demo-container
      image: nginx
      resources:
        requests:
          cpu: "200m"
          memory: "256Mi"
        limits:
          cpu: "500m"
          memory: "512Mi"
```

## 🧩 Real-Life Example

Think of an office with 10 employees:

- Some work less (low request)
- Some work medium (medium request)
- Some work more (high request)

The boss (Kubernetes Scheduler) **reserves CPU and memory** for each employee so no one uses others’ resources 😅

## 🚨 What Happens if a Pod Crosses Its Limit?

- **CPU:** Kubernetes will **slow down the Pod** (throttle)
- **Memory:** Kubernetes will **kill the Pod** (`OOMKilled`)

---

## 📊 Check Resource Usage

You can use **Kubernetes Dashboard** or:

```bash
kubectl top pod
```

This shows **CPU and Memory usage** of Pods.

---

## ✅ Summary

| Concept     | Meaning                    | Effect                             |
| ----------- | -------------------------- | ---------------------------------- |
| **Request** | Minimum resource needed    | Scheduler chooses Node accordingly |
| **Limit**   | Maximum resource allowed   | Exceed → Pod slow or killed        |
| **CPU**     | Processor power (milliCPU) | 1000m = 1 CPU core                 |
| **Memory**  | RAM (Mi / MegaBytes)       | RAM usage control                  |

---

## 🧩 Assignment

1️⃣ Create a Pod named `limit-test` with:

- Image: `nginx`
- Request: `cpu: 100m`, `memory: 128Mi`
- Limit: `cpu: 200m`, `memory: 256Mi`

2️⃣ Then check:

```bash
kubectl describe pod limit-test
kubectl top pod
```

You will see how much resource your Pod is using.

---

Well done Juboraj! 💪
Next, we can learn **Lesson 17: Horizontal Pod Autoscaler (HPA)** —
It automatically increases/decreases Pod numbers based on resource usage.

👉 Do you want me to start **Lesson 17** now?
