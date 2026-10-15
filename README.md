# Kubernetes

A collection of Kubernetes manifests for a personal k3s homelab cluster, covering core
object types: Deployments, StatefulSets, Services, Pods, ConfigMaps, and NFS-backed
PersistentVolumes/Claims.

## Layout

| File | Kind(s) | Purpose |
| --- | --- | --- |
| `webstorage.yaml` | PersistentVolume | NFS-backed volume (`10.0.1.16:/srv/nfs/share/webroot`) used by `web.yaml`'s StatefulSet |
| `web.yaml` | Service, StatefulSet | Stateful Nginx web server backed by the `www` PersistentVolume |
| `storage-kevflix.yaml` | PersistentVolume, PersistentVolumeClaim | NFS-backed storage for the `kevflix` demo app |
| `pato-kevflix.yaml` / `pato.yaml` | Deployment, Service | Demo Nginx app serving content from the `kevflix` storage claim |
| `webserver.yaml` | Pod | Standalone Nginx pod |
| `ws.yaml` / `ws-svc.yaml` | Deployment, Service | Nginx deployment exposed via a NodePort service |
| `nginx-pod.yaml` | Pod | Minimal single-container Nginx pod |
| `customer1-configmap.yaml` | ConfigMap | Sample ConfigMap used to practice injecting config into pods |
| `helm/`, `rbac/` | — | Reserved for future Helm chart and RBAC examples |

Related repos:
- [`ansible`](https://github.com/Kevin-Smith-112/ansible) — playbooks that provision and
  configure the k3s nodes these manifests are applied to.
- [`homelab-infrastructure`](https://github.com/Kevin-Smith-112/homelab-infrastructure) —
  a separate AWS/Terraform lab environment (unrelated infrastructure, same homelab habit).

## Usage

```bash
kubectl apply -f webstorage.yaml
kubectl apply -f web.yaml
```

Manifests that depend on the NFS server (`10.0.1.16`) assume that host is reachable from
every k3s node; see the `ansible` repo for how the NFS share and nodes are provisioned.
