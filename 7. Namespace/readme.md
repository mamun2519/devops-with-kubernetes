# 🧩 Lesson 10: Namespace – Resource Isolation in Kubernetes

## 🎯 Learning Goals

By the end of this lesson, you’ll know:

- What is a Namespace
- Why Namespace is needed
- How to create and use Namespace
- How Namespace helps to isolate resources (with real-life example)

## 🧠 Problem: What happens if everything is in one place?

Imagine your Kubernetes cluster has **3 different teams** working on it:

| Team   | Work Area                        |
| ------ | -------------------------------- |
| Team A | Backend (API)                    |
| Team B | Frontend                         |
| Team C | Monitoring (Prometheus, Grafana) |

Now, if **all Pods, Services, and Deployments** of all teams stay in one place 😩  
then you’ll face many problems:

- Resource conflicts
- One team might delete another team’s pod by mistake
- Security and management become a nightmare

👉 To solve this — Kubernetes gives us **Namespace** 🔒

## 💡 Simple Definition

> A **Namespace** is like a **virtual cluster** inside Kubernetes.  
> It helps you group and isolate resources logically.

Each Namespace works like a separate space for different teams or environments.

## 🏗️ Default Namespaces in Kubernetes

Kubernetes already has some default namespaces:

| Namespace           | Description                                                     |
| ------------------- | --------------------------------------------------------------- |
| **default**         | Used when you don’t specify any namespace                       |
| **kube-system**     | Used by Kubernetes system components (like kube-dns, scheduler) |
| **kube-public**     | For publicly readable information (rarely used)                 |
| **kube-node-lease** | For node heartbeat and lease management                         |

## 🧩 Part 1: Create a Custom Namespace

### 🔹 Using YAML

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-team
```

Apply it:

```bash
kubectl apply -f namespace.yaml
```

Or create directly:

```bash
kubectl create namespace dev-team
```

View all namespaces:

```bash
kubectl get namespace
```

**Output Example:**

```
NAME              STATUS   AGE
default           Active   2d
kube-system       Active   2d
dev-team          Active   10s
```

## 🧩 Part 2: Create Deployment inside a Namespace

Let’s deploy Nginx inside the **dev-team** namespace.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev-team
spec:
  replicas: 2
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
```

Apply it:

```bash
kubectl apply -f deployment.yaml
```

Now check pods:

```bash
kubectl get pods -n dev-team
```

👉 You’ll see pods are created only inside that specific namespace.

## 🧩 Part 3: Check Resources by Namespace

| Task                         | Command                                                     |
| ---------------------------- | ----------------------------------------------------------- |
| View all namespaces          | `kubectl get namespace`                                     |
| View pods in a namespace     | `kubectl get pods -n dev-team`                              |
| View services in a namespace | `kubectl get svc -n dev-team`                               |
| Set default namespace        | `kubectl config set-context --current --namespace=dev-team` |

## 💬 Real-Life Example

Imagine your company has **three environments**:

| Environment | Namespace | Purpose                 |
| ----------- | --------- | ----------------------- |
| Development | dev       | Developers testing code |
| Staging     | staging   | QA testing              |
| Production  | prod      | Live users              |

Using separate namespaces means:

- Teams don’t conflict with each other ✅
- Resource usage and access control are easier to manage 🔐
- Monitoring and debugging become simpler ⚙️

## ⚙️ Bonus Concept: Resource Quota & LimitRange

Another big advantage of namespaces —
you can **set resource limits (CPU, Memory, Pod count)** per namespace.

Example:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev-team
spec:
  hard:
    pods: "5"
    requests.cpu: "1"
    requests.memory: "1Gi"
    limits.cpu: "2"
    limits.memory: "2Gi"
```

This means:

- Max 5 Pods allowed in `dev-team` namespace
- Total CPU ≤ 2 cores
- Total Memory ≤ 2Gi

This keeps resources controlled and fair between teams ⚙️
