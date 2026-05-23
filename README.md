
# Quick Argo Setup

## Deploy the App
kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## Get the Admin Secret
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

## Port Forward for Access
kubectl port-forward svc/argocd-server -n argocd 8080:443

## Apply Application Set Manifest
k apply -f argoApplicationSet.yaml -n argocd

# Testing Cluster Level App Sets

## Grafana
## Get the Admin Secret
kubectl -n monitoring get secret kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d; echo

## Port Forward for Access (No Ingress)
kubectl port-forward svc/kube-prometheus-stack-grafana -n argocd 8081:80


# The Vision
1. Group ApplicationSets by Cluster Environment
sandbox/
  APPS // for internal testing
dev/
  APPS // for program consumption in Dev/Test
prod/
  APPS // for program consumption in Production
2. Deploy all of the same apps across the (3) cluster environments -- 3 deployments total
3. Updates sync across deployments seemlessly

apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: cf-core-cluster-services-dev
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - list:
      elements:
      - cluster: program1-dev-cluster
        url: https://1.2.3.4
      - cluster: program2-dev-cluster
        url: https://2.4.6.8
      - cluster: program3-dev-cluster
        url: https://9.8.7.6



apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: cf-core-cluster-services-prod
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - list:
      elements:
      - cluster: program1-prod-cluster
        url: https://1.2.3.4
      - cluster: program2-prod-cluster
        url: https://2.4.6.8
      - cluster: program3-prod-cluster
        url: https://9.8.7.6


