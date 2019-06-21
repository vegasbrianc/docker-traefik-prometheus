# Monitor Traefik with Prometheus

This Repo helps you get started with monitoring [Traefik](https://traefik.io/) the amazing Reverse Proxy + so much more. Along with Traefik we will provision Prometheus for Time Series Data collection and Grafana for Visualization. Traefik will also act as a proxy in front of Promethues and Grafana while Prometheus montiors Traefik the other way.

Before you can begin ensure you have Docker installed with Docker Swarm enabled. If you are using Docker for Desktop Mac or Windows you already have Swarm enabled. For all others please follow the [Swarm setup guide](https://docs.docker.com/engine/swarm/swarm-mode/)

Goals of this Repo:

* Provision a Traefik Stack with Prometheus metrics enabled
* Deploy Prometheus & Grafana
* Configure Dashboards in Grafana for Traefik
