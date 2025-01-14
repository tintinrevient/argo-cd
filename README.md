**Releases:**
[![Release Version](https://img.shields.io/github/v/release/argoproj/argo-cd?label=argo-cd)](https://github.com/argoproj/argo-cd/releases/latest)
[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/argo-cd)](https://artifacthub.io/packages/helm/argo/argo-cd)

**Code:** 
[![Integration tests](https://github.com/argoproj/argo-cd/workflows/Integration%20tests/badge.svg?branch=master)](https://github.com/argoproj/argo-cd/actions?query=workflow%3A%22Integration+tests%22)
[![codecov](https://codecov.io/gh/argoproj/argo-cd/branch/master/graph/badge.svg)](https://codecov.io/gh/argoproj/argo-cd)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/4486/badge)](https://bestpractices.coreinfrastructure.org/projects/4486)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fargoproj%2Fargo-cd.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fargoproj%2Fargo-cd?ref=badge_shield)

**Social:**
[![Twitter Follow](https://img.shields.io/twitter/follow/argoproj?style=social)](https://twitter.com/argoproj)
[![Slack](https://img.shields.io/badge/slack-argoproj-brightgreen.svg?logo=slack)](https://argoproj.github.io/community/join-slack)

# Argo CD - Declarative Continuous Delivery for Kubernetes

## What is Argo CD?

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.

![Argo CD UI](docs/assets/argocd-ui.gif)

[![Argo CD Demo](https://img.youtube.com/vi/0WAm0y2vLIo/0.jpg)](https://youtu.be/0WAm0y2vLIo)

## Why Argo CD?

1. Application definitions, configurations, and environments should be declarative and version controlled.
1. Application deployment and lifecycle management should be automated, auditable, and easy to understand.

## Who uses Argo CD?

[Official Argo CD user list](USERS.md)

## Documentation

To learn more about Argo CD [go to the complete documentation](https://argo-cd.readthedocs.io/).
Check live demo at https://cd.apps.argoproj.io/.

## Demo

1. Create the namespace `argocd`:
```bash
kubectl create namespace argocd
```

2. Install `argocd` resources in Kubernetes:
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3. Ensure that all the resources are up and running:
```bash
kubectl get pods -n argocd

NAME                                                READY   STATUS    RESTARTS     AGE
argocd-application-controller-0                     1/1     Running   0            9h
argocd-applicationset-controller-7c86dd8cd7-hq2c4   1/1     Running   0            9h
argocd-dex-server-786fb4b8b-nrj4t                   1/1     Running   0            9h
argocd-notifications-controller-7c946895bb-lcj5v    1/1     Running   0            9h
argocd-redis-598f75bc69-pcvhh                       1/1     Running   0            9h
argocd-repo-server-648db4756c-ds62c                 1/1     Running   3 (8h ago)   9h
argocd-server-6cfb678659-kmmrc                      1/1     Running   2 (8h ago)   9h
```

4. Configure `port-forward` to `argocd` dashboard:
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

4. Instead of port forwarding, you can also patch the service of `argocd-server` from type `ClusterIP` to type `LoadBalancer` as below:
```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

5. Log into the dashboard - https://localhost:8080, with username `admin` and password retrieved as below:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

qFmxbSUvjDblJ4mW
```

6. Deploy the example application `sock-shop` from https://github.com/argoproj/argocd-example-apps in the dashboard:
<p float="left">
  <img src="pix/sock-shop-app-config.png" width="650" />
</p>

7. Check the overview of the application `sock-shop` in the dashboard:
<p float="left">
  <img src="pix/sock-shop-app-overview.png" width="850" />
</p>

<ins>The things to pay attention to are as below</ins>:

1. The images pulled for `argocd` are as below:
```bash
REPOSITORY                        TAG             IMAGE ID       CREATED         SIZE
quay.io/argoproj/argocd           v2.5.8          ea3fb8e9ac54   17 hours ago    369MB
redis                             7.0.7-alpine    26b875a60c63   2 weeks ago     29.9MB
ghcr.io/dexidp/dex                v2.35.3         0dcae8edf686   3 months ago    90.3MB
```

2. Remove the example app images by `docker rmi $(docker images | grep weaveworksdemos | tr -s ' ' | cut -d ' ' -f 3)`.

## References

* https://argo-cd.readthedocs.io/en/stable/developer-guide/running-locally/
* https://github.com/argoproj/argo-cd/blob/master/docs/developer-guide/running-locally.md
* https://argoproj.github.io/argo-workflows/quick-start/
* https://github.com/argoproj/argo-workflows/
* https://github.com/argoproj/argocd-example-apps
* https://microservices-demo.github.io/docs/quickstart.html
* https://github.com/argoproj/argo-helm
* https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* https://docs.docker.com/config/pruning/
* https://github.com/argoproj/argo-helm

## Community

### Contribution, Discussion and Support

 You can reach the Argo CD community and developers via the following channels:

* Q & A : [Github Discussions](https://github.com/argoproj/argo-cd/discussions)
* Chat : [The #argo-cd Slack channel](https://argoproj.github.io/community/join-slack)
* Contributors Office Hours: [Every Thursday](https://calendar.google.com/calendar/u/0/embed?src=argoproj@gmail.com) | [Agenda](https://docs.google.com/document/d/1xkoFkVviB70YBzSEa4bDnu-rUZ1sIFtwKKG1Uw8XsY8)
* User Community meeting: [First Wednesday of the month](https://calendar.google.com/calendar/u/0/embed?src=argoproj@gmail.com) | [Agenda](https://docs.google.com/document/d/1ttgw98MO45Dq7ZUHpIiOIEfbyeitKHNfMjbY5dLLMKQ)


Participation in the Argo CD project is governed by the [CNCF Code of Conduct](https://github.com/cncf/foundation/blob/master/code-of-conduct.md)


### Blogs and Presentations

1. [Awesome-Argo: A Curated List of Awesome Projects and Resources Related to Argo](https://github.com/terrytangyuan/awesome-argo)
1. [Unveil the Secret Ingredients of Continuous Delivery at Enterprise Scale with Argo CD](https://blog.akuity.io/unveil-the-secret-ingredients-of-continuous-delivery-at-enterprise-scale-with-argo-cd-7c5b4057ee49)
1. [GitOps Without Pipelines With ArgoCD Image Updater](https://youtu.be/avPUQin9kzU)
1. [Combining Argo CD (GitOps), Crossplane (Control Plane), And KubeVela (OAM)](https://youtu.be/eEcgn_gU3SM)
1. [How to Apply GitOps to Everything - Combining Argo CD and Crossplane](https://youtu.be/yrj4lmScKHQ)
1. [Couchbase - How To Run a Database Cluster in Kubernetes Using Argo CD](https://youtu.be/nkPoPaVzExY)
1. [Automation of Everything - How To Combine Argo Events, Workflows & Pipelines, CD, and Rollouts](https://youtu.be/XNXJtxkUKeY)
1. [Environments Based On Pull Requests (PRs): Using Argo CD To Apply GitOps Principles On Previews](https://youtu.be/cpAaI8p4R60)
1. [Argo CD: Applying GitOps Principles To Manage Production Environment In Kubernetes](https://youtu.be/vpWQeoaiRM4)
1. [Creating Temporary Preview Environments Based On Pull Requests With Argo CD And Codefresh](https://codefresh.io/continuous-deployment/creating-temporary-preview-environments-based-pull-requests-argo-cd-codefresh/)
1. [Tutorial: Everything You Need To Become A GitOps Ninja](https://www.youtube.com/watch?v=r50tRQjisxw) 90m tutorial on GitOps and Argo CD.
1. [Comparison of Argo CD, Spinnaker, Jenkins X, and Tekton](https://www.inovex.de/blog/spinnaker-vs-argo-cd-vs-tekton-vs-jenkins-x/)
1. [Simplify and Automate Deployments Using GitOps with IBM Multicloud Manager 3.1.2](https://www.ibm.com/cloud/blog/simplify-and-automate-deployments-using-gitops-with-ibm-multicloud-manager-3-1-2)
1. [GitOps for Kubeflow using Argo CD](https://v0-6.kubeflow.org/docs/use-cases/gitops-for-kubeflow/)
1. [GitOps Toolsets on Kubernetes with CircleCI and Argo CD](https://www.digitalocean.com/community/tutorials/webinar-series-gitops-tool-sets-on-kubernetes-with-circleci-and-argo-cd)
1. [CI/CD in Light Speed with K8s and Argo CD](https://www.youtube.com/watch?v=OdzH82VpMwI&feature=youtu.be)
1. [Machine Learning as Code](https://www.youtube.com/watch?v=VXrGp5er1ZE&t=0s&index=135&list=PLj6h78yzYM2PZf9eA7bhWnIh_mK1vyOfU). Among other things, describes how Kubeflow uses Argo CD to implement GitOPs for ML
1. [Argo CD - GitOps Continuous Delivery for Kubernetes](https://www.youtube.com/watch?v=aWDIQMbp1cc&feature=youtu.be&t=1m4s)
1. [Introduction to Argo CD : Kubernetes DevOps CI/CD](https://www.youtube.com/watch?v=2WSJF7d8dUg&feature=youtu.be)
1. [GitOps Deployment and Kubernetes - using Argo CD](https://medium.com/riskified-technology/gitops-deployment-and-kubernetes-f1ab289efa4b)
1. [Deploy Argo CD with Ingress and TLS in Three Steps: No YAML Yak Shaving Required](https://itnext.io/deploy-argo-cd-with-ingress-and-tls-in-three-steps-no-yaml-yak-shaving-required-bc536d401491)
1. [GitOps Continuous Delivery with Argo and Codefresh](https://codefresh.io/events/cncf-member-webinar-gitops-continuous-delivery-argo-codefresh/)
1. [Stay up to date with Argo CD and Renovate](https://mjpitz.com/blog/2020/12/03/renovate-your-gitops/)
1. [Setting up Argo CD with Helm](https://www.arthurkoziel.com/setting-up-argocd-with-helm/)
1. [Applied GitOps with Argo CD](https://thenewstack.io/applied-gitops-with-argocd/)
1. [Solving configuration drift using GitOps with Argo CD](https://www.cncf.io/blog/2020/12/17/solving-configuration-drift-using-gitops-with-argo-cd/)
1. [Decentralized GitOps over environments](https://blogs.sap.com/2021/05/06/decentralized-gitops-over-environments/)
1. [How GitOps and Operators mark the rise of Infrastructure-As-Software](https://paytmlabs.com/blog/2021/10/how-to-improve-operational-work-with-operators-and-gitops/)
1. [Getting Started with ArgoCD for GitOps Deployments](https://youtu.be/AvLuplh1skA)
1. [Using Argo CD & Datree for Stable Kubernetes CI/CD Deployments](https://youtu.be/17894DTru2Y)

