# GitOps Repository
GitOps repository managing Kubernetes manifests, Helm charts, and ArgoCD application deployments.

The bank application serves requests directly:

```text
Route53 -> ALB (Gateway / HTTPRoute) -> mak-app-active:80 -> frontend:8080
  -> backend ClusterIP Services:8080 -> PostgreSQL
```

Redis waiting rooms, worker sidecars, and Redis queue autoscalers are removed.
ALB target groups check `/ready` on port 8080. Frontend predictive scaling remains
disabled by default.

Rebuild and publish the frontend image from the App repository, then sync the
frontend and backend Argo CD Applications from the `test_alb` branch. The frontend
chart sets `ENABLE_WAITING_ROOM=false` to bypass the waiting room even while an
older image is still running. Terraform no longer injects Redis connection values.

