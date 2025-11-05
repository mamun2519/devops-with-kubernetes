## 🔁 Part 3: Rollback (If the new version has issues)

Imagine your new **nginx version** has a **bug** 🐞
Now you want to go back to the **previous stable version**.

That’s super easy 👇

```bash
kubectl rollout undo deployment/nginx-deployment
```

Kubernetes will **automatically roll back** your deployment to the **last working version** ⚙️

### 🔹 Bonus Commands

| Task                             | Command                                                            |
| -------------------------------- | ------------------------------------------------------------------ |
| View rollout history             | `kubectl rollout history deployment/nginx-deployment`              |
| Roll back to a specific revision | `kubectl rollout undo deployment/nginx-deployment --to-revision=2` |
| Check deployment status          | `kubectl get deployment`                                           |
| View detailed deployment info    | `kubectl describe deployment nginx-deployment`                     |

### ⚙️ Real-Life Analogy

| Concept                   | Burger Shop Example 🍔             | Kubernetes Equivalent |
| ------------------------- | ---------------------------------- | --------------------- |
| New cook is being trained | A new pod is being created         | Rolling Update        |
| Old cook slowly replaced  | Old pod is being deleted gradually | Rolling Update        |
| New recipe has a problem  | New version has a bug              | Rollback              |
| Returning to old recipe   | Reverting to previous version      | Rollback Command      |
