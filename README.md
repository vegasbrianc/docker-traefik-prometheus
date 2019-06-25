# Monitor Traefik with Prometheus

This Repo helps you get started with monitoring [Traefik](https://traefik.io/) the amazing Reverse Proxy + so much more. Along with Traefik we will provision Prometheus for Time Series Data collection and Grafana for Visualization. Traefik will also act as a proxy in front of Promethues and Grafana while Prometheus monitors Traefik the other way. Cool, huh?

Before we can begin ensure you have Docker installed with Docker Swarm enabled. If you are using Docker for Desktop Mac or Windows you already have Swarm enabled. For all others please follow the [Swarm setup guide](https://docs.docker.com/engine/swarm/swarm-mode/).

# Goals of the Traefik Monitoring Repo:

* Provision a Traefik Stack with Prometheus metrics enabled
* Deploy Prometheus & Grafana
* Verify Traefik Metrics
* Configure Dashboards in Grafana for Traefik

## Deploy Traefik, Prometheus, and Grafana
In this section we will prepare and deploy our Traefik Reverse Proxy and our monitoring stack. 

First, we need to clone this Repo to your Docker Swarm. Ensure you are performing this on your Manager node. If you only have one node then this is also the manager.

    git clone https://github.com/vegasbrianc/docker-traefik-prometheus.git

Next, lets review what the stack is doing before we deploy it. With your favorite editor (VI of course). Open the `docker-compose.yml` file.

    cd docker-traefik-prometheus
    vi docker-compose.yml

## Check the Metrics

## Login to Grafana and Visualize Metrics
