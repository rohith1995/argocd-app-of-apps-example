# argocd-app-of-apps-example

This ArgoCD app of apps can be run from the bootstrap.yaml file using the following command:

```
kubectl apply -f bootstrap.yaml
```

## Components

It has 4 components (will be 6 in the final version):
 - cert manager
 - pod admission controller
 - k8s-sidecar (a modified version of the kiwigrid app)
 - promethetheus-exporters-uber-chart which is an umbrella Helm chart containing:
   - the Kubernetes Prometheus stack, including Prometheus Operator and Grafana
   - two Prometheus metrics exporters, one demonstrating a 1:many relationship with the app being scraped (Postgres) and the other a 1:1 relationship

## Parameters

Parameters may be customised to suit the particular cluster config.

**N.B. It will also be necessary to make a new fork or branch of the prometheus-exporters-uber-chart repo if you want to change Prometheus exporters**

**N.B. As well as the parameters mentioned, each component can be enabled or disabled and can have its namespace set.**

```
prometheuswithexporters:
  repourl: https://user@gerrit.ericsson.se/a/argonauts/cos.git # use this if you want to point to a different fork or branch of the prometheus-exporters-uber-chart repo
  path: prometheus-exporters-uber-chart
  targetrevision: master
k8ssidecar:
  repourl: https://user@gerrit.ericsson.se/a/argonauts/k8s-sidecar.git
  path: .
  targetrevision: master
  folderId: 0
  folderUid: "l3KqBxCMz"
  sidecar:
    env: # Grafana API details - must correspond with the setup inside the prometheus-uber-chart component
      label: grafana_dashboard
      reqMethod: POST
      reqUrl: http://172.17.0.20:3000
      reqUsername: admin
      reqPassword: prom-operator
```
