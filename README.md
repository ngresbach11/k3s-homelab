# k3s-homelab

Greetings! My name is Nate. Over the past few years, I've been running a homelab to explore new technologies and methodologies, and provide myself an enviornment to play around in that can allow me to learn effectively. My homelab has gone through several iterations- what started as a simple old gaming computer running a NAS and a few VMs became a multi-node ESXi cluster which eventually became proxmox, and now I am exploring a new setup to expand what I currently have running in my homelab.

One of my goals currently is to gain a deeper understanding of kubernetes, and I figured the best way to do this would be to gain some more hands on experience outside of my professional enviornments I'm exposed to at work. I also figured that by hosting this repo publically, it would provide another resource for others to learn from, and a way for me to be able to document things I have done and learned along the way.

## Overview

Currently I am running two kubernetes clusters. The first being single virtualized node "staging" cluster that is being used to test applications, their deployments, etc. The second of the two is a cluster of three mini-pc's I bought used online for relatively cheap, with one server node and two agent nodes. This setup isn't "truly" highly available, but it is good enough for just my homelab.

 I wanted to have a few focus areas to keep in the forefront of my mind during this project:

- Gitops wherever possible, baking in automation wherever appropriate
- Keeping things as secure as possible through the use of encrypting all secrets pushed to git, keeping nodes in a separate VLAN from the rest of my network, running applications as a non-root user, etc
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
- [Cert-manager](https://cert-manager.io/) - Being used to automate the issuance and renewal of [Lets Encrypt](https://letsencrypt.org/) certificates to applications
- [Renovate](https://github.com/renovatebot/renovate) - Automated image updates through creating PRs to keep my applications up to date
- [SOPS](https://fluxcd.io/flux/guides/mozilla-sops/) - Used to handle encryption/decryption of secrets so I can safely keep secrets in code
- [Longhorn](https://longhorn.io/) - Distribued HA storage between nodes. I looked into Rook+Ceph and a few other options, but longhorn seemed lightweight and easy enough to use so I went with that for now.

## Directory structure

To keep things organized, I've sticked to Flux's official [guide for structuring repositories](https://fluxcd.io/flux/guides/repository-structure/). Specifically, I'm basing my structure on their "monorepo" way of handling things, as this will make things easier to manage when I end up adding another cluster:

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

## Currently deployed publically exposed applications

- [Linkding](https://github.com/sissbruecker/linkding) - A self-hosted bookmark manager. I followed a tutorial to assist in getting this stood up as a demo application, but I am using it to save articles and other links for myself. Available hosted and exposed at https://linkding.natetech.org/
- [Weatherstar 4000+](https://github.com/netbymatt/ws4kp) - Self hosted weather site that pulls data from [NOAA's weather API](https://www.weather.gov/documentation/services-web-api) and displays it in the style of a 90s weather channel. Being hosted and exposed at https://weatherstar.natetech.org

## Currently deployed non-public applications

- [OpenSpeedTest](https://github.com/openspeedtest/Speed-Test) - An open source, self hostable speedtest server
- [Open-WebUI](https://github.com/open-webui/open-webui?tab=readme-ov-file#open-webui-) - Self hosted web app for running locally hosted large language models. I'm currently running ollama on docker on another machine and communicating into that, but am playing with the idea of moving ollama to k8s as well, and setting it to use a gpu node.
- [Jellyfin](https://jellyfin.org/) - A free, open source media server. I honestly prefer the TV app for plex over jellyfin, but it can be nice to have a backup mediaserver running. I had plex running in here fine, but after about a week the TV app was having issues connecting (not sure if it's an issue because of going through traefik etc). Decided to move plex back to docker on another system. Jellyfin is a good backup in case plex breaks anyway.
- [Navidrome](https://github.com/navidrome/navidrome) - A free, open source music streaming platform. Think self-hosted spotify for your own music. I mostly did this for fun, but also because I have some music that isn't on spotify- various amateur artists' old music I have from years past and such. I'm using [Symfonium](https://symfonium.app/) on my phone currently to connect to this, because I think it's a bit better than navidrome's web UI.

## Current goals/additional plans

- Explore options for better storage. Right now most of my stuff I'm initially standing up is using default local-path storage class, considering setting up NFS or something similar
- Deploy more applications: specifically a dashboard, kanban board app (I want to experiment with putting some of my personal "todo" lists in this kind of format to organize things. Testing a few options now) and a few other apps I've played with in the past
- Automated alerting - right now I'm considering using [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) to send automated alerts from prometheus to a discord server
- Testing other ways of doing things. Right now I'm using flux for CD because I've never touched it before, but want to play with argo as well.