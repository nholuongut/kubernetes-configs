# Kubernetes configs

![](https://i.imgur.com/waxVImv.png)
### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
<br/>

Advanced Kubernetes YAML configurations & templates, based on my experiences running Kubernetes in production at different companies.

The top-level directory contains standard Kubernetes object templates with many Best Practices, Tips & Tricks learned over time across production environments.

The sub-directories contain ready-to-run real world apps that I've run across environments.

## Templates

Start with [deployment.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/deployment.yaml) / [statefulset.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/statefulset.yaml), for advanced users see [kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/kustomization.yaml).

The [service.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/service.yaml) and
[ingress.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/service.yaml) configs contain settings
for using static public IP addresses and locking down your cloud load balancer's firewall rules eg. to private IP
addresses, and patches for Cloudflare Proxied or VPN IPs. You may need to extend those IP lists to your office /
VPN / public addresses if really want to permit direct internet access to your ingresses and aren't proxying them
through a WAF in proxied mode etc.

See also the [Dockerfile](https://github.com/nholuongut/templates/blob/master/Dockerfile) template in the
[nholuongut/templates](https://github.com/nholuongut/templates) repo to containerize your custom apps for deploying
on to Kubernetes.

## Apps

Real-world app deployments are found in the more specific `<app>/` directories.

These follow the standard Kustomize `<app>/base/` and `<app>/overlay/` layout to make it easy to use as-is by just tweaking a couple settings in the overlay to your specific environment.

## CI/CD

Advanced auto-scaling production-grade CI/CD on Kubernetes:

- [ArgoCD](https://argoproj.github.io/cd/) - deployment, configs and optimizations. Start here: [argocd/base/kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/argocd/base/kustomization.yaml)
- [Jenkins](https://www.jenkins.io/) - jenkins server and dynamically scaling agents on kubernetes. Start here: [jenkins/base/kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/jenkins/base/kustomization.yaml)
  - see also: [Jenkins](https://github.com/nholuongut/Jenkins) repo with advanced Jenkinsfile & Jenkins Shared Library
- [TeamCity](https://www.jetbrains.com/teamcity/) - teamcity server and dynamically scaling agents on kubernetes. Start here: [teamcity/base/kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/teamcity/base/kustomization.yaml)
- [Selenium Grid](https://www.selenium.dev/documentation/grid/) - simple and distributed auto-scaling deployments. Start here: [selenium-grid/base/kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/selenium-grid/base/kustomization.yaml) / [selenium-grid-distributed/base/kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/selenium-grid-distributed/base/kustomization.yaml)

## Helm Repos

Helm repos you should probably have installed:

| Repo Label       | URL                                 |
|------------------|-------------------------------------|
| stable           | https://charts.helm.sh/stable       |
| bitnami          | https://charts.bitnami.com/bitnami  |
| fairwinds-stable | https://charts.fairwinds.com/stable |

Table generated from [helm-repos.txt](https://github.com/nholuongut/kubernetes-configs/blob/master/helm-repos.txt)
by script [generate_repos_markdown_table.sh](https://github.com/nholuongut/kubernetes-configs/blob/master/generate_repos_markdown_table.sh)

These repos can quickly installed in one command using script [install_repos.sh](https://github.com/nholuongut/kubernetes-configs/blob/master/install_repos.sh):

```shell
./install_repos.sh
```

## Helm + Kustomize integration

See [kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/kustomization.yaml) for 2 methods provided:

1. template the Helm chart using a `values.yaml` to Git and serve from there (see [DevOps Bash Tools](https://github.com/nholuongut/devops-bash-tools) for the `helm_template.sh` convenience script)
1. dynamically load the Helm chart from upstream with a `values.yaml`

...then patch override anything the chart doesn't directly support using the standard Kustomize patching examples given in the [kustomization.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/kustomization.yaml).

## Production Ready Checklist

This section has been moved and enhanced in my amazing
[Knowledge-Base](https://github.com/nholuongut/knowledge-base/blob/master/kubernetes-production-ready-checklist.md)
repo:

[Kubernetes Production Ready Checklist](https://github.com/nholuongut/knowledge-base/blob/master/kubernetes-production-ready-checklist.md)

## Further Documentation

The best documentation links are provided at the top of each yaml for fast referencing (my advanced [.vimrc](https://github.com/nholuongut/devops-bash-tools/blob/master/configs/.vimrc) can open these URLs from the current file via a hotkey!)

See also [nholuongut/knowledge-base](https://github.com/nholuongut/knowledge-base), especially:

- [Kubernetes](https://github.com/nholuongut/knowledge-base/blob/main/kubernetes.md)
- [Helm](https://github.com/nholuongut/knowledge-base/blob/main/helm.md)
- [Kustomize](https://github.com/nholuongut/knowledge-base/blob/main/kustomize.md)
- [ArgoCD](https://github.com/nholuongut/knowledge-base/blob/main/argocd.md)
- [Cert Manager](https://github.com/nholuongut/knowledge-base/blob/main/cert-manager.md)

## Extra Docs

[Datree Kubernetes ArgoCD best practices](https://www.datree.io/resources/argocd-best-practices-you-should-know)

## Environment Enhancements

[.envrc](https://github.com/nholuongut/kubernetes-configs/blob/master/.envrc) - use with `direnv` to auto-load correct Kubernetes context isolated to current shell to avoid race conditions between shells and scripts caused by naively changing the global `~/.kube/config` context

Shortcut symlinks are for faster instantiation from these configs using the standard kubernetes shortcuts such as `new pvc.yaml` - see the [Templates](https://github.com/nholuongut/templates) repo for more details on the `new` command to fast create new files from templates.

## Diagrams

For more amazing diagrams see [nholuongut/diagrams-as-code](https://github.com/nholuongut/diagrams-as-code)

### Kubernetes Deployment with Horizontal Pod Autoscaler and Ingress

- [deployment.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/deployment.yaml)
- [horizontal-pod-autoscaler.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/horizontal-pod-autoscaler.yaml)
- [ingress.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/ingress.yaml)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/kubernetes_deployment_hpa_ingress.png)

### Kubernetes Stateful Architecture with persistent volumes

- [statefulset.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/statefulset.yaml)
- [persistent-volume-claim.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/persistent-volume-claim.yaml)
- [persistent-volume.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/persistent-volume.yaml)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/kubernetes_stateful_architecture.png)

### Kubernetes Service External Traffic Policy

- [service.yaml](https://github.com/nholuongut/kubernetes-configs/blob/master/service.yaml#L141)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/kubernetes_external_traffic_policy.svg)

### Kubernetes on Premise

- GitHub repo: [nholuongut/haproxy-configs](https://github.com/nholuongut/haproxy-configs)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/kubernetes_on_premise.svg)

with [MetalLB](https://metallb.universe.tf/):

- [metal-lb/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/metal-lb/base)

Is it just me or do MetaLB think they're [Starfleet](https://en.wikipedia.org/wiki/Starfleet)? (compare their logos)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/kubernetes_on_premise_metallb.svg)

### Traefik Ingress on GKE

A Traefik deployment I did for a client using:

- [traefik/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/traefik/base)
- [traefik-hub-agent/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/traefik-hub-agent/base)

### Kong API Gateway on AWS EKS

A Kong API Gateway deployment I did for a client using:

- [kong/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/kong/base)
- [cert-manager/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/cert-manager/base)
- [argocd/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/argocd/base)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/kubernetes_kong_api_gateway_eks.png)

### Jenkins on Kubernetes

A production Jenkins on Kubernetes I built for a client with auto-spawning agents for horizontal scaling and integration with Docker, SonarQube, Clair, Grype and Trivy for code & container scanning.

- [jenkins/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/jenkins/base) - core config that is inherited by all environments
- [jenkins/overlay/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/jenkins/overlay) - environment specific settings like ingress address, NFS server mount on agents
- [claire/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/clair/base)
- [sonarqube/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/sonarqube/base)
- [trivy/base/*.yaml](https://github.com/nholuongut/kubernetes-configs/tree/master/trivy/base)
- GitHub repo: [nholuongut/Jenkins](https://github.com/nholuongut/Jenkins)
  - Advanced [Jenkinsfile](https://github.com/nholuongut/Jenkins/blob/master/Jenkinsfile)
  - [Groovy Shared Library](https://github.com/nholuongut/Jenkins/tree/master/vars) with the code & container scanning functions
    - [clair.groovy](https://github.com/nholuongut/Jenkins/blob/master/vars/clair.groovy)
    - [grype.groovy](https://github.com/nholuongut/Jenkins/blob/master/vars/grype.groovy)
    - [trivy.groovy](https://github.com/nholuongut/Jenkins/blob/master/vars/trivy.groovy), [trivyFS.groovy](https://github.com/nholuongut/Jenkins/blob/master/vars/trivyFS.groovy), [trivyImages](https://github.com/nholuongut/Jenkins/blob/master/vars/trivyImages.groovy)
    - [gcrDockerAuth.groovy](https://github.com/nholuongut/Jenkins/blob/master/vars/gcrDockerAuth.groovy), [garDockerAuth.groovy](https://github.com/nholuongut/Jenkins/blob/master/vars/garDockerAuth.groovy)
    - among others in [vars/](https://github.com/nholuongut/Jenkins/tree/master/vars) and don't forget about the epic [Jenkinsfile](https://github.com/nholuongut/Jenkins/blob/master/Jenkinsfile)

#### Useful Notes

[nholuongut/knowledge-base - Jenkins](https://github.com/nholuongut/knowledge-base/blob/main/jenkins.md)

[nholuongut/knowledge-base - Jenkins-on-Kubernetes](https://github.com/nholuongut/knowledge-base/blob/main/jenkins-on-kubernetes.md)

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/jenkins_kubernetes_cicd.svg)


### OpenTSDB on Kubernetes and HBase

A high scale production OpenTSDB replatform I did to Kubernetes for a client, ingesting 9 billion data points per day and serving 3 million queries per day.

I also had to do advanced performance tuning of their production HBase cluster which was suffering from frequent outages at this scale due to being set up by a non-SME on the wrong hardware (I had to make do with the existing hardware of course).

This was the second client I did in-depth performance tuning of HBase for - I've published a selection of useful HBase tools - see `hbase_*.py` and `opentsdb_*.py` in my [DevOps Python tools](https://github.com/nholuongut/DevOps-Python-tools) repo.

![](https://github.com/nholuongut/diagrams-as-code/raw/master/images/opentsdb_kubernetes_hbase.svg)


# 🚀 I'm are always open to your feedback.  Please contact as bellow information:
### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com)
* [PayPal.me](https://www.paypal.com/paypalme/nholuongut)

![](https://i.imgur.com/waxVImv.png)
![](Donate.png)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.🌟