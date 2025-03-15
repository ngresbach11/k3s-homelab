# k3s-homelab

Greetings! My name is Nate. Over the past few years, I've been running a homelab to explore new technologies and methodologies, and provide myself an enviornment to play around in that can allow me to learn effectively. My homelab has gone through several iterations- what started as a simple old gaming computer running a NAS and a few VMs became a multi-node ESXi cluster which eventually became proxmox, and now I am exploring a new setup to expand what I currently have running in my homelab.

One of my goals currently is to gain a deeper understanding of kubernetes, and I figured the best way to do this would be to gain some more hands on experience outside of my professional enviornments I'm exposed to at work.

## Overview

Currently I am running a single virtualized node "staging" cluster to get things up and running. I wanted to have a few focus areas to keep in the forefront of my mind during this project:

- Gitops wherever possible, baking in automation wherever appropriate
- Keeping things as secure as possible through the use of encrypting all secrets pushed to git, keeping nodes in a separate VLAN from the rest of my network, etc
- Building a strong basic infrastructure that allows for as much scalability as possible
- Staying organized to make things easier for myself and anybody else exploring this repo
- Documenting my progress and changes I make over time so I can learn from my successes and mistakes

## Current infrastructure/configuration

- [K3s](https://k3s.io/) - K3s is the kubernetes distribution I opted to go for due to its lightweight nature and widespread use
- [Flux](https://fluxcd.io/) - Flux is being used for gitops/continuous delivery into my cluster
- [Prometheus/Grafana](https://github.com/prometheus-community/helm-charts/tree/main) - I am using the kube-prometheus-stack to easily deploy and control prometheus and grafana for stack and application monitoring
- [Kustomize](https://kustomize.io/) - Heavily used by flux, this is how I am structuring changes between clusters, and I believe this creates a scalable way to deploy applications across clusters when I eventually get a "production" cluster running
- [Cloudflare Tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) - This provides a secure yet simple way to expose applications to the public internet
- [Traefik](https://github.com/traefik/traefik) - I'm currently using Traefik to serve as my ingress controller
- [Renovate](https://github.com/renovatebot/renovate) - Automated image updates through creating PRs to keep my applications up to date
- [SOPS](https://fluxcd.io/flux/guides/mozilla-sops/) - Used to handle encryption/decryption of secrets so I can safely keep secrets in code

## Directory structure

To keep things organized, I've sticked to Flux's official [guide for structuring repositories](https://fluxcd.io/flux/guides/repository-structure/). Specifically, I'm basing my structure on their "monorepo" way of handling things:

```
├── apps
│   ├── base
│   ├── production 
│   └── staging
├── infrastructure
│   ├── base
│   ├── production 
│   └── staging
└── clusters
    ├── production
    └── staging
```

## Currently Deployed applications

- [Linkding](https://github.com/sissbruecker/linkding) - A self-hosted bookmark manager. I followed a tutorial to assist in getting this stood up as a demo application, but I am using it to save articles and other links for myself. Currently available hosted by myself here https://linkding.natetech.org/

## Current goals/additional plans

- Set up an additional "production" cluster
- Deploy more applications
- Explore possibly using [cert-manager](https://github.com/cert-manager/cert-manager) to automate issuance/renewal of [Lets Encrypt](https://letsencrypt.org/) certs
- Automated alerting - right now I'm considering using [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) to send automated alerts from prometheus to a discord server