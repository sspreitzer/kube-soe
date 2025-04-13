# kube-soe - The Kubernetes Standard Operating Environment

## DISCLAIMER

This project and code is currently in a development state. The information in the documentation and the codebase might not up-to-date. Insofar do not expect reliability or stability. But feel invited to contribute and enhance.

## What is kube-soe?

`kube-soe`is an opensource project that enables Kubernetes platform engineers or administrators to maintain a fleet of Kubernetes clusters via GitOps principles.

## Why does kube-soe exist?

`kube-soe` was born out of the idea to create a Kubernetes distribution and vendor independent way to maintain Kubernetes clusters that is not using the operator pattern for itself. The project shall suffice multiple vendor scenarios at once while having a relatively low learning curve.

## Principles

### External Standards

Where applicable and possible we stick to standards. List of external standards we follow:

* [Conventional Commits](https://www.conventionalcommits.org)
* [Semantic Versioning](https://semver.org)
  * In its simple form of MAJOR.MINOR.PATCH or X.Y.Z

## Prerequisites

On the server side you will need to create a fork of the `kube-soe` main repository and regularly merge the upstream updates into your fork and stage them into your branches. On the client side various tools will come in handy.

Server:

* A *private* Git repository (eg. github.com or gitlab.com)

CLIs:

* `tofu` (optional, recommended)
* `kind` (optional, recommended)
* `git`
* `kubectl`
* `helm`
* `argocd`

## Preparation and Planning



## Installation

The installation of `kube-soe` is divided in two phases.

### Phase 1 - The Kubernetes Cluster(s)

Phase 1 is the installation of a Kubernetes cluster itself. This can be done via Infrastructure as Code (IaC) tools such as [OpenTofu](https://opentofu.org), [Terraform](https://www.terraform.io), [Ansible](https://docs.ansible.com/ansible/latest/index.html), [Crossplane](https://www.crossplane.io) or many more.

For example:

```shell
$ kind create cluster
```

or

```shell
$ tofu init
$ tofu plan
$ tofu apply -auto-approve
```

### Phase 2 - Installing The SOE

Phase 2 is the installation of the soe ArgoCD and the soe Helm Chart `kube-soe`. First we install a fresh ArgoCD instance with values that are important for the later soe Helm Chart. And afterwards we install the soe Helm Chart that will pick up the management of the soe ArgoCD and Kubernetes Cluster.

Make sure your current [kubectl kubeconfig context](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-use-context-em-) points to the target kubernetes cluster.

For example:

```shell
$ helm install soe-argo-cd argo/argo-cd --create-namespace --namespace soe-argo-cd --set dex.enabled=false
$ argocd app create soe \
    --repo https://git.yourserver.com/kube-soe.git \
    --revision yourBranchOrTagOrRef \
    --path charts/kube-soe \
    --dest-namespace soe-argo-cd \
    --dest-server https://kubernetes.default.svc \
    --helm-set clusterName=kind
```

## Configuration


## Maintenance


## Contributing

