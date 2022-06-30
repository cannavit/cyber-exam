# Introduction to NGINX

[NGINX](https://www.nginx.com/resources/glossary/nginx/) is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.

![image](https://cdn-media-1.freecodecamp.org/images/RooSvbKlAWsOjkz8JPactXH-GPf4Pe6DC3Ue ':size=80%')


I have selected a simple EKS architecture with two t3.micro nodes. I will create the infrastructure using terraform.

## Requirements

1. Have access K8s Cluster
2. Install [Kubectl](https://kubernetes.io/docs/tasks/tools/) Client
3. Install [Helm](https://helm.sh/docs/intro/install/)

## Install NGINX

I am going to configure NGINX in the cluster using a CHART the helm. This is a fast and secure way to add configurations and applications to the cluster.


I use the CHART [ingress-nginx](https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx) supported by the NGINX/KUBERNETES team:


Add repository

    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

Install chart

    helm install my-ingress-nginx ingress-nginx/ingress-nginx --version 4.1.4

Expected response

    ceciliocannavaciuolo@MBP-de-Cecilio cyber-exam % helm install my-ingress-nginx ingress-nginx/ingress-nginx --version 4.1.4
    NAME: my-ingress-nginx
    LAST DEPLOYED: Thu Jun 30 17:56:02 2022
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    The ingress-nginx controller has been installed.
    It may take a few minutes for the LoadBalancer IP to be available.
    You can watch the status by running 'kubectl --namespace default get services -o wide -w my-ingress-nginx-controller'

    An example Ingress that makes use of the controller:
      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        name: example
        namespace: foo
      spec:
        ingressClassName: nginx
        rules:
          - host: www.example.com
            http:
              paths:
                - pathType: Prefix
                  backend:
                    service:
                      name: exampleService
                      port:
                        number: 80
                  path: /
        # This section is only required if TLS is to be enabled for the Ingress
        tls:
          - hosts:
            - www.example.com
            secretName: example-tls

    If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

      apiVersion: v1
      kind: Secret
      metadata:
        name: example-tls
        namespace: foo
      data:
        tls.crt: <base64 encoded cert>
        tls.key: <base64 encoded key>
      type: kubernetes.io/tls
