# notebook-hub

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: weave-ai-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  labels:
    toolkit.fluxcd.io/tenant: hub
  name: weave-ai-reconciler
  namespace: weave-ai-system
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: hub
  namespace: weave-ai-system
---
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: jupyterhub
  namespace: weave-ai-system
spec:
  interval: 1h0s
  url: https://hub.jupyter.org/helm-chart/
---
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: jupyterhub
  namespace: weave-ai-system
spec:
  chart:
    spec:
      chart: jupyterhub
      sourceRef:
        kind: HelmRepository
        name: jupyterhub
      version: '>=3.0.3'
  interval: 1h0s
  releaseName: jupyterhub
  targetNamespace: weave-ai-system
  install:
    crds: Create
    remediation:
      retries: -1
  upgrade:
    crds: CreateReplace
    remediation:
      retries: -1
```
