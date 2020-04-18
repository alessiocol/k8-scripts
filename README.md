# Kubernetes system and services
## Base configuration
This is a pre-requisite for all other services that are part of this repository.
### Deployment
```
kubectl apply -f services-namespace.yml
```
## Pi-hole

### Setup
Edit the following:
- `pihole\pihole-webpassword-secret.yaml` and set a valid secret.

### Deployment
Pi-hole:
```sh
cd pihole
kubectl -n services apply -f pihole-pv.yaml,pihole-pvc.yaml,pihole-deployment.yaml,pihole-service.yaml,pihole-webpassword-secret.yaml
```
Make sure containers point to the new DNS server:
```sh
cd base
kubectl apply -f kube-dns-upstream.yml
kubectl -n kube-system rollout restart deployment coredns
```
You might need to restart existing pods or services to make sure the new DNS settings are proagated to the containers.

**:warning:NOTE:warning::** The external IP address of the Pi-hole service, that is then used as DNS upstream by the Kubernetes DNS service, is set in `externalIPs` in `pihole-service.yaml`. Make sure the exact same IP address is used in `upstreamNameservers` in `kube-dns-upstream.yml`!

## DDNS update
### Setup
Edit the following:
- `ddns-update\godaddy-api-secret.yaml`: set a valid secret.
- `ddns-update\ddns-update-pod.yaml`: set correct environmental variables for the GoDaddy domain.

### Deployment
```sh
cd ddns-update
kubectl -n services apply -f godaddy-api-secret.yaml
kubectl -n services apply -f dns-update-pod.yaml
```