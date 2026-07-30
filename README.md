# Learn Grafana + Prometheus (Podman)

Local learning stack:

| Service    | URL                    | Notes                          |
|------------|------------------------|--------------------------------|
| Grafana    | http://localhost:3000  | login `admin` / `admin`        |
| Prometheus | http://localhost:9090  | Targets, Graph, Status         |
| Demo app   | http://localhost:9898  | Generates `/metrics` traffic   |

Prometheus is pre-wired as Grafana’s default datasource. A starter dashboard is under **Dashboards → Learning**.

## Start / stop

```bash
cd prometheus-grafana-get-started

# Podman machine must be running (macOS)
podman machine start

podman compose up -d
podman compose ps
podman compose logs -f

podman compose down          # stop
podman compose down -v       # stop + delete volumes
```

## Things to try

1. **Prometheus UI** → Status → Targets — confirm `prometheus` and `demo` are UP.
2. Hit the demo a few times, then query in Prometheus:
   ```promql
   rate(http_requests_total{job="demo"}[1m])
   up
   process_resident_memory_bytes
   ```
3. Generate traffic:
   ```bash
   for i in $(seq 1 50); do curl -s http://localhost:9898/ > /dev/null; done
   ```
4. **Grafana** → Explore → Prometheus → same PromQL → visualize.
5. Open the provisioned dashboard **Learning — Prometheus + Demo**.

## Layout

```
learn-observability/
├── compose.yml
├── prometheus/prometheus.yml
└── grafana/
    ├── dashboards/learning.json
    └── provisioning/
        ├── datasources/prometheus.yml
        └── dashboards/dashboards.yml
```
