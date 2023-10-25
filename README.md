# Argo CD GitOps Demo

[![Set random UI colour](https://github.com/markwcodes/argocd-gitops-demo/actions/workflows/randomhex.yaml/badge.svg)](https://github.com/markwcodes/argocd-gitops-demo/actions)

### Live demo: [podinfo.wilson.codes](https://podinfo.wilson.codes/)

## Overview

This project demonstrates automated continuous deployment following GitOps practices by utilising **Kubernetes**, **Argo CD** and **Kustomize**.

A custom GitHub Action applies a random UI colour to the application every hour, adding an interactive touch to the automated deployment process.

## How it works

Argo CD is setup to periodically scan for changes in this repository on branch `feat/random-ui-colour`.

A custom Github action updates the app's [config file](https://github.com/markwcodes/argocd-gitops-demo/blob/feat/random-ui-colour/gitops-config/podinfo.properties) every hour and randomises the UI colour.
 <!-- (on branch `feat/random-ui-colour`) -->

Once changes are detected, using git-ops principles, Argo CD uses the state defined in git as the source of truth, and kicks off a workflow to reconcile the cluster's state.

Kustomize provides field replacement based on patches defined in `kustomization.yaml` and builds a config map containing new settings, that is then applied to the app deployment.

## Setup & Requirements

1. Install Argo CD in your cluster

2. Access in-cluster Argocd UI:
`kubectl -n argocd port-forward svc/argocd-server 8080:443`\
Then navigate to: [localhost:8080](https://localhost:8080/)

3. Fetch the admin password: `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo`

4. Login and setup a new application

5. Enable auto-sync and resource pruning

## Screenshots

Argo CD UI:\
![image](https://github.com/markwcodes/argocd-gitops-demo/assets/7064464/04cdfca8-1fc6-48d5-80af-6a510ea6aaa3)

Podinfo app:\
![image](https://github.com/markwcodes/argocd-gitops-demo/assets/7064464/395881f7-2a7f-43b9-ab8e-2b27174210a7)

## To-do

- Setup Argo CD ingress and endpoint
- Use Github event webhooks instead of Argo CD polling
- Setup nginx ingress controller for demo apps to reduce the use of DigitalOcean load balancers
