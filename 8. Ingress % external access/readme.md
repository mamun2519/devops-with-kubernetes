# 🧩 Lesson 9: Kubernetes Ingress & External Access

---

## 🎯 Goal of This Lesson

After this lesson, you will understand:

- What is Ingress and why we need it
- The limitations of NodePort and LoadBalancer
- How Ingress helps route traffic using domain names
- A real example: multiple services under one domain

---

## 🧠 Problem: Limitations of NodePort / LoadBalancer

In the previous lesson, we learned:

- **NodePort** → You can access from outside but with fixed ports (like 30080)
- **LoadBalancer** → Gives a public IP, but **each service needs its own LoadBalancer!**

Example:  
You have 3 services:

1. frontend (React app)
2. backend (API server)
3. admin panel

Then you’ll need 3 LoadBalancers and 3 public IPs 😩  
Users would have to remember each IP like:

```

frontend → 35.11.12.1
backend → 35.11.12.2
admin → 35.11.12.3

```

That’s messy and expensive 💥

---

## 💡 Solution: Use Ingress

👉 **Ingress** is like a smart router 🚦 in front of your cluster.
It routes traffic to the right service based on **domain name** or **URL path**.

---

### Example Mapping

| Domain or Path    | Goes To          | Type         |
| ----------------- | ---------------- | ------------ |
| example.com       | frontend service | Ingress rule |
| example.com/api   | backend service  | Ingress rule |
| example.com/admin | admin service    | Ingress rule |

---

## 🧩 Part 1: What is an Ingress Controller?

Ingress needs a **Controller** to work —
it’s the software that actually handles routing.

Popular controllers:

- **NGINX Ingress Controller** (most common)
- Traefik
- HAProxy

If you are using **Minikube**, enable it with:

```bash
minikube addons enable ingress
```

---

## 🧩 Part 2: Example Setup

Let’s say you have two services:

1️⃣ frontend-service (port 3000)
2️⃣ backend-service (port 5000)

---

### frontend-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - port: 3000
      targetPort: 3000
  type: ClusterIP
```

### backend-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - port: 5000
      targetPort: 5000
  type: ClusterIP
```

---

## 🧩 Part 3: Ingress YAML Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: myapp.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend-service
                port:
                  number: 3000
          - path: /api
            pathType: Prefix
            backend:
              service:
                name: backend-service
                port:
                  number: 5000
```

---

## 🧠 Explanation

- When someone visits `http://myapp.local/` → goes to **frontend-service**
- When someone visits `http://myapp.local/api` → goes to **backend-service**

---

## 🧪 Local Test with Minikube

1️⃣ Enable Ingress:

```bash
minikube addons enable ingress
```

2️⃣ Apply all YAML files:

```bash
kubectl apply -f frontend-service.yaml
kubectl apply -f backend-service.yaml
kubectl apply -f ingress.yaml
```

3️⃣ Check ingress:

```bash
kubectl get ingress
```

You’ll see something like:

```
NAME          CLASS    HOSTS          ADDRESS        PORTS
my-ingress    <none>   myapp.local    192.168.49.2   80
```

4️⃣ Add this line to your `/etc/hosts` file (for local testing):

```
192.168.49.2  myapp.local
```

Now open your browser:

```
http://myapp.local/
http://myapp.local/api
```

Both will work perfectly 🎉

---

## 💬 Real Life Example

| Concept            | Real Example                         |
| ------------------ | ------------------------------------ |
| LoadBalancer       | Separate shop entrances              |
| Ingress Controller | The main gate of a shopping mall     |
| Ingress Rules      | Directions to different shops inside |

---

## ✅ Assignment #9

1️⃣ Enable Ingress in Minikube
2️⃣ Create two deployments: frontend and backend
3️⃣ Create Ingress that sends `/` → frontend and `/api` → backend
4️⃣ Test in browser

💡 Bonus: Add `/admin` path for another service

---

## 🔮 Next Lesson Preview

👉 **Lesson 10: ConfigMap & Secret in Kubernetes**
(Learn how to store and manage app configuration, passwords, and API keys safely)

---

```

---

Would you like me to generate the **folder path + file name suggestion** for this lesson (e.g. `lessons/lesson-09-ingress-external-access/README.md`)?
That way you can paste this content directly in your repo folder.
```
