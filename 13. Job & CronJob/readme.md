# 🕒 **Lesson 13: Job & CronJob — One-Time and Scheduled Tasks in Kubernetes**

## 🎯 Goal of This Lesson

After this lesson, you will know 👇

- What is a **Job** in Kubernetes
- What is a **CronJob** and how it works
- When to use Job instead of Deployment
- Real-life examples (backup, cleanup, report generator, etc.)
- Full YAML examples with explanation

## 🧠 1️⃣ The Problem — Some tasks should run only once!

Let’s say you have a script that **cleans a database** or **takes a backup**.
You want this to run **only once** and then stop.
You don’t want it to keep running forever like a Deployment.

👉 That’s where **Job** is useful!

## 💡 2️⃣ What is a Job?

🧩 A **Job** is a Kubernetes resource that runs one or more Pods **until the task is complete**.
When the task finishes, the Pod stops — and Kubernetes won’t restart it.

### 🔹 Example: One-time Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: print-date
spec:
  template:
    spec:
      containers:
        - name: date-job
          image: busybox
          command: ["sh", "-c", "echo The date is $(date)"]
      restartPolicy: Never
```

### 🧩 Explanation

| Part                   | Meaning                                                                   |
| ---------------------- | ------------------------------------------------------------------------- |
| `kind: Job`            | This job runs only once                                                   |
| `restartPolicy: Never` | If it fails, Kubernetes makes a new Pod, but doesn’t re-run finished ones |
| `command`              | The command that runs once and exits                                      |

### 🧰 Check Job Status

```bash
kubectl apply -f job.yaml
kubectl get jobs
kubectl get pods
kubectl logs <pod-name>
```

Example output 👇

```
The date is Mon Nov 3 18:20:05 UTC 2025
```

✅ Done! The job ran one time and finished.

## ⏰ 3️⃣ CronJob — Run Tasks on Schedule

Now, imagine you want to run that same job **every few minutes or every day** —
for example, every morning or every 2 minutes.

You can do that using a **CronJob** in Kubernetes.

### 🔹 Example: CronJob

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: say-hello
spec:
  schedule: "*/2 * * * *" # runs every 2 minutes
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: busybox
              command: ["sh", "-c", "echo Hello Juboraj! The time is $(date)"]
          restartPolicy: OnFailure
```

### 🧩 Explanation

| Part                       | Meaning                                     |
| -------------------------- | ------------------------------------------- |
| `schedule`                 | Uses **cron syntax** (like Linux cron jobs) |
| `"*/2 * * * *"`            | Run every 2 minutes                         |
| `jobTemplate`              | Defines what the Job will do each time      |
| `restartPolicy: OnFailure` | Only restarts if it fails                   |

### 🔎 Cron Syntax Quick Guide

| Syntax        | Meaning               |
| ------------- | --------------------- |
| `* * * * *`   | Every minute          |
| `*/5 * * * *` | Every 5 minutes       |
| `0 * * * *`   | Every hour            |
| `0 0 * * *`   | Every day at midnight |

## 🧰 4️⃣ Real-Life Examples

| Use Case              | Description                   |
| --------------------- | ----------------------------- |
| **Database backup**   | Run backup script every night |
| **Log cleanup**       | Delete old logs automatically |
| **Report generation** | Generate report every Monday  |
| **Email reminders**   | Send reminder every hour      |
