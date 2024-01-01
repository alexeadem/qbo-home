# Quick start
> Create a Kubernetes cluster with `qbo` and access it via kubectl
## Add cluster
> Type  your name in the autocomplete section and select the Kubernetes version

![add cluster](img/add_cluster.gif)

> Get `qbo` version
```bash
qbo version | jq .version[]?
```
> Get kubernetes nodes with `qbo` CLI

```bash
qbo get nodes $USER | jq .nodes[]?
```
## Access cluster
> Get kubeconfig with `qbo` CLI

![get kubeconfig](img/get_kubeconfig.gif)

```bash
qbo get cluster $USER -k | jq -r '.output[]?.kubeconfig | select( . != null)' > $HOME/.qbo/$USER.cfg
```
> export kubeconfig
```bash
export KUBECONFIG=$HOME/.qbo/$USER.cfg
```
> Get nodes with kubectl
```bash
kubectl get nodes
```
