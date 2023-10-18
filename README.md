# Azure-K8s-ssl-certificates

## Project Summary 
This project configures AKS to leverage LetsEncrypt.org and automatically obtain an SSL certificate for your domain. The certificate will be installed on Application Gateway, performing SSL/TLS termination for your AKS cluster. The setup described here uses the cert-manager Kubernetes add-on, which automates the creation and management of certificates.

## Project Scope

- Fix not properly secured S3 buckets: Lambda functions can be used to monitor S3 bucket access and enforce security policies, such as preventing public access, 
encrypting data at rest, and logging all access activity.

## High Level Design

![ssl design](<azure vm and web server.PNG>)
