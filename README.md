# Let's Encrypt with Cert Manager on OpenShift 4

Tested on:

- OpenShift 4.7.19
- Cert-Manager 1.4.0

Create a new project:

`oc new-project demo-letsencrypt`

Install the cert-manager operator: Operator > Operator Hub > cert-manager community

- Edit `letsencrypt.yaml` and replace the email address
- Edit `ingress.yaml` and replace the URL

Create a cluster issuer:

`oc apply -f letsencrypt.yaml`

Deploy your new app:

`oc apply -f deployment.yaml`

Create the service:

`oc apply -f service.yaml`

Expose with a new ingress route:

`oc apply -f ingress.yaml`

The operator will spin up a pod that will take care of validations with Let's Encrypt.

You can keep an eye on the certicate generation:

```
$ oc -n demo-letsencrypt describe certificate
Name:         demo-tls
Namespace:    demo-letsencrypt
Labels:       <none>
Annotations:  <none>
API Version:  cert-manager.io/v1
Kind:         Certificate
Metadata:
  Creation Timestamp:  2021-07-12T11:38:23Z
  Generation:          1
  Managed Fields:
    API Version:  cert-manager.io/v1
[...]
    Fields Type:  FieldsV1
Status:
  Conditions:
    Last Transition Time:  2021-07-12T11:38:26Z
    Message:               Certificate is up to date and has not expired
    Observed Generation:   1
    Reason:                Ready
    Status:                True
    Type:                  Ready
  Not After:               2021-10-10T10:38:24Z
  Not Before:              2021-07-12T10:38:25Z
  Renewal Time:            2021-09-10T10:38:24Z
  Revision:                1
Events:
  Type    Reason     Age   From          Message
  ----    ------     ----  ----          -------
  Normal  Issuing    64m   cert-manager  Issuing certificate as Secret does not exist
  Normal  Generated  64m   cert-manager  Stored new private key in temporary Secret resource "demo-tls-9w5qq"
  Normal  Requested  64m   cert-manager  Created new CertificateRequest resource "demo-tls-9l9x5"
  Normal  Issuing    64m   cert-manager  The certificate has been successfully issued
```

When you see `The certificate has been successfully issued` you can browse to your URL.
