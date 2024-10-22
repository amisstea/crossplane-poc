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

2. Modify the annotation in `002-provider-configs.yaml` so that it points to the kubernetes
API server referenced in the prior kubeconfig.

2. Create the admin-controlled crossplane resources.

    **Note:** Some resources may not apply successfully the first time. If this happens give
    the cluster some time to reconcile dependencies and try again.

    ```shell
    kubectl apply \
      -f 000-namespaces.yaml \
      -f 001-providers.yaml \
      -f 002-provider-configs.yaml \
      -f 003-functions.yaml \
      -f 004-xrds.yaml \
      -f 005-compositions.yaml
    ```

## Request a Namespace

Create a claim for a namespace.

```shell
./namespace-demo.sh
```

In addition to outputting the generated namespace, serviceaccount, secrets and rolebindings,
this script will write a `kubeconfig` file to the working directory.
Use the config to access the namespace. For example:

```
KUBECONFIG=kubeconfig kubectl get serviceaccounts
```

Delete all EaaS namespaces when you're ready.

```shell
kubectl delete --all namespaces.eaas.konflux-ci.dev
```

Notice the connection credentials have been deleted as well.

```shell
kubectl get secrets
```
