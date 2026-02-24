# Deploying Voting App on Kubernetes

A deployment of the [Docker Example Voting App](https://github.com/dockersamples/example-voting-app) on a local Kubernetes cluster using [kind](https://kind.sigs.k8s.io/).

## Architecture

```
+-------------+         +-------+         +---------+         +--------+
|  vote app   | ------> | redis | ------> |  worker | ------> |   db   |
| (port 8080) |         |       |         |         |         |  (pg)  |
+-------------+         +-------+         +---------+         +--------+
                                                                   |
+-------------+                                                    |
| result app  | <-------------------------------------------------+
| (port 8081) |
+-------------+
```

| Component | Image | Description |
|-----------|-------|-------------|
| `vote` | `dockersamples/examplevotingapp_vote` | Python web app to cast votes |
| `result` | `dockersamples/examplevotingapp_result` | Node.js web app to view results |
| `worker` | `dockersamples/examplevotingapp_worker` | .NET worker that consumes votes from Redis and writes to Postgres |
| `redis` | `redis:alpine` | In-memory queue for incoming votes |
| `db` | `postgres:15-alpine` | Persistent store for vote results |

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Setup

### 1. Create the kind cluster

```bash
kind create cluster --config kind-config.yaml --name voting-app
```

This creates a single-node cluster with host port mappings for `31000` (vote) and `31001` (result).

### 2. Deploy all resources

```bash
kubectl apply -f redis-deployment.yml -f redis-service.yml
kubectl apply -f db-deployment.yml    -f db-service.yml
kubectl apply -f worker-deployment.yml
kubectl apply -f vote-deployment.yml   -f vote-service.yml
kubectl apply -f result-deployment.yml -f result-service.yml
```

Or apply everything at once:

```bash
kubectl apply -f .
```

### 3. Verify the deployment

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

## Accessing the Apps

| App | URL |
|-----|-----|
| Voting app | http://localhost:31000 |
| Results app | http://localhost:31001 |

## Cleanup

```bash
# Delete all resources
kubectl delete -f .

# Delete the kind cluster
kind delete cluster --name voting-app
```

## File Structure

```
.
├── kind-config.yaml          # kind cluster configuration (port mappings)
├── redis-deployment.yml      # Redis deployment
├── redis-service.yml         # Redis ClusterIP service
├── db-deployment.yml         # PostgreSQL deployment
├── db-service.yml            # PostgreSQL ClusterIP service
├── worker-deployment.yml     # Worker deployment
├── vote-deployment.yml       # Vote app deployment
├── vote-service.yml          # Vote app NodePort service (31000)
├── result-deployment.yml     # Result app deployment
└── result-service.yml        # Result app NodePort service (31001)
```
