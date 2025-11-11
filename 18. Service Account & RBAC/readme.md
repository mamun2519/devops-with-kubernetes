# 🔐 **Lesson 20: Service Account & RBAC (Role-Based Access Control)**

---

## 🎯 **What You’ll Learn**

- What is a **Service Account**
- How **RBAC** works
- Difference between **Role**, **ClusterRole**, **RoleBinding**, and **ClusterRoleBinding**
- A complete example step by step

---

## 🧩 **1️⃣ What is a Service Account?**

In Kubernetes, every Pod automatically gets a **Service Account**.
It helps the Pod talk to the **Kubernetes API Server** safely.

👉 Example:
If a Pod needs to get information about other Pods in the cluster,
it must make API calls — and for that, it needs a **Service Account Token**.

Check existing service accounts:

```bash
kubectl get serviceaccount
```

You’ll see a default one named `default` inside every namespace.

---

## 🧩 **2️⃣ Create a Custom Service Account**

Let’s create a Service Account for a monitoring Pod 👇

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: monitor-sa
  namespace: default
```

Apply it:

```bash
kubectl apply -f monitor-sa.yaml
```

Now `monitor-sa` is ready to be used by any Pod.

---

## 🧩 **3️⃣ What is RBAC (Role-Based Access Control)?**

RBAC means controlling **who can do what** in Kubernetes.
It defines permissions for users or service accounts.

There are 4 main RBAC components 👇

| Component              | What It Does                                     |
| ---------------------- | ------------------------------------------------ |
| **Role**               | Gives permissions inside one namespace           |
| **ClusterRole**        | Gives permissions across the whole cluster       |
| **RoleBinding**        | Connects a Role to a user or service account     |
| **ClusterRoleBinding** | Connects a ClusterRole to a user/service account |

---

## 🧱 **4️⃣ Example: Role + RoleBinding**

Let’s give `monitor-sa` permission to **list Pods** only.

### 🧩 Step 1: Create a Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

---

### 🧩 Step 2: Create a RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-binding
  namespace: default
subjects:
  - kind: ServiceAccount
    name: monitor-sa
    namespace: default
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

Apply both files:

```bash
kubectl apply -f role.yaml
kubectl apply -f rolebinding.yaml
```

✅ Now `monitor-sa` can only **read Pods** in the default namespace.

---

## 🧩 **5️⃣ Attach Service Account to a Pod**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: monitor-pod
spec:
  serviceAccountName: monitor-sa
  containers:
    - name: monitor
      image: curlimages/curl
      command: ["sleep", "3600"]
```

✅ This Pod can now call Kubernetes API to list Pods
because it’s using the `monitor-sa` service account which has that Role.

---

## ⚖️ **6️⃣ Role vs ClusterRole**

| Feature  | Role                 | ClusterRole                                            |
| -------- | -------------------- | ------------------------------------------------------ |
| Scope    | One namespace        | Whole cluster                                          |
| Used for | Namespace-level apps | Cluster-level tools (e.g., kube-proxy, metrics-server) |

---

## ⚡ **7️⃣ Common Use Cases**

| Use Case                      | RBAC Setup                          |
| ----------------------------- | ----------------------------------- |
| Monitoring tools (Prometheus) | ClusterRole + ClusterRoleBinding    |
| Namespace dashboards          | Role + RoleBinding                  |
| API automation                | Custom ServiceAccount + RoleBinding |

---

## 🧠 **Simple Real-Life Example**

Imagine a company with many teams.
Each team = a **namespace**
Your **Service Account** = your **ID card**
Your **Role** = your permissions (what tasks you can do)
Your **RoleBinding** = the office saying “This person can do these tasks.”

---

## ✅ **Summary**

| Concept                              | Meaning                                    |
| ------------------------------------ | ------------------------------------------ |
| **ServiceAccount**                   | Identity of a Pod                          |
| **Role / ClusterRole**               | Defines what actions are allowed           |
| **RoleBinding / ClusterRoleBinding** | Gives permission to a specific user or Pod |
| **RBAC**                             | Controls who can do what in the cluster    |

---

## 📝 **Quick Recap Quiz**

1️⃣ Why do we need a Service Account?
2️⃣ What’s the difference between Role and ClusterRole?
3️⃣ What does RoleBinding connect?
4️⃣ What is the main purpose of RBAC?

---

You did great, Juboraj! 💪
Now you understand **Security & Access Control** in Kubernetes — a very powerful skill.

Next, we’ll move to 👉
**Lesson 21: Kubernetes Network Policies**
(where you’ll learn how to control and secure Pod-to-Pod communication).

Ready to start Lesson 21? 🌐
