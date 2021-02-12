# kube-system-apps

Collection of manifests to bootstrap a Kubernetes cluster with a set of baseline apps that provide AAA, GitOps, and security.

## Getting Started

This is designed to be a test/demo envrionment mostly for me to learn Kubernetes concepts, but 

### Prerequisites

There is a wrapper script to create a single node cluster using Kubeadm. That script does NOT install the KubeAdm [prerequisite components](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)  If you don't use this, you'll need access to a Kubernetes cluster.

You will also need access to both [Kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/) and [Helm v3](https://helm.sh/docs/intro/install/).

Modify the values in both /kubeadm/kubeadm-config.yaml and /kubeadm/calico-config.yaml to fit your environment. Then:
```
bash ./kubeadm/cluster-wrapper.sh create
```

### Installing

Almost all of the applications are managed with GitOps in ArgoCD. There is a chicken and egg problem so ArgoCD and its associated deployments need to be installed manually. 

Install Hashicorp Vault

```
cd sealedsecrets
kustomize build . | kubectl create -f-
```

Install ArgoCD

```
kustomize build ./argocd | kubectl create -f-
```
