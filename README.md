# home-tls-gateway
This is my home tls-gateway that provides tls encryption for all my local services using DNS-01 challenge on a microk8s/cert-manager/GitOsp micro cluster

# Reason for this setup
I have 3 clusters or more at times where I need TLS encryption on apps/services which I tend to trash and rebuild.
As I do not feel like I want to rebuild the whole certificate handling setup and I have the resources, I dedicated a single VM to handle wildcard DNS challenges for me which is always-on. A low footprint declarative gateway.
If I need to modify a service or add one, I just use the template, push to git and wait a minute or 2 to reconcile the new setup.
if I need to delete one, then the same principle applies.

# On the VM
- microk8s enable cert-manager (needed for DNS-01 Challenge)
- FluxCD shell utility
- microk8s enable metrics-server

```bash
flux install \
  --components=source-controller,kustomize-controller,helm-controller \
  --components-extra=source-watcher \
  --namespace=flux-system
```

# Check usage:
```bash
k top pods -n flux-system
NAME                                    CPU(cores)   MEMORY(bytes)   
helm-controller-8489bc77fb-xxg45        1m           25Mi            
kustomize-controller-69cd78bc9b-8vfmk   1m           14Mi            
source-controller-7964fb6b7d-5jldl      1m           51Mi            
source-watcher-df5956f4b-4s528          2m           11Mi 
```

# Updates

# Update 1
I knew ArgoCD was heavy on resources, but it trashed a MicroK8S cluster with 4cpu/4Gib so the most obvious alternative is FluxCD or Kustomize

# Update 2
Flux seems to be more reliable and stable in idle mode. 

