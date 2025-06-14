> [!NOTE] Run the following commands from the root directory.

Install **Operator Lifecycle Manager** (OLM) onto the cluster using Homebrew:

<https://sdk.operatorframework.io/docs/installation/#install-from-github-release>

```bash
operator-sdk olm install
```

Create Doppler secret in the Avelin bootstrap folder:

```bash
kubectl create secret generic \
 doppler-avelin-token \
  --namespace=avelin \
  --from-literal dopplerToken=$(doppler configs tokens create --project avelin --config prd doppler-auth-token --plain) \
  --dry-run=client \
  -o yaml > ./gitops/cluster-apps/avelin/00-bootstrap/doppler-avelin-secret.yaml
```

Bootstrap the cluster by installing ArgoCD:

```bash
kubectl apply -k bootstrap/init
# Wait for the CRDs to be created (i.e. 1-2 minutes), then run the command again
kubectl apply -k bootstrap/init
```

Boostrap the rest of the cluster, now that ArgoCD is installed:

```bash
kubectl apply -k bootstrap
```
