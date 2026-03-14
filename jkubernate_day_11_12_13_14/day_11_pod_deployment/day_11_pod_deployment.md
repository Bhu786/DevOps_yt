[](./k8s_artitacture.png)

I analyzed **all 10 pages** of your Kubernetes PDF and created **short + solid revision notes without skipping any topic**. This is optimized for **interview revision / DevOps quick recall**.

Source: 

---

# Kubernetes Short & Solid Notes (All Pages)

## 1. Kubernetes Overview

**Kubernetes (K8s)** = Open-source **container orchestration platform**

Used to:

* Deploy containers
* Scale applications
* Load balance traffic
* Manage networking & storage
* Maintain desired application state

**Simple line for interview**

👉 *Kubernetes automates deployment, scaling, and management of containerized applications across a cluster of machines.*

---

# 2. Kubernetes Architecture

Kubernetes cluster has **2 main parts**

| Component     | Role             |
| ------------- | ---------------- |
| Control Plane | Manages cluster  |
| Worker Nodes  | Run applications |

---

# 3. Control Plane (Master Node)

Control plane manages the **entire cluster decisions**.

### Components

### 1️⃣ API Server (kube-apiserver)

Entry point of Kubernetes.

Functions:

* Accept REST requests
* Validate requests
* Update cluster state
* Communication hub for all components

---

### 2️⃣ ETCD

Distributed **key-value database**

Stores:

* Pods
* Deployments
* Services
* Cluster metadata

👉 **Important**

```
Kubernetes brain = etcd
```

---

### 3️⃣ Controller Manager (kube-controller-manager)

Ensures **actual state = desired state**

Controllers inside:

| Controller            | Work                    |
| --------------------- | ----------------------- |
| ReplicaSet Controller | Maintain number of pods |
| Deployment Controller | Manage deployments      |
| Job Controller        | Run batch jobs          |
| Node Controller       | Manage node lifecycle   |

---

### 4️⃣ Scheduler (kube-scheduler)

Assigns **pods → worker nodes**

Based on:

* CPU
* Memory
* Affinity rules
* Constraints

---

### 5️⃣ Cloud Controller Manager

Used in cloud environments (AWS/GCP/Azure)

Manages:

* Load balancers
* Storage volumes
* Cloud integration

---

# 4. Worker Nodes

Worker nodes run **actual applications (containers)**.

### Components

---

### 1️⃣ Kubelet

Agent running on every node.

Responsibilities:

* Communicates with API server
* Ensures containers are running
* Maintains pod health

---

### 2️⃣ Kube-Proxy

Handles **network routing**

Functions:

* Pod communication
* Service communication
* Creates iptables / IPVS rules

---

### 3️⃣ Container Runtime

Software that runs containers.

Examples:

* containerd
* CRI-O
* Docker (deprecated after K8s 1.23)

Functions:

* Pull images
* Run containers
* Manage lifecycle

---

### 4️⃣ Pods

Smallest unit in Kubernetes.

Characteristics:

* Contains **one or more containers**
* Shared networking
* Shared storage
* Same node scheduling

Important:

👉 Each pod gets **its own IP address**

---

# 5. Namespaces

Namespaces divide cluster into **virtual environments**.

Used for:

* Organizing resources
* Application isolation
* Access management
* Multi-team usage

---

### Default Namespaces

| Namespace       | Purpose                      |
| --------------- | ---------------------------- |
| default         | Default resources            |
| kube-system     | Kubernetes system components |
| kube-public     | Public cluster information   |
| kube-node-lease | Node heartbeat               |

---

# 6. Kubernetes Services

Service = **stable access point for pods**

Because:

* Pods are temporary
* Pod IPs change

Service provides:

* Stable IP
* DNS name
* Load balancing

Uses:

```
Labels + Selectors
```

to find pods.

---

# 7. Service Types

## 1️⃣ ClusterIP (Default)

Scope: Internal only

Use case:

* Microservice communication

Access:

```
Pod → Service → Pod
```

---

## 2️⃣ NodePort

External access via node port.

Port range:

```
30000 — 32767
```

Access format:

```
<NodeIP>:<NodePort>
```

