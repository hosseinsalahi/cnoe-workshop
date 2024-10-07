# CNOE Workshop: Introduction to CNOE

Welcome to the **CNOE (Cloud-Native Orchestration Engine)** Workshop! In this section, we will provide an introduction to CNOE (pronounced Kuh.no), its purpose, use cases, and the prerequisites required for this workshop.

## Overview of CNOE

CNOE, short for Cloud-Native Orchestration Engine, is a platform designed to simplify and automate the management of cloud-native applications and infrastructure. It provides developers and DevOps teams with a consistent interface to deploy, monitor, and manage applications in Kubernetes clusters, as well as provision and orchestrate cloud resources.

### Key Features:
- **Unified Developer Portal**: CNOE integrates tools like CI/CD, infrastructure provisioning, and application monitoring in a single interface.
- **Extensibility**: Highly customizable with support for various plugins and integrations like Backstage, Argo Workflows, and Crossplane.
- **Automation**: Streamlines deployment pipelines and automates cloud resource provisioning, reducing operational complexity.

## Use Cases and Benefits

CNOE enables teams to:
- Build and deploy internal developer platforms tailored to specific organizational needs.
- Manage application life-cycles across multiple Kubernetes environments.
- Automate the provisioning of cloud resources such as databases, storage, and networking.
- Centralize control of workflows, CI/CD pipelines, and infrastructure as code (IaC) through a single platform.

**Common Use Cases:**
- Streamlining DevOps processes by centralizing tools and workflows.
- Reducing the operational burden of managing cloud-native infrastructure.
- Enabling faster iteration cycles by simplifying deployment processes.

## Prerequisites for This Workshop

To get the most out of this workshop, you’ll need:
- **Basic Knowledge of Kubernetes**: Familiarity with Kubernetes concepts such as pods, services, and deployments is required.
- **Container Runtime**: Ensure Docker/Podman is installed and running on your system.
- **Access to a Kubernetes Cluster**: You can use a local cluster (like Minikube) or a cloud-based Kubernetes environment (such as EKS, GKE, or AKS).
- **Git**: To clone the workshop repository and follow along with the examples.

Make sure your environment is ready before proceeding to the next section.

---

### Next Step

Once your environment is set up and you’ve reviewed this introduction, proceed to the next section: [Setting Up Your Development Environment](./02-setup-environment.md).

---

## Helpful Links
- [CNOE GitHub Repository](https://github.com/cnoe-io)
- [CNOE Homepage](https://cnoe.io/docs/category/getting-started)
- [Kubernetes Documentation](https://kubernetes.io/docs)