# Kubernetes Pi-hole service
Creates a new service running [Pi-hole](https://pi-hole.net/) DNS/DHCP server and includes the necessary configuration for enabling Kubernetes cluster DNS server to use Pi-hole as a [upstream nameserver](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#configure-stub-domain-and-upstream-dns-servers).
## Setup
Edit the following:
- [`pihole-pv.yaml`](pihole-pv.yaml) and configure it to a valid volume for your cluster.
- [`pihole-webpassword-secret.yaml`](pihole-webpassword-secret.yaml) and set a valid secret, otherwise `admin` is the default password.

## Deployment
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
