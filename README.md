# Demo microservice deployment in Kubernetes cluster through Kustomize

Kustomize: kubernetes native configuration management

## Pre-requisits

Install kustomize:

`$ go get sigs.k8s.io/kustomize`

## General

File system approach :

```
├── someapp
    ├── base
    │   ├── deployment.yaml
    │   ├── kustomization.yaml
    │   └── service.yaml
    └── overlays
        ├── dev
        │   ├── deployment.yaml
        │   └── kustomization.yaml
        ├── preprod
        │   ├── deployment.yaml
        │   └── kustomization.yaml
        └── production
            ├── deployment.yaml
            └── kustomization.yaml
```

In base folder we define common resources.
In overlays folders we define environment specific patches. Into the environment folder we can have for example a yaml file called replica_count.yaml or cpu_count.yaml instead of generic name like deployment.yaml.

## Deployment

Dry-run deployment in dev environment:

`$ kustomize build deploy/demo/overlays/dev`

Create/deploy the resources for the dev environment variant:

`$ kustomize build deploy/demo/overlays/dev | kubectl apply -f -`

If we want to deploy and set the tag used on an image to match an environment variable, run:

`$ kustomize edit set image demo:123456`

(*demo* is our image name in this example)

and then our previous command which build and apply:

`$ kustomize build deploy/demo/overlays/dev | kubectl apply -f -`

*Kustomize edit* command will edit the *kustomization.yaml* file in *deploy/k8s/* folder:

```
...
images:
- name: demo
  newTag: "123456"
```