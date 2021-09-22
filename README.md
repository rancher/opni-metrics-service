# Opni-metric-anomaly-detector

## Installation
#### Prerequisites:
Kubernetes cluster running with Prometheus and Grafana and Opni installed.
Rancher user can install Prometheus and Grafana by [enable Rancher monitoring](https://rancher.com/docs/rancher/v2.5/en/monitoring-alerting/guides/enable-monitoring/).


#### Build and Run
To build the metric-anomaly-detection Docker image and then push the image:
```
docker build -t opni-metric-forecasting-service ./
docker tag opni-metric-forecasting-service-service [ACCOUNT_NAME]/opni-metric-forecasting-service
docker push [ACCOUNT_NAME]/opni-metric-forecasting-service

```

then run
```
kubectl apply -f metric_forecasting.yaml
```
Make sure to update the metric_forecasting.yaml file to point to the correct image path!

#### Import Grafana Dashboard
1. Navigate to Grafana and login. For Rancher monitoring user, the username/password is `admin/prom-operator`.
2. [Import dashboard](https://grafana.com/docs/grafana/latest/dashboards/export-import/#import-dashboard) and upload the json file `grafana-dashboard.json` in this repo.

## Development (To be updated)
How to run the development version of opni in your cluster:
- Clone rancher/opni
- Switch to the branch `nats-metrics-listener`
- Download Tilt (https://tilt.dev)
- If you need a cluster, use the script `hack/create-k3d-cluster.sh`. Otherwise, if you are using an existing cluster, you must perform the following additional steps:
  - Create a publically available repo in your docker hub account called "opni-manager"
  - Insert the following lines into the opni Tiltfile:
    - Above the call to `docker_build_with_restart`: `default_registry('docker.io/your-user-name')`
    - Near the top of the file under the existing call to allow_k8s_contexts: `allow_k8s_contexts('your-context-name')`
- Run `tilt up`

## Contributing
We use `pre-commit` for formatting auto-linting and checking import. Please refer to [installation](https://pre-commit.com/#installation) to install the pre-commit or run `pip install pre-commit`. Then you can activate it for this repo. Once it's activated, it will lint and format the code when you make a git commit. It makes changes in place. If the code is modified during the reformatting, it needs to be staged manually.

```
# Install
pip install pre-commit

# Install the git commit hook to invoke automatically every time you do "git commit"
pre-commit install

# (Optional)Manually run against all files
pre-commit run --all-files
```
