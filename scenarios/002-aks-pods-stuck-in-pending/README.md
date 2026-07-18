# AKS Incident #002 - Pods Stuck in Pending

## 📌 Overview

A deployment was successfully created in AKS, but the pods remained in the **Pending** state. No containers started, and the application was unavailable.

---

## 🖼 Architecture / Incident Diagram

![Pods Pending](architecture-overview.png)

---

## ❗ Symptoms

- Deployment created successfully
- Pods stuck in `Pending`
- No containers started
- No application logs
- Application unavailable

---

## 🔍 Investigation

### Check pod status

```bash
kubectl get pods
```

Pods remained in `Pending`.

### Describe the pod

```bash
kubectl describe pod <pod-name>
```

Example Events:

```
0/5 nodes are available:
5 Insufficient memory.
Preemption is not helpful for scheduling.
```

The **Events** section explains why the scheduler cannot place the pod.

---

## 💡 Common Causes

- Insufficient CPU or memory
- Cluster Autoscaler reached its maximum size
- Node affinity or node selector mismatch
- Untolerated taints
- Persistent Volume constraints
- Resource requests too high

---

## ✅ Resolution

- Right-size CPU and memory requests
- Increase the node pool or autoscaler limits
- Verify node affinity and tolerations
- Resolve storage constraints
- Confirm the Cluster Autoscaler is functioning correctly

---

## 📚 Lessons Learned

- A `Pending` pod is not necessarily broken—it is waiting for a suitable node.
- Always inspect the **Events** section with `kubectl describe pod`.
- Resource requests directly affect scheduling.
- Verify autoscaler and node capacity before troubleshooting the application.

---

## 🛠 Commands Used

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl get nodes
kubectl top nodes
kubectl describe node <node-name>
```