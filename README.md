# 🚀 Kubernetes Volumes Lab (Assignment 6)
## Production‑Style Storage: From Ephemeral to Persistent to Multi‑Volume Architecture

![Architecture Diagram](architecture.png)

<p align="center">
  <b>Kubernetes Storage Evolution Lab</b>
</p>

---

# 🔥 Overview

In this lab, we explored how Kubernetes handles storage across different levels of complexity — starting from **temporary in‑Pod storage**, moving to **node-level storage**, then to **persistent cluster storage**, and finally combining everything into a **real-world multi-volume architecture**.

This lab simulates how modern applications manage:
- temporary cache
- persistent data
- external configuration

---

# 🎯 Lab Objectives

By completing this lab, you will:

- Understand ephemeral vs persistent storage
- Share data between containers using volumes
- Access node storage using `hostPath`
- Create and bind `PersistentVolume` and `PersistentVolumeClaim`
- Persist data across Pod restarts
- Design a Pod with multiple storage layers
- Use ConfigMap as a configuration source

---

# 🧱 Architecture Flow

This lab follows a clear evolution:

```text
emptyDir → hostPath → PersistentVolume → PersistentVolumeClaim → Multi‑Volume Pod
```

Each step builds on the previous one until we reach a production-like design.

---

# 📁 Project Structure

```bash
K8s_Assignment_6
│
├── 01-emptydir-volume.yaml
├── 02-hostpath-volume.yaml
├── 03-persistentvolume.yaml
├── 04-persistentvolumeclaim.yaml
├── 05-pod-with-pvc.yaml
├── 06-app-pv.yaml
├── 06-deployment-with-pvc.yaml
└── 07-multi-volume-pod.yaml
```

---

# 🧪 Task 1 — emptyDir (Ephemeral Storage)

We created a Pod with two containers:

- ✍️ Writer → writes timestamps
- 👀 Reader → reads the same file

Both share:
```bash
/shared
```

### 💡 Key Insight
- Data exists **only while the Pod is alive**
- Deleting the Pod = data loss

### 📸 Screenshot 01
![ S1 ](Screenshot_1.png)
![ S1 ](Screenshot_1.2.png)
![ S1 ](Screenshot_1.3.png)


---

# 🧪 Task 2 — hostPath (Node Storage)

We created a directory on the node:
```bash
/tmp/webdata
```

Then mounted it inside an nginx Pod.

### 💡 Key Insight
- Pod reads directly from the node
- Changes on node → reflect instantly inside Pod

### 📸 Screenshot 02
![ S2 ](Screenshot_2.png)

---

# 🧪 Task 3 — PersistentVolume (PV)

We created:
```text
my-pv
```

With:
- 1Gi storage
- Retain policy
- ReadWriteOnce

### 💡 Key Insight
- PV is independent storage in the cluster

---

# 🧪 Task 4 — PersistentVolumeClaim (PVC)

We created:
```text
my-pvc
```

And bound it to `my-pv`.

### 💡 Key Insight
- PVC = request for storage
- Kubernetes handles binding automatically

### 📸 Screenshot 03
![ S3 ](Screenshot_3.png)

---

# 🧪 Task 5 — Persistent Data in Pod

We used PVC inside a Pod:

Steps:
1. Create file
2. Delete Pod
3. Recreate Pod
4. Verify file still exists

### ✅ Result
Data persisted successfully

### 📸 Screenshot 04
![ S4 ](Screenshot_4.png)

---

# 🧪 Task 6 — Multi‑Volume Pod (Real‑World Design)

We created a Pod with **3 storage layers**:

| Volume Type | Mount Path | Purpose |
|--------|--------|--------|
| emptyDir (Memory) | /tmp/cache | Fast temporary cache |
| PVC | /data | Persistent storage |
| ConfigMap | /etc/config | Application config |

### 💡 Key Insight
Real applications use **multiple storage types simultaneously**

### 📸 Screenshot 05
![ S5 ](Screenshot_5.png)

---

# 🧠 Key Learnings

## 🔹 emptyDir
- Temporary
- Pod lifecycle only

## 🔹 hostPath
- Direct node access
- Not recommended for production

## 🔹 PV & PVC
- Persistent storage
- Decoupled from Pods

## 🔹 Multi‑Volume Architecture
- Cache + Data + Config
- Matches real production systems

---

# 🏁 Final Result

This lab demonstrates the full journey of Kubernetes storage:

```text
Ephemeral → Node Storage → Persistent → Multi‑Layer Architecture
```

Which reflects how real-world systems are designed:
- scalable
- maintainable
- reliable

---

# 🚀 Next Step

Try:
- Using StorageClass (Dynamic Provisioning)
- Using NFS / Cloud Storage
- Converting this setup into a Deployment

---

💡 *This lab is a strong foundation for production-grade Kubernetes storage design.*
