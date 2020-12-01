**Overview**

Handling the logs of your services is critical for validating health and improvements.  It can be tricky and it can be resource intensive.  efk-stack-for-traefik tries to package a set of pieces in a certain configuration that will be easy to launch without significant configuration, performant on minimal machine configurations, and capable of capturing and analyzing the service history that you depend on.  

****Disclaimer****

This configuration is for small-scale, experimental setups, and has employed no explicit service resiliency or security hardening.

***Major ELements***

- Elasticsearch: standard-bearer of the log analysis market
- Fluentd: newcomer but powerful contender in the log collection sphere, especially on kubernetes
- Kibana: visualization of elasticsearch indexing efforts
- Docker labels on the `elasticsearch` service to enable traefik integration

***Prerequisites***

- Docker, Docker-Compose
- Traefik: operating as an external service-set or deployment or stack but within the same network

***Scale***

While kubernetes (the new and open way to cluster) is new and hot and enterprise, not all coding cow-pokes live in that arena.  Here we are targeting docker-compose deployments (single logical node).  Simple, straight-forward, lightly managed and lightly monitored. We are also targeting the deployment behind traefik (edge-routing, reverse-proxy), so services that will be exposed will be adorned with traefik service labels.

***Integration***

To integrate or consume the logging service conglemerate created by this deployment, you will need to add the following to your client conglomerates in your docker-compose (:

```
# above and below the triple dot (...) is for context; you will already have this in your particlar setup
services:
  client:
    image: <client conglomerate needing integrated logging>
...
    logging:
      driver: "fluentd"
      options:
        fluentd-address: fluentd:24224
        tag: client.access
...
```
