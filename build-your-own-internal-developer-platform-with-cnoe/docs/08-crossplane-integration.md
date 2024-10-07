# Crossplane Integration
This chapter demonstrates how to integrate cloud-native IaC tool Crossplane into our CNOE stack. By integrating Crossplane, you can provision and manage cloud resources such as databases, storage, and networking, while ensuring seamless deployment of applications within Kubernetes.

## Crossplane
Crossplane is an open-source control plane that allows teams to provision and manage cloud infrastructure using Kubernetes Custom Resource Definitions (CRDs). It enables the declarative management of cloud resources (like databases, compute instances, and networks) alongside application deployments, using Kubernetes-native tools and workflows.

With Backstage, Crossplane’s infrastructure as code (IaC) capabilities are surfaced through a user-friendly developer portal, allowing teams to request, manage, and deploy cloud resources with minimal manual effort.

## Prerequisites
Before deploying an application with cloud resources using Crossplane, ensure:

- `idpbuilder`CLI 
- `kubectl`CLI
- You have access to your local cluster
- You have access to cloud provider credentials (AWS) for provisioning resources.

### Installation 
idpbuilder is extensible to launch custom Crossplane patterns using package extensions.
Please use the below command to deploy an IDP reference implementation with an Argo application for preparing up the setup for terraform integrations:

```bash
idpbuilder create \
  --use-path-routing \
  --package https://github.com/cnoe-io/stacks//ref-implementation \
  --package https://github.com/cnoe-io/stacks//crossplane-integrations
```

Let's confirm that the Crossplane and AWS provider are installed by using the following command:

```bash
kubectl -n crossplane-system get po
NAME                                       READY   STATUS    RESTARTS   AGE
crossplane-6494656b8b-rmmvq                1/1     Running   0          22s
crossplane-rbac-manager-8458557cdd-mmdwf   1/1     Running   0          22s

kubectl get providers.pkg.crossplane.io
NAME           INSTALLED   HEALTHY   PACKAGE                                                   AGE
provider-aws   True        True      xpkg.upbound.io/crossplane-contrib/provider-aws:v0.48.0   71s
```

### Installation Review
In the previous step, we installed the following components:

- Crossplane Runtime
- AWS provider
- Basic Compositions (S3, RDS, etc)

However, this needs your AWS credentials for this to work. Update the credentials secret with your owns in the file and apply it into your cluster: 

```bash
cat <<EOF > ./provider-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: local-secret
  namespace: crossplane-system
stringData:
  creds: |
    [default]
    aws_access_key_id = replaceme
    aws_secret_access_key = replaceme
    aws_session_token = replacemeifneeded
EOF

kubectl apply -f ./provider-secret.yaml
secret/local-secret configured
```

In this part, we added crossplane to the CNOE stack and configured AWS provider and respective credentials.