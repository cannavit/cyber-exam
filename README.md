# Welcome to Exam of 

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
eloped by the CyberExam team.


<!-- ![k8s](docs/assets/CyberExamDark.png':size=100%') -->

<!-- ![image](docs/assets/CyberExam_intro.png ':size=50%') -->

Kubernetes is a fantastic tool for this type of case.

### Requirements

To run the files in your local environment you need:


###  Table of contents

1. [Introduction](#tls-certificates)
2. [Architecture based in MicroServices](#canary-flagger)



<!-- ## Architecture based in MicroServices

> Helm  Chart: -->



helm template jetstack/cert-manager -n $NAMESPACE > template_tls.yaml