Mostly used for **testing**

---

## 3️⃣ LoadBalancer

Production use.

Creates:

```
AWS ELB
ALB
NLB
```

Used for:

* Web applications
* Public APIs

---

## 4️⃣ ExternalName

Maps service → external DNS.

Example:

```
service → external database
```

---

## 5️⃣ Headless Service

ClusterIP = None

DNS returns:

```
List of pod IPs
```

Used with:

* StatefulSets
* Kafka
* Cassandra

---

# 8. Deployment

Deployment manages **pods lifecycle**

Features:

| Feature        | Meaning                    |
| -------------- | -------------------------- |
| Scaling        | Increase / decrease pods   |
| Rolling Update | Update without downtime    |
| Rollback       | Revert to previous version |

---

# 9. ReplicaSet

Ensures **required number of pods running**

Example:

```
replicas = 3
```

If one pod dies → new pod created automatically.

---

# 10. StatefulSets

Used for **stateful applications**

Examples:

* Databases
* Kafka
* Cassandra

Features:

* Stable network identity
* Ordered deployment
* Persistent storage

---

# 11. DaemonSet

Ensures pod runs on **every node**.

Common uses:

* Logging agents
* Monitoring agents
* Network plugins

Example:

```
Prometheus node exporter
Fluentd
```

---

# 12. Jobs & CronJobs

### Job

Runs **batch task once**

Example:

```
data processing
backup
```

---

### CronJob

Runs job **on schedule**

Example:

```
every night backup
```

Same as Linux cron.

---

# 13. Ingress

Ingress manages **external HTTP/HTTPS traffic**

Features:

* Load balancing
* SSL termination
* Domain routing

Example:

```
example.com → service
```

---

# 14. Kubernetes Networking

### Pod-to-Pod Networking

* All pods communicate directly
* Flat network
* Each pod has unique IP

---

### Service Discovery

Kubernetes automatically assigns **DNS names to services**

Example:

```
service-name.namespace.svc.cluster.local
```

---

### Network Policies

Used for **security**

Controls:

```
which pod can talk to which pod
```

Based on:

* labels
* namespaces

---

# 15. Affinity

Affinity = scheduling rule

Meaning:

```
"I want my pod near something"
```

---

### Node Affinity

Run pods on specific nodes.

Example:

```
node with SSD
node type = high memory
```

Based on:

```
node labels
```

---

### Pod Affinity

Run pod **near another pod**

Based on:

```
pod labels
```

---

### Pod AntiAffinity

Avoid scheduling pods together.

Example:

```
spread replicas across nodes
```

Used for:

```
high availability
```

---

# 16. Taints and Tolerations

Used for **node restrictions**

---

## Taint

Applied on **nodes**

Meaning:

```
"Pods cannot run here"
```

Example command

```
kubectl taint nodes gpu-node key=gpu:NoSchedule
```

---

## Toleration

Applied on **pods**

Meaning:

```
"This pod is allowed on tainted node"
```

Example:

```yaml
tolerations:
- key: "gpu"
  operator: "Equal"
  value: "true"
  effect: "NoSchedule"
```

---

# 17. Taint Effects

| Effect           | Meaning                            |
| ---------------- | ---------------------------------- |
| NoSchedule       | Pod not scheduled unless tolerated |
| PreferNoSchedule | Avoid scheduling                   |
| NoExecute        | Remove existing pods               |

---

# 18. Final Summary

| Concept       | Purpose                     |
| ------------- | --------------------------- |
| Control Plane | Manage cluster              |
| Worker Nodes  | Run applications            |
| Pod           | Smallest unit               |
| Service       | Stable access to pods       |
| Deployment    | Manage pods lifecycle       |
| ReplicaSet    | Maintain pod count          |
| StatefulSet   | Stateful apps               |
| DaemonSet     | Pod on every node           |
| Job           | Run task once               |
| CronJob       | Scheduled jobs              |
| Ingress       | External traffic routing    |
| Affinity      | Pod placement rule          |
| Taint         | Restrict nodes              |
| Toleration    | Allow pods on tainted nodes |

---

=========================================================

