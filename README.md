# Overview

This repo provides a few examples of using [Kustomize](https://kustomize.io) with Kubernetes. [Kustomize](https://kustomize.io) is a cool tool that lets you use a base set of Kubernetes files and override (overlay) them as needed, like per enviornment. These overlays can be used to change settings like environment variables, resource limites, and more without specifying a whole new set of files per environment. A typical [Kustomize](https://kustomize.io) layout is below and note that it is not limited to the example files used (i.e. you can have secrets, clusterroles, etc):

```
base/
  kustomization.yaml
    | resources:
    | - deployment.yaml
    | - configMap.yaml
  deployment.yaml
  configMap.yaml

app1/
  kustomization.yaml
    | resources:
    | - ../base
    | patches:
    | - patch1.yaml
  patch1.yaml

app2/
  kustomization.yaml
    | resources:
    | - ../base
    | patches:
    | - patch2.yaml
  patch2.yaml
```

## Prerequisites 

The only real prerequisite is that Kubernetes is installed somewhere (I recommend [Minikube](https://minikube.sigs.k8s.io/docs/)) and that your Kubernetes context is set to use it.

## How To Deploy

Per the [Kustomize](https://kustomize.io) documentation, all you need to do to deploy these applications is run the following commands:

** Note that we use `-k` rather than `-f` to run [Kustomize](https://kustomize.io)

#### Deploy the Base Application:
```
kubectl apply -k hello-world/base
kubectl apply -k nginx/base
```

#### Deploy an Overlay:
```
kubectl apply -k hello-world/overlay/dev
kubectl apply -k hello-world/overlay/stage
kubectl apply -k hello-world/overlay/prod

kubectl apply -k nginx/base/overlay/dev
kubectl apply -k nginx/base/overlay/stage
kubectl apply -k nginx/base/overlay/prod
```
