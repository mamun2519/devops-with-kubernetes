# 🌐 **Lesson 19: Kubernetes Network Policies**

## 🎯 **What You’ll Learn**

- What is a **Network Policy**
- Control Pod-to-Pod communication
- Control **Ingress** (incoming) and **Egress** (outgoing) traffic
- Learn with a live YAML example

## 🧠 **Simple Explanation**

Imagine your cluster has many Pods:

| Pod   | App Type   |
| ----- | ---------- |
| Pod-A | Web        |
| Pod-B | Database   |
| Pod-C | Monitoring |

Rules:

- Web Pod can talk to Database Pod
- Monitoring Pod can talk to all Pods
- Random Pods cannot talk to Database Pod

✅ Network Policy controls this **traffic**, allowing or blocking Pod communication.

## ⚙️ **What is a Network Policy?**

- A Kubernetes resource to control **Ingress** and **Egress** traffic for Pods.
- It’s like a **Pod-level firewall** — not cluster-wide.

## 🧩 **Key Points**

| Field                      | Meaning                                  |
| -------------------------- | ---------------------------------------- |
| **podSelector**            | Select which Pods this policy applies to |
| **policyTypes**            | Ingress / Egress / Both                  |
| **ingress / egress rules** | Rules to allow specific traffic          |

## 🔧 **Example 1️⃣: Ingress Policy**

Allow only Web Pods to access Database Pod:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: db
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: web
      ports:
        - protocol: TCP
          port: 3306
```

✅ Explanation:

- Select Pods with label `app=db`
- Only Pods with label `app=web` can access TCP port 3306

## 🔧 **Example 2️⃣: Egress Policy**

Allow Web Pod to talk only to Database Pod:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: web-egress
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
    - Egress
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: db
      ports:
        - protocol: TCP
          port: 3306
```

## 🔍 **Real-Life Analogy**

Think of an office:

- Database room → only Web team can enter
- Admin room → only Admin team can enter
- Visitors → cannot enter

Network Policy is like a **digital access card** for Pods — it tells who can go where.

## 📊 **Summary Table**

| Concept              | Use                                                                  |
| -------------------- | -------------------------------------------------------------------- |
| **podSelector**      | Choose which Pods the policy applies to                              |
| **policyTypes**      | Ingress / Egress / Both                                              |
| **ingress / egress** | Rules to allow traffic                                               |
| **Default Deny**     | Without policy → all open; with policy → only allowed traffic passes |

## 🧩 **Assignment**

1️⃣ Create 2 Pods: `web` and `db`
2️⃣ Apply **Ingress Network Policy** on `db` so only `web` Pod can reach it
3️⃣ Apply **Egress Network Policy** on `web` so it can only talk to `db` Pod
4️⃣ Test using `kubectl exec` to verify connection
