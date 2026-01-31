---
name: docker-containerization
description: Containerizes ObsidianList components using Docker for Phase 4 local deployment.
---

# Docker Containerization Skill

## Purpose
Build Docker images for ObsidianList frontend (Next.js), backend (FastAPI + MCP + Agents SDK), and DB proxy, following PDF containerization requirements.

## Process
1. Create Dockerfile for each component (e.g., frontend: FROM node:20, npm install, build, EXPOSE 3000).
2. Use multi-stage builds for optimization.
3. Handle .env secrets (BETTER_AUTH_SECRET, DATABASE_URL).
4. Build images: docker build -t obsidianlist-frontend .
5. Test locally: docker run -p 3000:3000 obsidianlist-frontend.

## Examples
Frontend Dockerfile:
FROM node:20 as builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build
FROM node:20-slim
COPY --from=builder /app/.next ./.next
CMD ["npm", "start"]

## Guidelines
- Follow PDF Docker requirements.
- Optimize for Minikube (small images).
- Integrate with devops-engineer-subagent.