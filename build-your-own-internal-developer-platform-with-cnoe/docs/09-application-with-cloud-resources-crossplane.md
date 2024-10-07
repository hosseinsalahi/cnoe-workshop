# Deploying an Application with Cloud Resources
This chapter demonstrates how to provision cloud infrastructure and deploy applications using Crossplane through Backstage. By integrating Crossplane with Backstage, you can provision and manage cloud resources such as databases, storage, and networking, while ensuring seamless deployment of applications within a single interface.

## Prerequisites
Before deploying an application with cloud resources using Crossplane, ensure:

- `Crossplane` is installed and configured in your Kubernetes cluster.
- You have configured the Crossplane with your cloud provider credentials (AWS, GCP, Azure, etc.) for provisioning resources.

### Deployment 
We will deploy an application with cloud resources using Backstage templates. In this example, we will create an application with a S3 Bucket.
Choose a template named App with S3 bucket, type demo3 as the name, then choose a region to create this bucket in.
Once you click the create button, you will have a very similar setup as the basic example. The only difference is we now have a resource for a S3 Bucket which is managed by Crossplane.

`Note:` Bucket is not created because Crossplane doesn't have necessary credentials to do so. If you'd like it to actually create a bucket, update the credentials secret file.

Log into the [Backstage](https://cnoe.localtest.me:8443/):
 
- Click on the Create... button on the left.
- Then select the `Add a Go App with AWS resources`.
- Type demo-crossplane for the `name` field.
- Update `region` filed with your AWS_REGION (e.g. eu-central-1)
- Then click create. 
- You will notice that the Backstage templating steps are very similar to the basic example in the previous part.
- Click on the Open In Catalog button to go to the entity page.


### Deployment Review
This will create a new ArgoCD application which will create K8s deployment and a S3 bucket via Crossplane. Let's take a look at them: 

```bash
kubectl -n argocd get apps | grep demo-crossplane
demo-aws                  Synced        Healthy

kubectl get deployment
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   3/3     3            3           146m

kubectl get buckets.s3.aws.crossplane.io
NAME                   READY   SYNCED   AGE
demo-aws-p7b7f-fg4l8   True    True     47m
```

In the section we used a Backstage template to create a cloud resource in AWS via Crossplane and deploy an example application on K8s. 

## Cleaning Up Resources
You can use the following command to destroy the stack:

```bash
idpbuilder delete
```