# Add Vector with NVIDIA GPU

## Requirements
* [CLI](cli.md) Configuration 
* [NVIDIA GPU Operator verison v23.9.1](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/platform-support.html#operator-platform-support)
* [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
* [NVIDIA Driver 535.129.03](https://www.nvidia.com/download/driverResults.aspx/213194/en-us/)

## qbot
```
git clone https://github.com/alexeadem/qbo-demo
cd qbo-demo
./qbot nvidia
```

## Clreate K8s Cluster

> For this tutorial we are using `NVIDIA` as our cluster name
```bash
export NAME=nvidia
```

> Get Qbo versiont to make sure we have access to the API
```bash
qbo version | jq .version[]?
```

> Add a K8s cluster with default image v1.27.3
```bash
qbo add cluster $NAME | jq
```

> Get nodes information using the qbo API

```bash
qbo get nodes $NAME | jq .nodes[]?
```

> Configure kubectl 
```bash
export KUBECONFIG=$HOME/.qbo/$NAME.cfg
```

> Get nodes with kubectl
```bash
kubectl get nodes
``` 


## Nvidia GPU Operatator
> Deploy Nvidia GPU Operatator helm chart

```bash
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia || true
helm repo update
helm install --wait --generate-name -n gpu-operator --create-namespace nvidia/gpu-operator --set driver.enabled=false

```


## Vector Add Application
```
cat cuda/vectoradd.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cuda-vectoradd
spec:
  restartPolicy: OnFailure
  containers:
  - name: cuda-vectoradd
    image: "nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda11.7.1-ubuntu20.04"
    resources:
      limits:
        nvidia.com/gpu: 1

```

> Deploy vectore add application 
```bash
kubectl apply -f cuda/vectoradd.yaml
```

> Verify that operation was successful
```bash
kubectl logs cuda-vectoradd
```

