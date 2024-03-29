# Enforce Automated 'k8s cluster Compliance and Security using kyverno policy generator and argocd

Project descriptio:  As application workloads share cluster resources, it is important to limit resources requested and consumed by each Pod. It is recommended to require resource requests and limits per Pod, especially for memory and CPU. If a Namespace level request or limit is specified, defaults will automatically be applied to each Pod based on the LimitRange configuration. This policy validates that all containers have something specified for memory and CPU requests and memory limits.
In this project Kyverno controller will enforce to follow the organization's governence and compliance on your kubernetes cluster.
A DevOps engineer writes a simple kyverno policy custom resource in the form of YAML scripts which contains rules/compliance defined by the organization about kubernetes cluster's resources. And commit it to a Github repository. Argo CD which is pre configured with `auto-sync` to watch for resources in the git repo, deploys the Kyverno Policies on to the Kubernetes cluster.
Wheneven a user tries to ctreate or update any resource in the cluster, the Kyverno dynamic admission controller will validate the user's actions with the policy and acts upon repspectively.
Whether your kubernetes cluster is on AWS, Azure, GCP or on-premises, this project will work without any additional changes.

To explain the project with examples, using this configuration you can 

1. Generate -> For example, Create a default network policy whenever a namespace is created.
2. Validate -> For example, Block users from using `latest` tag in the deployment or pod resources.
3. Mutate -> For example, Attach pod security policy for a pod that is created without any pod security policy configuration.
4. Verify Images -> For example, Verify if the Images used in the pod resources are properly signed and verified images.

## High Level Design

![Kyverno Hight Level Design](https://github.com/venugopalreddy1322/project-diagrams/blob/main/Kyverno_project.png
)

## Installation (Follwed official documentation)

To setup this project you need to install Argo CD controller and Kyverno controller, Assuming you have Kubernetes installed.

Installation of both Kyverno and Argo CD are pretty straight forward as both of them support Helm charts and also provide a consolidated installation yaml files. 

### Kyverno

There are two easy ways to install kyverno:

1. Using Helm
2. Using the kubernetes manifest files

### Using Helm 

Add helm repo for kyverno 

```
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
```

Install kyverno in High Availability mode

```
 helm install kyverno kyverno/kyverno -n kyverno --create-namespace --set replicaCount=3
```

(or)

Install kyverno in Standalone mode

```
helm install kyverno kyverno/kyverno -n kyverno --create-namespace --set replicaCount=1
```

Install a specific version of kyverno

```
helm search repo kyverno -l | head -n 10
```

```
helm install kyverno kyverno/kyverno -n kyverno --create-namespace --version 2.6.5
```

### Using Kubernetes manifest yaml files

```
kubectl create -f https://github.com/kyverno/kyverno/releases/download/v1.8.5/install.yaml
```

### Argo CD

There are three ways to install Argo CD

1. `kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/master/manifests/install.yaml`

2. Helm Charts, Follow the [link](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd#installing-the-chart) 

3. Using the Argo CD Operator, Follow the [link](https://argocd-operator.readthedocs.io/en/latest/install/olm/)

## Kyverno Policies & Rules 

Kyverno is a policy engine designed for Kubernetes

A Kyverno policy is a collection of rules. Each rule consists of a match declaration, an optional exclude declaration, and one of a validate, mutate, generate, or verifyImages declaration. Each rule can contain only a single validate, mutate, generate, or verifyImages child declaration.

Image source:[link](https://kyverno.io.)
![Kyverno policy](https://github.com/venugopalreddy1322/project-diagrams/blob/main/kyvero_img.png)


Policies can be defined as cluster-wide resources (using the kind ClusterPolicy) or namespaced resources (using the kind Policy.) As expected, namespaced policies will only apply to resources within the namespace in which they are defined while cluster-wide policies are applied to matching resources across all namespaces. Otherwise, there is no difference between the two types.

Additional policy types include PolicyException and (Cluster)CleanupPolicy which are separate resources and described further in the documentation.

### Architecture

Image source:[link](https://kyverno.io.)
![Kyverno_architecture](https://github.com/venugopalreddy1322/project-diagrams/blob/main/Kyverno_architecture.png)

For more policies:
[link]()


For more policies official kyverno gitHub repo [link](https://github.com/kyverno/policies)

@Venugopal Reddy
