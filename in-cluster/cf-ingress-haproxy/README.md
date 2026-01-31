This is a drop in replacement for ingress-nginx!

The goal is to laterally shift to contour and continue to leverage the extremely stable Ingress API. When the Gateway API becomes more stable, we will enable that capability with Contour

The steps to deploy and sunset ingress-nginx are as follows:

1. Install cf-ingress-contour helm chart
2. Update all ingress templates/charts ingressClassName from "nginx" to "contour" and redeploy
3. Test connectivity to all endpoints via the browser
4. Uninstall ingress-nginx chart

_IMPORTANT_ If there are any specific header mappings that were implemented in nginx-ingress to allow for the passing of SSL specific metadata (e.g. Common Name) that will have to be tested
