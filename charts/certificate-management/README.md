# Certificate Management Service
This chart deploys the certificate management service, which is used to manage the certificates on the ACP's API and ingress. The service interfaces with ACME-capable certificate issuer systems to automatically issue and rotate certificates as needed.

## Required Values
This chart requires a few inputs to configure the service, see below for more information:

```yaml
# Define this to trigger deployment of the service
certificateManagement:
  # If a certificate should be issued for the API endpoints, set this to true.
  issueAPICert: true
  # If a certificate should be issued for the ingress, set this to true.
  issueIngressCert: true
  # The base DNS zone of the cluster.
  clusterBaseDNSZone: acp.site1.yourcompany.com
  # Define secrets needed to authenticate to ACME servers, or to resolvers, as needed.
  # Each list element will trigger the creation of a new secret.
  secrets:
    - name: zero-ssl-eabsecret
      stringData:
        secret: yoursecretdatahere
    - name: cloudflare-api-token-secret
      stringData:
        api-token: yoursecretdatahere
  # Define the issuer here.
  # For a full list of supported issuers, visit the docs: https://cert-manager.io/docs/configuration/issuers/
  # The .spec field will be deployed as stated in your values.yaml file.
  issuer:
    name: zerossl-production
    spec:
      acme:
        server: https://acme.zerossl.com/v2/DV90
        email: your@email.here
        privateKeySecretRef:
          name: zerossl-prod
        externalAccountBinding:
          keyID: yourEABkeyhere
          # Ensure this matches the correct secret created above.
          keySecretRef:
            name: zero-ssl-eabsecret
            key: secret
          keyAlgorithm: HS256
        # Define solvers to be used when issuing certificates
        # This will be copied into the configuration as stated in your values.yaml file
        # For more info, review the solvers (http01, dns01) in the docs: https://cert-manager.io/docs/configuration/acme/
        solvers:
          - dns01:
              cloudflare:
                email: your@email.here
                apiTokenSecretRef:
                  name: cloudflare-api-token-secret
                  key: api-token
            selector:
              dnsZones:
                  - yourdomain.here
```

## Service Deployment
This service can be deployed individually, however, it's recommended to deploy it alongside other ACP services using the acp-standard-services parent application.

To deploy it individually, use the following command:
```
helm install -f /path/to/values.yaml certificate-management-service charts/certificate-management/
```