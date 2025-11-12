# 🚀 **Lesson 20: Helm Basics — Package Management in Kubernetes**

## 🎯 **What You’ll Learn**

- What is Helm and why it’s useful
- Helm chart structure
- How to install Helm
- How to create your own chart
- Real-life example

## 🧠 **What is Helm (Simple Explanation)**

Helm is a **package manager for Kubernetes**.

👉 Like we use **apt** or **yum** in Linux to install software,
👉 We use **Helm** to install apps in Kubernetes easily.

## 💬 **Example to Understand**

Suppose your app needs these files to run:

- Deployment YAML
- Service YAML
- Ingress YAML
- ConfigMap YAML

👉 Applying all these files manually is boring and time-consuming.

With Helm, you can pack all YAMLs into **one package (called Chart)**
and install everything with just **one command** 👇

```bash
helm install myapp ./mychart
```

Helm will automatically create and deploy all the YAML files ✅

## 🧩 **3 Main Concepts of Helm**

| Concept        | Meaning                                             |
| -------------- | --------------------------------------------------- |
| **Chart**      | A package of your app (contains YAML templates)     |
| **Release**    | A running instance of a chart                       |
| **Repository** | A place where charts are stored (like an app store) |

---

## ⚙️ **Install Helm (CLI)**

👉 Command for Linux:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

👉 Check version:

```bash
helm version
```

---

## 🏗️ **Helm Chart Structure**

When you create a new chart 👇

```bash
helm create mychart
```

It will create this folder structure:

```
mychart/
│
├── Chart.yaml          # Chart info (name, version)
├── values.yaml         # Default configurable values
├── templates/          # All Kubernetes YAML files
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── charts/             # Dependencies (other charts)
```

---

## 📦 **Useful Helm Commands**

### 1️⃣ Install a chart:

```bash
helm install myapp ./mychart
```

### 2️⃣ List all releases:

```bash
helm list
```

### 3️⃣ Upgrade your app:

```bash
helm upgrade myapp ./mychart
```

### 4️⃣ Uninstall your app:

```bash
helm uninstall myapp
```

---

## 🧩 **Example: Deploy Nginx using Helm**

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx
```

✅ Helm will automatically download and deploy everything —
Deployment, Service, ConfigMap — all done together!

---

## 🧠 **Real-Life Example**

Imagine you order food on **Foodpanda**.
You can order each item separately, but it takes time.
Helm is like a **combo meal** 🍱 —
it includes App + Config + Service + Ingress all together!

---

## 🧩 **Assignment**

1️⃣ Install Helm in your Kubernetes cluster
2️⃣ Deploy **Nginx** using Helm
3️⃣ Check releases using `helm list`
4️⃣ Try to upgrade the app by changing image version
5️⃣ Uninstall it using `helm uninstall`

---

## 🏁 **Summary**

| Concept         | Meaning                              |
| --------------- | ------------------------------------ |
| **Helm**        | Kubernetes package manager           |
| **Chart**       | App’s package (YAML templates)       |
| **Release**     | Deployed version of the chart        |
| **Repository**  | Chart storage                        |
| **values.yaml** | File for configurable settings       |
| **Commands**    | install / list / upgrade / uninstall |

---

✅ Now you can **deploy production-grade apps with just one command**!
This is the **industry standard** way of managing Kubernetes apps 🎯

---

Next:
👉 **Lesson 23: Kubernetes Monitoring — Prometheus & Grafana (Real-Time Cluster Observation)**

Ready to start, Juboraj? 🔥
