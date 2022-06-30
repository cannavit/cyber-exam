# Welcome to Exam by DevOps position

> Cecilio Cannavacciuolo
## Exam Questions

On a local kubernetes cluster:

1. Run three different containers of the same docker image “vad1mo/hello-world-rest:latest”
(from official docker.io repository);
2. Expose a cluster port to external connections;
3. User can query all rests through one endpoint on the exposed port;
4. First container MUST respond on public REST path “/one”;
5. Second container MUST respond on public REST path “/two”;
6. Third container MUST respond on public REST path “/three”;
7. Connection SHOULD be https, certificates SHOULD be generated internally.

Please provide all yaml files and any additional files if required.

### Requirements

To run the files in your local environment you need:


###  Table of contents

1. [Introduction](#tls-certificates)
2. [Architecture based in MicroServices](#canary-flagger)


## Solution Exam

### Solution


Question: Run three different containers of the same docker image “vad1mo/hello-world-rest:latest”
(from official docker.io repository);


### Resolution strategy

To solve the exam questions, I decided to develop the following architecture. It comprises a Kubernetes cluster (K8s), created through a configuration with Terraform with AWS.
The architecture has three services that use the same image. A dynamic TLS configuration managed letsencrypt and created via HELM. An Ingress service was also created with HELM and addressed to have a URL for each service.
I have also configured an external monitoring service that automatically verifies that all services are operational.

![image](docs/assets/cyber-exam.png ':size=80%')

### Solution of Q1

I have one list of manifiest inside of the folder in the route /k8s. With the configuration for run three different service with the same image. Here is possible to view the result after to run the command

Apply the Manifiest

    kubectl apply -f k8s/

Expected result:

    namespace/cyber-exam-cannavit created
    deployment.apps/hello-first created
    horizontalpodautoscaler.autoscaling/hello-first created
    service/hello-first created
    deployment.apps/hello-second created
    horizontalpodautoscaler.autoscaling/hello-second created
    service/hello-second created
    deployment.apps/hello-third created
    horizontalpodautoscaler.autoscaling/hello-third created
    service/hello-third created
    clusterissuer.cert-manager.io/letsencrypt-hello unchanged
    ingress.networking.k8s.io/ingress-hello-first created

View all Resources 

     kubectl get all -n cyber-exam-cannavit

Expected Result (Result Q1)

    NAME                               READY   STATUS    RESTARTS   AGE
    pod/hello-first-67c95f7dff-gx6pj   1/1     Running   0          2m23s
    pod/hello-second-566db6669-hxl4g   1/1     Running   0          2m23s
    pod/hello-third-569895d4d8-qwqnm   1/1     Running   0          2m22s

    NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
    service/hello-first    ClusterIP   172.20.132.20    <none>        5050/TCP   2m23s
    service/hello-second   ClusterIP   172.20.214.175   <none>        5050/TCP   2m23s
    service/hello-third    ClusterIP   172.20.208.167   <none>        5050/TCP   2m22s

    NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/hello-first    1/1     1            1           2m23s
    deployment.apps/hello-second   1/1     1            1           2m23s
    deployment.apps/hello-third    1/1     1            1           2m22s

    NAME                                     DESIRED   CURRENT   READY   AGE
    replicaset.apps/hello-first-67c95f7dff   1         1         1       2m23s
    replicaset.apps/hello-second-566db6669   1         1         1       2m23s
    replicaset.apps/hello-third-569895d4d8   1         1         1       2m22s


### Solution of Q2 to Q7

The answer of Q2, Q3, Q4, Q5, Q6, and Q7. They are associated with each other so that I will answer them all in this section. I have exposed the connection to the cluster by using the Ingress configuration.

#### Configuration of Ingress

Here I show the configuration that I have used in the Ingress. You can find this file in path k8s/05-nginx-config.yaml


    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: ingress-hello-first
      namespace: cyber-exam-cannavit
      annotations:
        cert-manager.io/cluster-issuer: "letsencrypt-hello"
    spec:
      ingressClassName: nginx
      rules:
        - host: cyber-exam.cannavit.dev        # >>>>> >>>>> >>>>> CONFIG Q3
          http:
            paths:
              - pathType: Prefix                # >>>>> >>>>> >>>>> CONFIG Q4
                backend:
                  service:
                    name: hello-first
                    port:
                      number: 5050
                path: /one
              - pathType: Prefix                # >>>>> >>>>> >>>>> CONFIG Q5
                backend:
                  service:
                    name: hello-second
                    port:
                      number: 5050
                path: /two
              - pathType: Prefix                # >>>>> >>>>> >>>>> CONFIG Q6
                backend:
                  service:
                    name: hello-third
                    port:
                      number: 5050
                path: /three
      tls:                                     # >>>>> >>>>> >>>>> CONFIG Q7
        - hosts:                  
          - cyber-exam.cannavit.dev
          secretName: cyber-prod-tls


Next I will add the answers, for a matter of resource optimization I will add the snippets and once the answers have been written I will deactivate the EKS Cluster that way I will not incur additional costs

#### List of IPs from Pods

    kubectl get pods -n $NAMESPACE -w -o wide

![image](docs/assets/ScreenShot2022-06-30at19.32.55.png ':size=100%')

#### Q4 - First container MUST respond on public REST path “/one”;

As you can see in the indicated URL the service hello-first, which points to the IP address 10.0.2.256. This is the IP address of the pod. So this question has been completed successfully

![image](docs/assets/ScreenShot2022-06-30at19.25.30.png ':size=80%')

#### Q5 - First container MUST respond on public REST path “/two”;

As you can see in the indicated URL the service hello-second, which points to the IP address 10.0.2.169. This is the IP address of the pod. So this question has been completed successfully

![image](docs/assets/ScreenShot2022-06-30at19.25.44.png ':size=80%')

#### Q6 - First container MUST respond on public REST path “/three;

As you can see in the indicated URL the service hello-second, which points to the IP address 10.0.2.189. This is the IP address of the pod. So this question has been completed successfully

![image](docs/assets/ScreenShot2022-06-30at19.25.56.png ':size=80%')

#### Q7 - Connection SHOULD be https, certificates SHOULD be generated internally.;

I used the Letsencrypt service to generate the certificates automatically, Letsencrypt also keeps the certificates updated. You can see the TLS-Certificate section in this documentation to see a little more about this service.

You can see the certificates and secure URL on the next page

![image](docs/assets/ScreenShot2022-06-30at20.21.52.png ':size=80%')

![image](docs/assets/ScreenShot2022-06-30at20.22.14.png ':size=80%')


### Conclusion

I was able to successfully complete each section of the requested exam. This exam was exercised to show skills related to [Kubernetes](https://kubernetes.io/), Pods, Use of Services, Income, and Certificate Management with [letsencrypt](https://letsencrypt.org/). Additionally, execute the infrastructure in [Terraform](https://www.terraform.io/) with the [AWS](https://aws.amazon.com/) provider. This will also show skills from the point of view of automation of infrastructure execution and the use of Roles, and permissions in AWS, in addition to configuring a private DNS. Also, create a documented flow using [Docsify](https://docsify.js.org/#/). I hope to have met your expectations.

Thank you so much 😉

Cecilio Cannavacciuolo

![](docs/assets/the-office-bow.gif "-gifcontrol-disabled;")
