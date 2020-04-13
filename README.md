# K8 services
## Pi-hole
Deployment:
```
kubectl apply -f services-namespace.yml
cd pihole
kubectl -n services apply -f pihole-pv.yaml,pihole-pvc.yaml,pihole-deployment.yaml,pihole-service.yaml,pihole-webpassword-secret.yaml
```