# Distributed Lovable — AI-Driven Code Generation SaaS Platform

A distributed, multi-tenant SaaS platform that generates full React applications from natural-language prompts (e.g., "Build a snake game in React"), inspired by tools like Lovable and v0.dev.

## What it does

Users describe an app in plain English, and the platform streams back a working React project in real time — generating code, provisioning a live preview environment, and persisting the output, all through a microservices backend.

## Architecture

The system is split into independent services, each with a single responsibility:

- **api-gateway** — single entry point, routing, and auth
- **discovery-service** — service registry for inter-service discovery
- **config-service** — centralized configuration management
- **account-service** — user accounts, subscriptions, and billing
- **intelligence-service** — orchestrates Spring AI / LLM calls, prompt handling, and code generation logic
- **workspace-service** — manages per-user project workspaces and file persistence
- **common-lib** — shared DTOs, utilities, and contracts across services
- **k8s/** — Kubernetes manifests for deployment (pods, services, ingress)

## Key features

- **Real-time streaming** — Server-Sent Events (SSE) stream generated code token-by-token to the client as it's produced, rather than waiting for the full response.
- **Live preview environments** — each generation spins up a Kubernetes pod (via Fabric8 client) with its own Ingress route, so users can see a working live preview of their generated app almost immediately.
- **Persistent storage** — generated projects are stored in MinIO (S3-compatible object storage) with NFS-backed shared volumes for build environments.
- **Multi-tenant SaaS** — token quota tracking, role-based access control (RBAC), and Stripe-based subscription billing.

## Tech stack

Java, Spring Boot, Spring AI, Spring Cloud (Gateway, Config, Discovery), Kubernetes, Fabric8 Kubernetes Client, MinIO, Docker

## Running locally

> Fill in your actual setup steps here, e.g.:
```bash
# Clone the repo
git clone https://github.com/vikikumar-pandit/distributed-lovable.git
cd distributed-lovable

# Start dependencies (Postgres, MinIO, etc.) via docker-compose
docker-compose up -d

# Build and run each service
./mvnw spring-boot:run -pl config-service
./mvnw spring-boot:run -pl discovery-service
./mvnw spring-boot:run -pl api-gateway
# ...and so on
```

## Status

Actively developed as a personal project to apply production-style distributed systems design (microservices, event-driven flows, Kubernetes orchestration) alongside AI-integrated backend engineering.

