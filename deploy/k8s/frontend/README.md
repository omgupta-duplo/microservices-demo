# frontend – baseline Kubernetes manifests

Generated baseline for `src/frontend` (Go HTTP server). Apply with `kubectl apply -k deploy/k8s/frontend`.
Not deployed anywhere by this PR; no namespace is set.

| Setting | Value | Source |
|---|---|---|
| Container port | 8080 | Detected: `EXPOSE 8080` in Dockerfile, `port = "8080"` in main.go |
| Health check | `/_healthz` | Detected: route in main.go |
| Required env vars | 8 `*_SERVICE_ADDR` | Detected: `mustMapEnv` in main.go; values from `kubernetes-manifests/frontend.yaml` |
| Resources | 100m/200m CPU, 64Mi/128Mi mem | Reused from existing manifest |
| Replicas | 2 | Assumption (basic availability) |
| Exposure | Public, Ingress on `/`, any host | Assumption (frontend is the only user-facing service) |
| Image | `frontend:latest` | **Placeholder** – replace with your registry image |
| Secret | `frontend-secrets`, no keys | Placeholder – app reads no secrets today |

## Before use
- Set the real image.
- Set `ingressClassName`, host and TLS for your cluster.
- The upstream services in the ConfigMap (productcatalogservice, cartservice, …) must also be deployed; they are not included here.
- `kubernetes-manifests/frontend.yaml` defines resources with the same names (`frontend` Deployment/Service). Apply one set or the other in a namespace, not both.
