# k3s-homelab

** Greetings! My name is Nate. Over the past few years, I've been running a homelab to explore new technologies and methodologies, and provide myself an enviornment to work in that can allow me to learn effectively. **

One of my goals currently is to gain a deeper understanding of kubernetes, and I figured the best way to do this would be to gain some more hands on experience outside of my professional enviornments I use at work.

## Overview

Currently I am running a single virtualized node "staging" cluster to get things up and running. Overall infrastructure is as follows:

## Current infrastructure/configuration

- [Dotfiles](https://github.com/christianlempa/dotfiles) - My personal configuration files on macOS
- [K3s](https://k3s.io/) - K3s is the kubernetes distribution I opted to go for due to its lightweight nature and widespread use
- [Flux](https://fluxcd.io/) - Flux is being used for gitops/continuous delivery into my cluster
- [Prometheus/Grafana](https://github.com/prometheus-community/helm-charts/tree/main) - I am using the kube-prometheus-stack to easily deploy and control prometheus and grafana for stack and application monitoring

## Currently Deployed applications

- [Linkding](https://github.com/sissbruecker/linkding) - A self-hosted bookmark manager. I followed a tutorial to assist in getting this stood up as a demo application, but I am using it to save articles and other links for myself. Currently available hosted by myself here https://linkding.natetech.org/

## Current goals/additional plans

- Set up an additional "production" cluster
- Expand the current applications I have deployed. 
- Configure automated updates to container versions
