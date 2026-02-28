# 🚀 Getting Started with Google Kubernetes Engine (GKE)

A beginner-friendly, interactive codelab built in the style of [Google Codelabs](https://codelabs.developers.google.com/) covering everything from containers to deploying, scaling, and updating a live app on GKE.

[![Made with GKE](https://img.shields.io/badge/Google%20Kubernetes%20Engine-GKE-blue?style=flat&logo=google-cloud)](https://cloud.google.com/kubernetes-engine)
[![Beginner Friendly](https://img.shields.io/badge/Level-Beginner-green?style=flat)](https://cloud.google.com/kubernetes-engine/docs)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📖 About

This codelab walks complete beginners through deploying their very first containerized application on **Google Kubernetes Engine (GKE)**. No prior Kubernetes or cloud experience required.

**What you'll learn:**
- Why containers exist and what problem they solve
- What Kubernetes and GKE are (in plain English)
- Core concepts: Pods, Deployments, Services, Nodes
- How to create a GKE cluster using Cloud Shell
- How to deploy, expose, scale, and update an app
- How to perform a zero-downtime rolling update

**Format:** Interactive step-by-step codelab · 8 steps · ~30 minutes

---

## 🗂️ Project Structure

```
gke-codelab/
├── gke-codelab.html      ← Standalone interactive codelab (open in any browser)
├── gke-codelab.md        ← CLaaT-compatible markdown source
└── README.md             ← You are here
```

---

## 🖥️ Live Walkthrough

Below is a full walkthrough of all the commands executed during this codelab, with real output screenshots.

---

### Step 1 — Create a GKE Cluster

Creates a 2-node GKE cluster in `us-central1-a`. The Kubernetes Engine API is auto-enabled on first run.

```bash
gcloud container clusters create my-first-cluster \
  --num-nodes=2 \
  --zone=us-central1-a
```

![Create GKE Cluster](images/Screenshot%202026-02-28%20085609.png)

> Cluster `my-first-cluster` created with **2 nodes**, running Kubernetes `v1.34.3-gke`, STATUS: **RUNNING**

---

### Step 2 — Connect kubectl to the Cluster

Configures `kubectl` to point at the newly created cluster.

```bash
gcloud container clusters get-credentials my-first-cluster \
  --zone=us-central1-a
```

![Get Credentials](images/04-get-credentials.png)

---

### Step 3 — Verify Nodes are Ready

```bash
kubectl get nodes
```

![Get Nodes](images/05-get-nodes.png)

> Both nodes show **STATUS: Ready** — the cluster is healthy and ready for workloads.

---

### Step 4 — Deploy the App

Creates a Deployment running the `hello-app:1.0` container image.

```bash
kubectl create deployment hello-app \
  --image=gcr.io/google-samples/hello-app:1.0
```

![Create Deployment](images/06-create-deployment.png)

---

### Step 5 — Verify Pod is Running

```bash
kubectl get pods
```

![Get Pods](images/07-get-pods.png)

> Pod `hello-app-6f65bf88...` is **Running** with 0 restarts ✅

---

### Step 6 — Expose the App to the Internet

Creates a `LoadBalancer` Service that gives the app a public IP.

```bash
kubectl expose deployment hello-app \
  --type=LoadBalancer \
  --port=80 \
  --target-port=8080
```

![Expose Service](images/08-expose-service.png)

---

### Step 7 — Get the Public IP

```bash
kubectl get service hello-app --watch
```

![Get Service](images/09-get-service.png)

> External IP assigned: **`34.41.57.97`** — app is now live on the internet 🌐

---

### Step 8 — App Live: Version 1.0

Opening the external IP in a browser shows the running app:

![Hello World v1](images/01-hello-world-v1.png)

> `Version: 1.0.0` · Hostname: `hello-app-6f65bf884d-trk2m`

---

### Step 9 — Scale to 3 Replicas

```bash
kubectl scale deployment hello-app --replicas=3
```

![Scale Replicas](images/10-scale-replicas.png)

> **One command** to go from 1 Pod to 3 — GKE load balances across all three automatically.

---

### Step 10 — Verify 3 Pods + Update Image

Confirms 3 Pods are running, then updates to `hello-app:2.0`:

```bash
kubectl get pods
kubectl set image deployment/hello-app \
  hello-app=gcr.io/google-samples/hello-app:2.0
```

![Scale and Update](images/11-scale-and-update.png)

---

### Step 11 — Rolling Update Completes

```bash
kubectl rollout status deployment/hello-app
```

![Rollout Status](images/12-rollout-status.png)

> `deployment "hello-app" successfully rolled out` — zero downtime ✅

---

### Step 12 — App Live: Version 2.0

After the rolling update, refreshing the browser shows the new version:

![Hello World v2](images/02-hello-world-v2.png)

> `Version: 2.0.0` · New Pod hostname confirms the update rolled out successfully

---

### Step 13 — Clean Up

Deletes the Service and the cluster to avoid ongoing charges.

```bash
kubectl delete service hello-app

gcloud container clusters delete my-first-cluster \
  --zone=us-central1-a
```

![Cleanup](images/13-cleanup.png)

> Service deleted · Cluster `my-first-cluster` deleted from `us-central1-a` ✅

---

##  Prerequisites 📋

| Requirement | Details |
|---|---|
| Google Account | Gmail works |
| Google Cloud Project | [Create one here](https://console.cloud.google.com) — $300 free credits for new accounts |
| Cloud Shell | Browser-based terminal, no local install needed |
| Billing enabled | Required for GKE cluster creation |

---

##  Cost 💰

This codelab costs approximately **$0.10–$0.30** to complete. All resources are deleted in the final cleanup step.

---

##  What's Next 🗺️

After completing this codelab, explore:

| Topic | Resource |
|---|---|
| ConfigMaps & Secrets | [kubernetes.io/docs](https://kubernetes.io/docs/concepts/configuration/) |
| Ingress & Gateway API | [GKE Docs](https://cloud.google.com/kubernetes-engine/docs/concepts/ingress) |
| Cluster Autoscaler | [GKE Docs](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-autoscaler) |
| Helm | [helm.sh](https://helm.sh) |
| CKA Certification | [cncf.io/certification/cka](https://www.cncf.io/certification/cka/) |
| Google Cloud Skills Boost | [cloudskillsboost.google](https://cloudskillsboost.google) |

---

##  License 📄

This project is open source and available under the [MIT License](LICENSE).
