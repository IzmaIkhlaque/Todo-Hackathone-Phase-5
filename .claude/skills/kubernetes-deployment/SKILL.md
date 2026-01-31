---
name: kubernetes-deployment
description: Deploys ObsidianList to local Kubernetes on Minikube for Phase 4.
---

# Kubernetes Deployment Skill

## Purpose
Generate and apply K8s YAML for ObsidianList deployment on Minikube, including pods, services, deployments, following PDF Kubernetes requirements.

## Process
1. Start Minikube: minikube start.
2. Generate YAML: Deployment for 1-3 replicas, Pod spec with Docker images, Service type LoadBalancer.
3. Expose ports: e.g., frontend 3000, backend 8000.
4. Apply: kubectl apply -f deployment.yaml.
5. Use AIOps: kubectl-ai "deploy ObsidianList with 2 replicas".

## Examples
Deployment YAML:
apiVersion: apps/v1
kind: Deployment
metadata: name: obsidianlist-frontend
spec: replicas: 2
  selector: matchLabels: app: frontend
  template: metadata: labels: app: frontend
    spec: containers: - name: frontend image: obsidianlist-frontend port: 3000

## Guidelines
- Local Minikube only.
- Prep for Helm in next skill.
- Ensure JWT/Neon connections work in pods.
- Integrate with devops-engineer-subagent.