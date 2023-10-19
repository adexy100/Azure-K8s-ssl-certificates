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
```
helm init
```
### 2.  Install Cert-Manager
Cert-Manager is a Kubernetes-native certificate management controller that automates the issuance and renewal of SSL certificates. Install it using Helm:
```
# Label the ingress-basic namespace to disable resource validation
kubectl label namespace ingress-basic cert-manager.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager jetstack/cert-manager \
  --namespace ingress-basic \
  --version v1.8.2 \
  --set installCRDs=true
```
### 2.  Create a ClusterIssuer:
- Create a ClusterIssuer that tells Cert-Manager to use Let's Encrypt. Create a YAML file (e.g., cluster-issuer.yaml) with the following content:
```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: dkalyanreddy@gmail.com
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt
    solvers:
      - http01:
          ingress:
            class: nginx
```
### 2.  Create an Ingress Resource:
Create an Ingress resource that defines how traffic should be routed to your application and where the SSL certificate should be applied. Create a YAML file (e.g., ingress.yaml) with the following content:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-ssl
  annotations:
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: letsencrypt    
spec:
  rules:
    - host: sapp1.kubeoncloud.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app1-nginx-clusterip-service
                port: 
                  number: 80
    - host: sapp2.kubeoncloud.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app2-nginx-clusterip-service
                port: 
                  number: 80                         
  tls:
  - hosts:
    - sapp1.kubeoncloud.com
    secretName: sapp1-kubeoncloud-secret           
  - hosts:
    - sapp2.kubeoncloud.com
    secretName: sapp2-kubeoncloud-secret

```
- Apply this configuration to your cluster:
  ```
  kubectl apply -f ingress.yaml
  ```

