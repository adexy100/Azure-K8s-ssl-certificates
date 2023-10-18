# Azure-K8s-ssl-certificates

## Project Summary 
This project configures AKS to leverage LetsEncrypt.org and automatically obtain an SSL certificate for your domain. The certificate will be installed on Application Gateway, performing SSL/TLS termination for your AKS cluster. The setup described here uses the cert-manager Kubernetes add-on, which automates the creation and management of certificates.

## Prerequisites:
Before you get started, make sure you have the following prerequisites set:

- Azure subscription and CLI installed.
- You need an existing Kubernetes cluster running on Azure (AKS).
- You will need an existing application running and have a “Service” for your own application, making it available to other pods in the cluster.
- Install the command-line tool “kubectl” on your machine to execute commands on a Kubernetes cluster. Configure kubectl to talk to your cluster.
- I also assume you already have a DNS name or how to set up a temporary one provided by Azure (test.westeurope.cloudapp.azure.com for example).

## Project Scope
### 1. Install an “Ingress Controller”
- You will need an existing application running and have a “Service” for your own application, making it available to other pods in the cluster.
- Install the command-line tool “kubectl” on your machine to execute commands on a Kubernetes cluster. Configure kubectl to talk to your cluster.
- I also assume you already have a DNS name or how to set up a temporary one provided by Azure (test.westeurope.cloudapp.azure.com for example).

## High Level Design

![ssl design](<azure vm and web server.PNG>)
