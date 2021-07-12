# Let's Encrypt with 

Tested on:

- OpenShift 4.7.19
- Cert-Manager 1.4.0

Create a new project:

`oc new-project demo-letsencrypt`

Install the cert-manager operator: Operator > Operator Hub > cert-manager community

Create a cluster issuer (change the email address before):

`oc apply -f letsencrypt.yaml`

Deploy your new app:

`oc apply -f deployment.yaml`

Create the service:

`oc apply -f service.yaml`

Expose with a new ingress route:

`oc apply -f ingress.yaml`

