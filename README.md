# Azure-K8s-ssl-certificates

## Project Summary 
This project configures AKS to leverage LetsEncrypt.org and automatically obtain an SSL certificate for your domain. The certificate will be installed on Application Gateway, performing SSL/TLS termination for your AKS cluster. The setup described here uses the cert-manager Kubernetes add-on, which automates the creation and management of certificates.

## Prerequisites:
Before you get started, make sure you have the following prerequisites set:

- An Azure Kubernetes Service (AKS) cluster.
- A custom domain that you own, with DNS control to point to your AKS cluster's public IP.
- Azure CLI installed and authenticated.
- kubectl (Kubernetes command-line tool) installed and configured to manage your AKS cluster.
- Ensure that your web application is deployed to your AKS cluster and that it's accessible via a public IP

## Project Scope
### 1.  Install and Configure Helm
Helm is a package manager for Kubernetes. You'll use it to install Cert-Manager and Nginx Ingress Controller. If you haven't already, install Helm and Tiller
- ``` helm init ``` -
### 2.  Install Cert-Manager
Cert-Manager is a Kubernetes-native certificate management controller that automates the issuance and renewal of SSL certificates. Install it using Helm:
