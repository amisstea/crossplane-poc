# crossplane-poc
Proof of Concepts with Crossplane

## Installation

Install crossplane using the official Helm chart.

```shell
helm repo add crossplane-stable https://charts.crossplane.io/stable

helm repo update

helm install crossplane \                 
--namespace crossplane-system \
--create-namespace crossplane-stable/crossplane \
-f settings.yaml
```

## Setup

1. First place a valid kubeconfig in the `eaas-cluster` secret within the `crossplane-system` namespace. Use the `kubeconfig` key.

2. Create the admin-controlled crossplane resources.

    **Note:** Some resources may not apply successfully the first time. If this happens give
    the cluster some time to reconcile dependencies and try again.

    ```shell
    kubectl apply -f 000-namespaces.yaml
    kubectl apply -f 001-providers.yaml
    kubectl apply -f 002-provider-configs.yaml
    kubectl apply -f 003-functions.yaml
    kubectl apply -f 004-xrds.yaml
    kubectl apply -f 005-xrs.yaml
    ```

## Request a Namespace

Create a namespace claim.

```shell
kubectl apply -f 006-claims.yaml
```

Get the connection details to the namespace.

```shell
kubectl get secret my-claim-secret -o yaml
```

Now delete the namespace claim.

```shell
kubectl delete -f 006-claims.yaml
```
