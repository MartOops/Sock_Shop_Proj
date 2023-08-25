
# Deployment of a microservice based architecture on Kubernetes using a clear IaaC approach.

This project provides a solution to deploy [Socks Shop](https://microservices-demo.github.io/), an Open Source microservice based application on Kubernetes with a clear IaaC (Infrastructure as Code) approach implemented with terraform.
## Project Todo
The following were provisioned using an infrastructure as Code approach
* Prometheus/Grafana as a monitoring tool
* EFK as a logging tool
* AWS as IaaS provider
* Let's Encrypt for SSL cert
## Prerequisites
* [AWS Account](https://portal.aws.amazon.com/billing/signup#/start/email)
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) configured
* [Kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Terraform](https://developer.hashicorp.com/terraform/downloads) installed
* Domain Name 
## Directory Structure
The structure below refers to the required files needed and to be understood before deploying.

* Socks Shop Manifests

```bash
deploy
 ┗ kubernetes
 ┃ ┗ manifests
 ┃ ┃ ┣ 00-sock-shop-ns.yaml
 ┃ ┃ ┣ 01-carts-dep.yaml
 ┃ ┃ ┣ 02-carts-svc.yml
 ┃ ┃ ┣ 03-carts-db-dep.yaml
 ┃ ┃ ┣ 04-carts-db-svc.yaml
 ┃ ┃ ┣ 05-catalogue-dep.yaml
 ┃ ┃ ┣ 06-catalogue-svc.yaml
 ┃ ┃ ┣ 07-catalogue-db-dep.yaml
 ┃ ┃ ┣ 08-catalogue-db-svc.yaml
 ┃ ┃ ┣ 09-front-end-dep.yaml
 ┃ ┃ ┣ 10-front-end-svc.yaml
 ┃ ┃ ┣ 11-orders-dep.yaml
 ┃ ┃ ┣ 12-orders-svc.yaml
 ┃ ┃ ┣ 13-orders-db-dep.yaml
 ┃ ┃ ┣ 14-orders-db-svc.yaml
 ┃ ┃ ┣ 15-payment-dep.yaml
 ┃ ┃ ┣ 16-payment-svc.yaml
 ┃ ┃ ┣ 17-queue-master-dep.yaml
 ┃ ┃ ┣ 18-queue-master-svc.yaml
 ┃ ┃ ┣ 19-rabbitmq-dep.yaml
 ┃ ┃ ┣ 20-rabbitmq-svc.yaml
 ┃ ┃ ┣ 21-session-db-dep.yaml
 ┃ ┃ ┣ 22-session-db-svc.yaml
 ┃ ┃ ┣ 23-shipping-dep.yaml
 ┃ ┃ ┣ 24-shipping-svc.yaml
 ┃ ┃ ┣ 25-user-dep.yaml
 ┃ ┃ ┣ 26-user-svc.yaml
 ┃ ┃ ┣ 27-user-db-dep.yaml
 ┃ ┃ ┗ 28-user-db-svc.yaml

```
The manifests for the microservice based architecture to be deployed.
* Terraform files used to provision the Socks Shop app.
```bash
tf-files
 ┣ domain.tf
 ┣ grafana.tf
 ┣ logging.tf
 ┣ main.tf
 ┣ nginx_ingress.tf
 ┣ output.tf
 ┣ prometheus.tf
 ┣ sock_shop.tf
 ┣ tls.tf
 ┗ var.tf
```

## Deployment

To deploy this project make the changes to the file with the neccesary details.

* Ingress.yaml file
```bash
files
 ┣ ingress.yaml
 Edit the ingress.yaml block to your Domain name <domainname>
  - hosts:
    - microservice.<domainname>
    secretName: acme-crt-prod
  rules:
  - host: microservice.<domainname>
```
* Let's Encrypt file (cert-files)
```bash
cert-files
 ┣ prodcert.yaml
 ┗ prodissuer.yaml
 Edit the blocks below in the file.
 # Email address used for ACME registration in prodissuer.yaml  
    email: <enteremail>
 # Edit <domainname> to Domain name
  dnsNames:
  - microservice.<domainname>
```
* Deploy
```bash
cd tf-files
terraform apply -auto-approve
```
The `terraform apply` command provisions the **EKS Cluster** and the resources in the _Task List_ will be deployed to the Cluster.

To access the Cluster and check the resources created.
```bash
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
```


## Domain Name
```bash
In the directory below, route53 zone and record was created
tf-files
 ┣ domain.tf
```
Copy the Hosted Zones from the AWS console to the Domain Registrar of your Domain Name immediately it is created.
## Access Application
Upon successful deployment the app can be visited with the URL in the browser.
`https://microservice.<domainname>/`
## Feedback

If you have any feedback,  please reach out to me oluwafemimartins1234@gmail.com

