# idpbuilder: Understanding How it Works
This section provides an overview of how **idpbuilder** works within the CNOE framework. idpbuilder is a core tool that helps automate the deployment of internal platform components and resources, allowing for easy and repeatable setups.

## What is idpbuilder?

**idpbuilder** is a deployment automation tool integrated with CNOE, designed to simplify the deployment process for cloud-native applications and internal platforms. It works by using declarative infrastructure-as-code templates to define, build, and deploy various services, components, and resources within a Kubernetes environment.

### Key Features:
- **Declarative Infrastructure Management**: Define your infrastructure and services in code, ensuring consistency and repeatability across environments.
- **Automation**: Automates the deployment of platform components, eliminating manual steps and reducing the likelihood of human error.
- **Extensibility**: Supports a variety of plugins and can integrate with other tools like Helm, Terraform, and Kubernetes manifests.
- **Environment-Specific Configuration**: Provides configuration management for different environments (e.g., development, staging, production) through templated configurations.

## How idpbuilder Works
idpbuilder operates by parsing and applying configuration templates to create the necessary infrastructure and deploy applications within your Kubernetes clusters. The typical workflow involves the following steps:

### 1. **Configuration Definition**
idpbuilder uses a configuration file (usually in YAML format) where you define your application components, services, and required infrastructure. The configuration may include:
- Kubernetes objects (namespace, deployments, services, etc.)
- Cloud resources (databases, storage)
- Networking components (ingress controllers, load balancers)

### 2. Template Parsing
Once the configuration file is defined, idpbuilder parses it and converts the declarative instructions into the appropriate Kubernetes manifests or cloud resource templates. It manages various platform components by abstracting away the underlying complexity. To this end, CNOE has its own K8s `CustomResources`:

```bash
kubectl get crds | grep cnoe
custompackages.idpbuilder.cnoe.io    2024-10-07T15:18:50Z
gitrepositories.idpbuilder.cnoe.io   2024-10-07T15:18:50Z
localbuilds.idpbuilder.cnoe.io       2024-10-07T15:18:51Z
```
Let's take a look at one of this CRs: 

```bash
kubectl get gitrepositories.idpbuilder.cnoe.io -n idpbuilder-localdev
NAME     AGE
argocd   18m
gitea    18m
nginx    18m
```

```bash
kubectl get gitrepositories.idpbuilder.cnoe.io -n idpbuilder-localdev nginx -o yaml
apiVersion: idpbuilder.cnoe.io/v1alpha1
kind: GitRepository
metadata:
  name: nginx
  namespace: idpbuilder-localdev
spec:
  provider:
    gitURL: https://gitea.cnoe.localtest.me:8443
    internalGitURL: https://gitea.cnoe.localtest.me:8443
    name: gitea
    organizationName: giteaAdmin
  secretRef:
    name: gitea-credential
    namespace: gitea
  source:
    embeddedAppName: nginx
    path: ""
    remoteRepository:
      cloneSubmodules: false
      path: ""
      ref: ""
      url: ""
    type: embedded
```

### 3. Environment-Specific Overrides
idpbuilder supports environment-specific configurations. You can define common configurations that apply to all environments, and override specific parameters for each environment (e.g., staging, production). For instance, you might want to scale up the number of replicas for production while keeping them minimal in development.

### 4. Automated Deployment
Once the configuration is parsed, idpbuilder leverages Kubernetes operator such as ArgoCD to apply the configuration, deploy services, and uses Terraform or Crossplane to build infrastructure in your target environment. This automation eliminates the need for manual application and infrastructure setup.

When idpbuilder creates an environment for you, it performs the following tasks.

- Create a local cluster if one does not exist yet.
- Create a [self-signed certificate](https://cnoe.io/docs/reference-implementation/installations/idpbuilder/how-it-works#self-signed-certificate), then set it as the default TLS certificate for ingress-nginx.
- [Configure CoreDNS](https://cnoe.io/docs/reference-implementation/installations/idpbuilder/how-it-works#in-cluster-dns-configuration) to ensure names are resolved correctly.
- Install [Core Packages](https://cnoe.io/docs/reference-implementation/installations/idpbuilder/how-it-works#core-packages), then hands control over to ArgoCD.