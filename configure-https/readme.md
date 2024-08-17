## HTTPS Configuration with Cert-Manager
To secure traffic to our application, we use Cert-Manager for managing TLS certificates. This setup ensures that our application is accessible over HTTPS.

### Configuration

**ClusterIssuer:** Configures Cert-Manager to use Let's Encrypt for issuing certificates. View ClusterIssuer YAML

**Certificate:** Defines the certificate to be issued for our domain. View Certificate YAML

``` yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-issuer  # Name of the ClusterIssuer, no need to change 
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory  # ACME server URL; usually remains the same for Let's Encrypt
    privateKeySecretRef:
      name: letsencrypt-private-key  # Name of the secret that holds the private key; no change needed
    solvers:
    - http01:
        ingress:
          ingressClassName: nginx  # # Keep this as 'nginx' if using the Nginx Ingress Controller; change if using a different ingress class
---

apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: dev-portfolio-certificate  # Name of the Certificate; change if you want 
spec:
  secretName: portfolio-tls-secret  # Name of the Kubernetes secret where the certificate will be stored; change if you want ; also used the same secrets in ingress.yaml
  issuerRef:
    name: letsencrypt-issuer                     # Reference Name of the ClusterIssuer
    kind: ClusterIssuer
  commonName: dev-portfolio.34.46.239.84.nip.io  #   change to your domain name
  dnsNames:
  - dev-portfolio.34.46.239.84.nip.io            #   change to your domain name
```




