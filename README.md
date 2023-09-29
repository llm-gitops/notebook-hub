# notebook-hub

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: weave-ai-system
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
  targetNamespace: flux-system
  install:
    crds: Create
    remediation:
      retries: -1
  upgrade:
    crds: CreateReplace
    remediation:
      retries: -1
```
