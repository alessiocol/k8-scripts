# Kubernetes Deployment for DDNS updates
Creates a new Kubernetes [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) for managing dynamic DNS updates.

Uses the [alcol/tiny-godaddy-ddns](https://hub.docker.com/r/alcol/tiny-godaddy-ddns) Docker image. Please refer to the documentation of the Docker image for configuration and setup.
## Setup
Edit the following:
- [`godaddy-api-secret.yaml`](godaddy-api-secret.yaml): set a valid secret.
- [`ddns-update-deployment.yaml`](ddns-update-deployment.yaml): set correct environmental variables for the GoDaddy domain.

## Deployment
```sh
cd ddns-update
kubectl -n services apply -f godaddy-api-secret.yaml
kubectl -n services apply -f ddns-update-deployment.yaml
```