Here’s a complete list of **Helm hooks** that you can use for pre/post-installation (and upgrade/lifecycle) jobs in a Helm chart:

---

## 🚀 Helm Hook Lifecycle Events

| Hook Type       | Description                                                     |
| --------------- | --------------------------------------------------------------- |
| `pre-install`   | Runs **before** any resources are installed (including CRDs).   |
| `post-install`  | Runs **after** all resources are installed.                     |
| `pre-delete`    | Runs **before** Helm deletes any resources on `helm uninstall`. |
| `post-delete`   | Runs **after** Helm deletes all resources on `helm uninstall`.  |
| `pre-upgrade`   | Runs **before** any resources are upgraded.                     |
| `post-upgrade`  | Runs **after** all resources are upgraded.                      |
| `pre-rollback`  | Runs **before** a release is rolled back.                       |
| `post-rollback` | Runs **after** a release is rolled back.                        |
| `test`          | Runs when `helm test` is executed.                              |

---

## ✅ Example Use Cases

| Hook            | Typical Use Case                                 |
| --------------- | ------------------------------------------------ |
| `pre-install`   | Create secrets, run migrations, validate configs |
| `post-install`  | Notify service, trigger an init job              |
| `pre-delete`    | Graceful shutdown, revoke credentials            |
| `post-delete`   | Cleanup, delete temp buckets, notify system      |
| `pre-upgrade`   | Backup data, drain traffic                       |
| `post-upgrade`  | Health checks, warm-up cache                     |
| `pre-rollback`  | Alert ops, take a snapshot                       |
| `post-rollback` | Cleanup, redeploy sidecars                       |
| `test`          | Smoke tests, connectivity tests                  |

---

## 📝 Sample Annotation Format

```yaml
metadata:
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-delete-policy": hook-succeeded
```

---

## 🧹 Hook Delete Policies

Control how Helm cleans up the hook resource:

| Policy                 | Meaning                                                              |
| ---------------------- | -------------------------------------------------------------------- |
| `hook-succeeded`       | Delete hook job only if it **succeeds**                              |
| `hook-failed`          | Delete hook job only if it **fails**                                 |
| `before-hook-creation` | Delete any existing hook job before new one is created (re-run safe) |
| `always`               | Always delete hook job after it's run                                |

You can combine multiple:

```yaml
annotations:
  "helm.sh/hook-delete-policy": hook-succeeded, before-hook-creation
```

---

## 🔧 Practical Tip

For repeatable jobs (like `pre-upgrade` or `post-upgrade`), always use:

* **templated job names** to avoid conflicts
* `before-hook-creation` to clean up old hooks

---

Would you like a Helm chart with all of these hooks as example templates?
