# HomeOps GitOps

A GitOps-driven repository for managing home Kubernetes infrastructure using ArgoCD.

## Overview

This repository contains all the configuration and manifests required to manage my homelab Kubernetes cluster. Everything is defined as code and reconciled automatically via [ArgoCD](https://argoproj.github.io/cd/).

## Repository Structure

```
homeops-gitops/
├── configs/
│   ├── argocd/                  # ArgoCD installation (Kustomize)
│   │   └── overlays/            # ArgoCD config overlays
│   └── argocd-apps/             # ArgoCD Application & AppProject definitions
│       ├── applications/        # App-of-apps and individual applications
│       └── appprojects/         # ArgoCD AppProject definitions
├── values/                      # Helm values files
│   └── kube-prometheus-stack.yml
├── kind-config.yml              # kind cluster configuration
└── Taskfile.yml                 # Task runner commands
```

## Getting Started

### Prerequisites

| Tool                                               | Purpose         |
|----------------------------------------------------|-----------------|
| [ArgoCD](https://argoproj.github.io/cd/)           | GitOps operator |
| [Kubectl](https://kubernetes.io/docs/tasks/tools/) | Kubernetes CLI  |
| [Task](https://taskfile.dev/)                      | Task runner     |

#### Optional

| Tool                              | Purpose                  |
|-----------------------------------|--------------------------|
| [kind](https://kind.sigs.k8s.io/) | Local Kubernetes cluster |

## Available Tasks

```
❯ task
task: [default] task --list-all
task: Available tasks for this project:
* default:                     Show this help message
* argocd:admin-password:       Get the initial admin password for ArgoCD
* argocd:bootstrap:            Bootstrap ArgoCD and the argocd-apps project
* argocd:uninstall:            Uninstall ArgoCD and the argocd-apps project
* kind:down:                   Delete kind cluster
* kind:up:                     Create kind cluster
```

## GitOps Workflow

This repository uses an **app-of-apps** pattern:

1. ArgoCD is bootstrapped manually via `task argocd:bootstrap`
2. ArgoCD then manages itself and all other applications declaratively from this repository
3. Any change merged to `main` is automatically reconciled by ArgoCD
