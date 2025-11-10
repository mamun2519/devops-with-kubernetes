# 💾 Lesson 10: Volumes & Persistent Storage in Kubernetes

## 🎯 Goal of This Lesson

After this lesson, you will learn:

- Why Pod data is temporary by default
- What a Volume is and why we use it
- Different Volume types (`emptyDir`, `hostPath`, `PersistentVolume`, `PersistentVolumeClaim`)
- What PV (Persistent Volume) and PVC (Persistent Volume Claim) are
- Real-life examples of data storage in Kubernetes

## 🧠 1️⃣ The Problem — Pod Data Gets Lost!

Imagine you run an Nginx container and create some files inside
`/usr/share/nginx/html`.

Now if the Pod restarts or gets deleted —
👉 all files are gone! 😢

That’s because Kubernetes Pod file systems are **temporary (ephemeral)**.
When the Pod dies, the data dies too.

---

## 💡 2️⃣ The Solution — Volumes

🪄 **Volume** means a separate storage space outside the Pod where data is stored.
Even if the Pod restarts, the data will still exist.

You can share the same Volume between multiple containers inside the same Pod.

---

## 🔹 Example 1: `emptyDir` Volume

`emptyDir` creates an empty folder when the Pod starts.
When the Pod is deleted, the data also disappears. (Temporary storage)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-demo
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: mydata
          mountPath: /usr/share/nginx/html
  volumes:
    - name: mydata
      emptyDir: {}
```

👉 Here, `emptyDir` is mounted to `/usr/share/nginx/html`.
Any files written there will stay only while the Pod is running.

---

## 🔹 Example 2: `hostPath` Volume

This type mounts a **folder from the host machine** into the Pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-demo
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: hostvol
          mountPath: /data
  volumes:
    - name: hostvol
      hostPath:
        path: /tmp/hostdata
        type: DirectoryOrCreate
```

👉 A folder `/tmp/hostdata` will be created on the **host machine**,
and it will appear as `/data` inside the container.

Even if the Pod is deleted, data stays on the host machine ✅

---

## 🏗️ 3️⃣ Persistent Volume (PV) & Persistent Volume Claim (PVC)

Now imagine you need **permanent storage** —
something that stays even after Pods are deleted and can be reused by other Pods.

Kubernetes gives us two resources for that:
**Persistent Volume (PV)** and **Persistent Volume Claim (PVC)**

---

### 🔹 Step 1: Persistent Volume (PV)

PV is a **cluster-level resource** — storage managed by Kubernetes.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-demo
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

---

### 🔹 Step 2: Persistent Volume Claim (PVC)

PVC is a **request for storage** made by the user or developer.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

---

### 🔹 Step 3: Pod Using PVC

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: data
          mountPath: /usr/share/nginx/html
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: pvc-demo
```

👉 Now, whatever the Pod writes to `/usr/share/nginx/html`
will be saved in `/mnt/data`, and it will **stay** even after the Pod is deleted.

---

## 📦 4️⃣ PV vs PVC (In Simple Words)

| Concept    | Persistent Volume (PV) | Persistent Volume Claim (PVC) |
| ---------- | ---------------------- | ----------------------------- |
| Role       | Provides storage       | Requests storage              |
| Created by | Cluster Admin          | Developer                     |
| Scope      | Cluster-wide           | Namespace specific            |

---

## 🧰 5️⃣ Real-Life Example

Imagine you’re running a **WordPress app** on Kubernetes.
When users upload photos, they’re saved in the `uploads` folder.

If the Pod gets deleted, those photos will be gone 😡
To prevent this — attach a **PVC** to your WordPress Pod.

Now all photos will be stored safely in `/mnt/data`,
and they’ll stay even if the Pod is restarted ✅

---

## 🧩 Assignment (Your Task)

1️⃣ Create a **PersistentVolume (1Gi)**
2️⃣ Create a **PersistentVolumeClaim (500Mi)**
3️⃣ Create an **nginx-pod** that uses that PVC
4️⃣ Run these commands to test:

```bash
kubectl exec -it nginx-pod -- touch /usr/share/nginx/html/test.txt
kubectl delete pod nginx-pod
```

Then create a new Pod and check if `/mnt/data/test.txt` still exists.

---

🎉 That’s it! Now you understand how **Kubernetes Persistent Storage** really works.

---

Let’s go Juboraj 💪
Finish this assignment and get ready for the next class —

📚 **Lesson 13: Kubernetes StatefulSet — Managing Stateful Applications (like Databases)**

Would you like me to start **Lesson 13** now?
