# Introduction to Terraform

[Terraform](https://www.terraform.io/) is an open-source infrastructure as code software tool created by HashiCorp. Users define and provide data center infrastructure using a declarative configuration language known as HashiCorp Configuration Language, or optionally JSON.


![image](https://www.terraform.io/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F58478%2F1650551824-block-3.png&w=3840&q=75 ':size=50%')

# Create Terraform Cluster with AWS

![image](../assets/cyber-exam-terraform.png ':size=40%')


I have selected a simple EKS architecture with two t3.micro nodes. I will create the infrastructure using terraform.


## AWS Login

Configure the aws access

    aws configure --profile cyber-exam

## Create Terraform EKS (AWS) Cluster

Go inside of the terraform folder

    cd terraform

Init Terraform 

    terraform init

Verify the configuration

    terraform plan

Create Infrastructure 

    terraform apply

Expected Output

![image](../assets/terraform-apply.png ':size=100%')

Configure the config for kubectl

    export CLUSTER_NAME=cyber-exam-cluster-E0ErrhvZ
    export CLUSTER_REGION=eu-central-1

Update/Create your config

    aws eks update-kubeconfig --name $CLUSTER_NAME --region $CLUSTER_REGION


View Kube config

    kubectl config view --minify

Verify Connection

    kubectl get svc 


Expected Response
  
    NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   73m
