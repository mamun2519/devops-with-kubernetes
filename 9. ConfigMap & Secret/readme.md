# 🧩 Lesson 9: ConfigMap & Secret — External Configuration in Kubernetes

## 🎯 Goal of This Lesson

After this lesson, you will understand:

- What is a ConfigMap and why we need it
- What is a Secret and how it’s different from ConfigMap
- How to define ConfigMap and Secret using YAML
- How to use them inside a Pod
- Which one is safer and when to use which

## 🧠 1️⃣ What is a ConfigMap?

👉 Imagine you are running an Nginx server.
If you hardcode file paths or environment variables directly inside your app, it becomes hard to change later.

🪄 **ConfigMap** helps you keep configuration **outside your code**.

You keep your code clean, and store all configurations (like environment variables, URLs, app names, etc.) in a ConfigMap.

### 🔹 Example: ConfigMap

Suppose your app needs these environment variables:

```
APP_NAME=MyApp
APP_ENV=production
```

You can store them in a ConfigMap like this 👇

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_NAME: "MyApp"
  APP_ENV: "production"
```

Then you can use it inside a Pod 👇

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
spec:
  containers:
    - name: demo
      image: nginx
      envFrom:
        - configMapRef:
            name: app-config
```

👉 This means: all key-value pairs from ConfigMap will become environment variables inside this container.

## 🔐 2️⃣ What is a Secret?

A **Secret** is almost like a ConfigMap — but used for **sensitive data** such as passwords, API keys, or tokens.

The main difference:

- ConfigMap stores data as **plain text** (anyone can read).
- Secret stores data in **Base64 encoded** form (a bit safer).

### 🔹 Example: Secret

Let’s say your database password is `mypassword` and user is `myuser`.

Here’s how to store them in a Secret 👇

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_USER: bXl1c2Vy
  DB_PASS: bXlwYXNzd29yZA==
```

👉 Note:
`bXl1c2Vy` = “myuser”
`bXlwYXNzd29yZA==` = “mypassword” (both are Base64 encoded)

Use it inside a Pod like this 👇

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
spec:
  containers:
    - name: app
      image: nginx
      envFrom:
        - secretRef:
            name: db-secret
```

---

## 💡 3️⃣ ConfigMap vs Secret

| Feature   | ConfigMap            | Secret               |
| --------- | -------------------- | -------------------- |
| Data Type | Non-sensitive config | Sensitive data       |
| Storage   | Plain text           | Base64 encoded       |
| Example   | APP_NAME, ENV        | Passwords, Tokens    |
| Best For  | Normal app settings  | Security credentials |

---

## 🧰 4️⃣ Real-Life Example

Suppose your Node.js app connects to MySQL and needs 3 configs:

- DB_HOST
- DB_USER
- DB_PASS

You can:

- Store `DB_HOST` in a **ConfigMap**
- Store `DB_USER` and `DB_PASS` in a **Secret**

✅ Result: Safe + Flexible configuration management

---

## 🧩 Assignment (Today’s Task)

1️⃣ Create a **ConfigMap** named `web-config`
with these values:

```yaml
APP_NAME: "KubeWeb"
APP_VERSION: "1.0"
```

2️⃣ Create a **Secret** named `db-secret`
with these values:

```yaml
DB_USER: "root"
DB_PASS: "12345"
```

3️⃣ Create a **Pod** named `nginx-config-pod`
that uses both the ConfigMap and Secret.

4️⃣ After the Pod runs, check the environment variables:

```bash
kubectl exec -it nginx-config-pod -- printenv | grep APP
```

👉 You should see `APP_NAME` and `APP_VERSION` in the output.

---

## 🔮 Next Lesson Preview

👉 **Lesson 12: Volumes & Persistent Storage in Kubernetes** 💾
(How to keep your data safe even when Pods are deleted)

---

Would you like me to start **Lesson 12** now, or do you want to finish this assignment first?
