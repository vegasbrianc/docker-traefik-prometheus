# Monitor Traefik with Prometheus

This Repo helps you get started with monitoring [Traefik](https://traefik.io/) the amazing Reverse Proxy + so much more. Along with Traefik we will provision Prometheus for Time Series Data collection and Grafana for Visualization. Traefik will also act as a proxy in front of Promethues and Grafana while Prometheus monitors Traefik the other way. Cool, huh?

Before we can begin ensure you have Docker installed with Docker Swarm enabled. If you are using Docker for Desktop Mac or Windows you already have Swarm enabled. For all others please follow the [Swarm setup guide](https://docs.docker.com/engine/swarm/swarm-mode/).

# Goals of the Traefik Monitoring Repo:

* Provision a Traefik Stack with Prometheus metrics enabled
* Deploy Prometheus & Grafana
* Verify Traefik Metrics
* Configure Dashboards in Grafana for Traefik

## Review the Traefik Monitoring Stack Deployment
In this section we will prepare and deploy our Traefik Reverse Proxy and our monitoring stack. 

First, we need to clone this Repo to your Docker Swarm. Ensure you are performing this on your Manager node. If you only have one node then this is also the manager.

    git clone https://github.com/vegasbrianc/docker-traefik-prometheus.git

Next, lets review what the stack is doing before we deploy it. With your favorite editor (vim of course). Open the `docker-compose.yml` file.

    cd docker-traefik-prometheus
    vi docker-compose.yml

The Traefik metrics are enabled by the command we pass to the Traefik container. The `--metrics` flag enables metrics and `--metrics.prometheus.buckets=0.1,0.3,1.2,5.0` defines the Prometheus bucket value (typically in seconds). Prometheus monitors for all values and stores the metric in the appropriate bucket.

    version: '3.7'

    services:
    traefik:
        image: traefik:v1.7-alpine
        command:
        - "--logLevel=DEBUG"
        - "--api"
        - "--metrics"
        - "--metrics.prometheus.buckets=0.1,0.3,1.2,5.0"
        - "--docker"
        - "--docker.swarmMode"
        - "--docker.domain=docker.localhost"
        - "--docker.watch"

Grafana and Prometheus are being deployed by Docker Swarm and the networking is managed by Traefik. We use labels for the services deployed to inform Traefik how to setup the frontend and backend for each service.

**Grafana Deployment**

    deploy:
     labels:
      - "traefik.port=3000"
      - "traefik.docker.network=inbound"
      -  "traefik.frontend.rule=Host:grafana.localhost"

**Prometheus Deployment**
    deploy:
      labels:
       - "traefik.frontend.rule=Host:prometheus.localhost"
       - "traefik.port=9090"
       - "traefik.docker.network=inbound"

Prometheus is also configured to monitor Traefik. This is configured in [Prometheus.yml](https://github.com/vegasbrianc/docker-traefik-prometheus/blob/master/prometheus/prometheus.yml#L40) which enables Prometheus to auto-discover Traefik inside of Docker Swarm. Prometheus is watching for the Service Task `tasks.traefik` on port 8080. Once the service is online metrics will begin flowing to Prometheus.

## Deploy Traefik, Prometheus, and Grafana
OK, we now know where everything is configured inside of the stack. The moment of truth `DEPLOY`

In the `docker-traefik-prometheus`directory run the following:

    docker stack deploy -c docker-compose.yml traefik

Verify all the services have been provisioned. The Replica count for each service should be 1/1 
**Note this can take a couple minutes**

    docker service ls
    
## Check the Metrics
Once all the services are up we can open the Traefik Dashboard

    http://localhost:8080

The dashboard should show us our frontend and backends configured for both Grafana and Prometheus.

Take a look at the Traefik metrics

    http://localhost:8080/metrics


## Login to Grafana and Visualize Metrics
