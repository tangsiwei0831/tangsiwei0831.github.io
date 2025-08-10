---
title: 'Kubernetes'
date: 2025-08-09
permalink: /posts/kube
tags:
  - Software
  - Devops
---
Port-forwarding
```
    set HTTP_PROXY = ...
    set HTTPs_proxy = ...

    gcloud init
    gcloud config set project ...
    gcloud container clusters get-credentials ... --zone northamerica-northeast1

    kubectl config set-context --current --namespace=...

    kubectl get pods
    kubectl port-forward  ... 8081:8080 -n ...

```