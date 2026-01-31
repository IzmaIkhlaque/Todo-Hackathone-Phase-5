---
name: helm-charts
description: Creates Helm Charts as spec-driven blueprints for ObsidianList deployment in Phase 4.
---

# Helm Charts Skill

## Purpose
Package ObsidianList as reusable Helm Chart for spec-driven Kubernetes deployment, following PDF blueprint requirements.

## Process
1. Create chart folder: helm create obsidianlist-chart
2. Define values.yaml: replicas: 1, image: obsidianlist-backend, secrets: BETTER_AUTH_SECRET
3. Templates: deployment.yaml, service.yaml from kubernetes-deployment-skill
4. Install: helm install obsidianlist ./obsidianlist-chart --values values.yaml
5. Use AIOps: kagent "generate Helm blueprint for ObsidianList".

## Examples
values.yaml:
replicas: 2
image:
  frontend: obsidianlist-frontend:latest
dbUrl: "{{ .Values.databaseUrl }}"

## Guidelines
- Spec-driven: Generate from Markdown specs.
- Reusable for Phase V cloud.
- Handle env secrets securely.
- Integrate with devops-engineer-subagent.