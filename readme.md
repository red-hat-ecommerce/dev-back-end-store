# dev-back-end-store

**dev-back-end-store** is a configuration repository that holds `Development` environment YAML definitions for the `Back End Store` service in the `Red Hat E‑Commerce` ecosystem.

---

## Table of Contents

- [Overview](#overview)  
- [Repository Contents](#repository-contents)  
- [Prerequisites](#prerequisites)  
- [How to Use](#how-to-use)  
- [Deployment Components](#deployment-components)  

---

## Overview

This repository contains OpenShift YAML configuration files for setting up a development environment for the Back‑End Store application. The manifests provide definitions for Deployment, Service, Route, Horizontal Pod Autoscaler (HPA), and related networking.

These configurations are used by the dev team to spin up a working instance of the Back‑End Store service that mirrors (as much as possible) the production environment, facilitating testing, debugging, and development.

---

## Repository Contents

| File | Description |
|---|-------------|
| `deployment.yaml` | Deployment spec for the Back‑End Store pods (containers, replica count, etc.) |
| `service.yaml` | Service definition to expose/delegate pod traffic inside the cluster |
| `route.yaml` | Route / ingress specification to expose the service externally (for dev environment) |
| `hpa.yaml` | Horizontal Pod Autoscaler configuration to scale the deployment based on CPU / memory / custom metrics |
| `readme.md` | This file — documentation for the repository |

---

## Prerequisites

To use these configurations, you’ll need:

- A OpenShift cluster (dev cluster)  
- `oc` CLI access with adequate permissions  
- Appropriate container image(s) of the Back‑End Store application, built and pushed somewhere accessible by the cluster  
- Any required environment variables, secrets, or config maps (database credentials, external services endpoints, etc.) in your dev environment  

---

## How to Use

1. **Set up required secrets/configs**  
   Ensure that any needed secrets, ConfigMaps, or settings (e.g. DB credentials, external API endpoints) are present in your dev cluster namespace.

2. **Apply the manifests**  
   Use `oc` to apply the YAML files in correct order:

   ```bash
   oc apply -f deployment.yaml
   oc apply -f service.yaml
   oc apply -f route.yaml
   oc apply -f hpa.yaml
   ```

3. **Verify**  
   - Check deployment pods are up: `oc get po`  
   - Ensure service is running: `oc get svc`  
   - Check route/ingress is reachable.  
   - Check HPA status: `oc get hpa`  

---

## Deployment Components

Here’s a bit more detail on each component:

- **Deployment**: Specifies how many replicas to run, container images, resource requests/limits.  
- **Service**: Internal load balancing within the cluster; defines on which ports the pods are reachable.  
- **Route**: Used in OpenShift (or ingress/ingress‑controllers in upstream Kubernetes) to expose HTTP(s) traffic.  
- **HPA**: Defines autoscaling behavior: minimum and maximum pods, thresholds (CPU / memory or custom metrics).