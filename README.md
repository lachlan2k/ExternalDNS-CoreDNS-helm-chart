# ExternalDNS-CoreDNS-helm-chart

A Helm chart to deploy ExternalDNS & CoreDNS, with an etcd backend store.

### Tl;dr

```
helm repo add externaldns-coredns https://lachlan2k.github.io/ExternalDNS-CoreDNS-helm-chart/
helm install --create-namespace --namespace=ext-dns ext-dns externaldns-coredns/externaldns-coredns

# Will take 1-2 mins to come up

# Find IP of new DNS server
kubectl get -n ext-dns svc ext-dns-coredns
```

### Tl;dr - ArgoCD

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: ext-dns
  namespace: argocd

spec:
  project: default

  destination:
    name: in-cluster
    namespace: ext-dns
    
  source:
    chart: externaldns-coredns
    repoURL: https://lachlan2k.github.io/ExternalDNS-CoreDNS-helm-chart/
    targetRevision: 0.1.1
    helm:
      releaseName: externaldns-coredns
                      
  syncPolicy:
    automated: {}
    syncOptions:
    - CreateNamespace=true
```