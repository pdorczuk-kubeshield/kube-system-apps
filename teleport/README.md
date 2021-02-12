# teleport

Files used to install and set up Teleport for secure access to both the host running kubeadm and the Kubernetes cluster used in this demo.

## Getting Started

This is designed to be a test/demo envrionment mostly for me to learn Kubernetes concepts, but 

### Prerequisites

There is a wrapper script to create a single node cluster using Kubeadm. That script does NOT install the KubeAdm [prerequisite components](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)  If you don't use this, you'll need access to a Kubernetes cluster.

You will also need access to both [Kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/) and [Helm v3](https://helm.sh/docs/intro/install/).

You'll need a valid certificate to attach to Teleport. In my example I am using CertBot in a Docker container using the Cloudflare DNS challenge.
```
docker run -it --rm --name certbot \
    -v "/etc/letsencrypt:/etc/letsencrypt" \
    -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
    certbot/dns-cloudflare certonly \
    --dns-cloudflare \
    --dns-cloudflare-credentials /etc/letsencrypt/cloudflare-creds \
    -d teleport.kubeshield.com
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
