# OpenMeter Pod Runtime Watcher Helm Chart

This chart deploys the OpenMeter Pod Runtime Watcher to your cluster.

## Quickstart

```bash
# Set vars
NAMESPACE=default
NAMESPACE_TO_WATCH=""  # set if you want to watch pods in a different namespace than what this is deployed to
OPENMETER_TOKEN=your-token-here
OPENMETER_TOKEN_SECRET_NAME=openmeter-token
INSTALL_NAME=openmeter-pod-runtime-watcher

# Create secret to use as an env var
kubectl create secret generic ${OPENMETER_TOKEN_SECRET_NAME} --from-literal=token=${OPENMETER_TOKEN} --namespace=${NAMESPACE}

# Add the repo and update.
helm repo add openmeter-pod-runtime-watcher https://tizz98.github.io/openmeter-pod-runtime-watcher
helm repo update

# Install the chart.
# Ideally you'd use gitops or something else to manage this rather than a bunch of `--set` flags.
helm upgrade ${INSTALL_NAME} openmeter-pod-runtime-watcher/openmeter-pod-runtime-watcher \
  --namespace=${NAMESPACE} \
  --set "settings.namespace_override=${NAMESPACE_TO_WATCH}" \
  --set "settings.openmeter_token_secret=${OPENMETER_TOKEN_SECRET_NAME}" \
  --atomic \
  --timeout 5m \
  --install
```

### Cleanup

```bash
helm uninstall ${INSTALL_NAME} --namespace=${NAMESPACE}
kubectl delete secret ${OPENMETER_TOKEN_SECRET_NAME} --namespace=${NAMESPACE}
```
