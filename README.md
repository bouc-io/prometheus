# Prometheus Installation and Configuration

Overall Prometheus installation and configuration for all environments (i.e. local, sandbox and production).

## Prerequisites

The following configuration is based on Docker and Docker-desktop setup, leveraging its internal, local kubernetes cluster (usually named "docker-desktop").

To deploy Prometheus manually via Helm, execute following command:

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## Local

To install Prometheus locally, execute the following command:

```shell
helm upgrade --install --namespace prometheus --create-namespace prometheus prometheus-community/prometheus -f base.values.yaml -f lcl.values.yaml
```

Prior to access the Prometheus dashboard, add the following entry to the /etc/hosts file:

127.0.0.1 prometheus.docker.internal

Access Prometheus at: http://prometheus.docker.internal/

## Sandbox

The setup for Sandbox mirrors Local but uses `sbnx.values.yaml`.

## Production

(To be defined)

## FluxCD GitOps

Prometheus is delivered by FluxCD. The Flux repo reconciles manifests through environment-specific Kustomizations which reference the values files located in this directory.

- `base.values.yaml` – Common configuration logic.
- `lcl.values.yaml` – Local-specific overrides.
- `sbnx.values.yaml` – Sandbox-specific overrides.

Flux wires them via `configMapGenerator` in the cluster's config Kustomization:

```yaml
  - name: prometheus-base-values
    namespace: prometheus
    files:
      - base.values.yaml=../../components/prometheus/base.values.yaml
  - name: prometheus-level-values
    namespace: prometheus
    files:
      - lcl.values.yaml=../../components/prometheus/lcl.values.yaml
```

## References

Refer to:
- https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus

## License

[Elastic License 2.0](./LICENSE) — source-available; not OSI open source.
