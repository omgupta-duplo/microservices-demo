# frontend: baseline Kubernetes manifests

Generated baseline for `src/frontend` (Go HTTP server, gorilla/mux).
This PR does not deploy anything and sets no namespace.

```sh
kubectl kustomize deploy/k8s/frontend      # render
kubectl apply -k deploy/k8s/frontend -n <ns>
```

| Setting | Value | Source |
|---|---|---|
| Container port | 8080 | Detected: `EXPOSE 8080` in Dockerfile, `port = "8080"` in main.go |
| Health check | `/_healthz` | Detected: route in main.go |
| Required env vars | 8 `*_SERVICE_ADDR` | Detected: `mustMapEnv` in main.go; values from `kubernetes-manifests/frontend.yaml` |
| Resources | CPU 100m / 200m, memory 64Mi / 128Mi | Reused from existing `kubernetes-manifests/frontend.yaml` |
| Replicas | 2 | Default (basic availability, zero-downtime rollout) |
| Exposure | Public: Ingress on `/`, any host | Default (frontend is the only user-facing service) |
| Image | `.../frontend:v0.10.7` | Detected: `release/kubernetes-manifests.yaml` |
| Secret | `frontend-secrets`, no keys | Placeholder: the app reads no secrets today |

## Before using in a real environment
- Replace the image with one from your own registry/CI if needed.
- Set `ingressClassName`, host and TLS for your cluster.
- The frontend panics at startup unless the upstream gRPC services (productcatalogservice, cartservice, ...) are reachable by name. They are **not** included here.
- `kubernetes-manifests/frontend.yaml` defines resources with the same names (`frontend` Deployment and Service). Use one set or the other in a namespace, not both.
