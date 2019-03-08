## Prometheus

Beginning with Anchore Engine 0.2.0 the Anchore Engine includes support for exposing metrics for use by Prometheus monitoring.

To enable Prometheus support simply enable the metrics configuration option in the Anchore Engine configuration file config.yaml

```YAML
metrics:
  enabled: True
```

If metrics are enabled then a /metrics route will be exposed on all individual Anchore services that are listening on a network interface and require authentication.

Prometheus should be configured to scrape data from each Anchore Service. If Anchore has been deployed in a scale-out configuration where multiple services are running, for example multiple analyzer services then a target for each Anchore service is defined.

Anchore Engine is comprised of 6 services outlined in the following table:

| Service | Default Port | Description |
| :------ | :----------- | :---------- |
| apiext | 8228 | External API service |
| kubernetes webhook | 8338 | External Kubernetes webhook |
| catalog | 8082 | Internal catalog service |
| simplequeue | 8083 | Internal queuing service |
| analyzer | 8084 | Internal image analyzer service |
| policy engine | 8087 | Internal policy engine service |

Only the external api service and kubernetes webhook service are typically enabled for external access. All other services are used only by the Anchore Engine.

The Prometheus should have network access to each service to be scraped and the prometheus service should be configured with credentials to access the engine.

In the following example the Anchore engine is running on a host called anchore-engine using the default ports, with username *admin* and password *foobar*.

Example prometheus.yml

```YAML
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s
alerting:
  alertmanagers:
  - static_configs:
    - targets: []
    scheme: http
    timeout: 10s
scrape_configs:
- job_name: anchore-api
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - anchore-engine:8228
  basic_auth:
    username: admin
    password: foobar

- job_name: anchore-catalog
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - anchore-engine:8082
  basic_auth:
    username: admin
    password: foobar

- job_name: anchore-simplequeue
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - anchore-engine:8083
  basic_auth:
    username: admin
    password: foobar

- job_name: anchore-analyzer
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - anchore-engine:8084
  basic_auth:
    username: admin
    password: foobar

- job_name: anchore-k8s
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - anchore-engine:8338
  basic_auth:
    username: admin
    password: foobar

- job_name: anchore-policy-engine
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - anchore-engine:8087
  basic_auth:
    username: admin
    password: foobar
```



