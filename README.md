## System Architecture

```
                         Internet
                            |
                            v
                   Kubernetes Cluster
                            |
        +-------------------+-------------------+
        |                   |                   |
    Frontend          User Service        Note Service
   (React:80)      (Spring Boot:8081)  (Spring Boot:8082)
        |                   |                   |
        +-------------------+-------------------+
                            |
                            v
                    PostgreSQL:5432
                   (user_service + note_service DBs)
```

## CI/CD Architecture

```
GitHub (Source Code)
    |
    | [Webhook Trigger]
    v
Jenkins (CI Pipeline)
    |
    | [Kaniko Build & Push]
    v
Docker Hub (Container Registry)
    |
    | [Update Manifests]
    v
GitHub (k8s-manifest/)
    |
    | [GitOps Sync]
    v
ArgoCD (CD Controller)
    |
    | [Auto Sync & Self-Heal]
    v
Kubernetes Cluster
```

## Technology Stack

### Application Layer
- **Frontend**: React 18, Vite 5, Vitest
- **User Service**: Spring Boot, Java 17, Maven
- **Note Service**: Spring Boot, Java 17, Maven
- **Database**: PostgreSQL 14

### Infrastructure & DevOps
- **Container Runtime**: Docker
- **Orchestration**: Kubernetes 1.20+
- **CI/CD**: Jenkins, Kaniko
- **GitOps**: ArgoCD
- **Registry**: Docker Hub
- **Version Control**: Git, GitHub

