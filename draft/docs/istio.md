## Istio Bookstore

In this demo we'll deploy a live bookstore (distributed application) with [istio](https://istio.io/latest/docs/setup/install/istioctl/) in Qbo.


> Qbo provides a demo repository with demos that can be deployed in Qbo for testing purposes. The demos are deployed using a typing bot. Commands below are provided for informational purposes only as they will be typed for you. All that is required is to start the bot and tap enter to continue. Keep in mind that the commands are not simulated but real commands that are just typed for you. The bot's name is `qbot`


1. Bookstore demo components:
- Live endpoint (Bookstore website with public IP)
- Load balancer
- Distributed application
- Istio injection
- Istio gateway


> Clone demo repo
```bash
git clone https://github.com/alexeadem/qbo-demo
cd qbo-demo
```

> Get Qbo version
```bash
qbo version | jq .version[]?
```

```json
{
    "qbo_cli": "dev-4.3.0-6efb3214c"
}
{
    "docker_api": "1.41",
    "docker_version": "20.10.23",
    "qbo_api": "cloud-dev-4.3.0-6efb3214c",
    "host": "lux.cloud.qbo.io"
}
```
> Get kubernetes nodes via the Qbo API
```bash
qbo get nodes alex | jq .nodes[]?
```
```json
{
    "name": "control-458c3d98.localhost",
    "id": "a88d29340112df19965d499a712040341e00fc35af07b665b8e8052c597d7c1e",
    "image": "kindest/node:v1.28.0",
    "cluster": "alex",
    "state": "ready",
    "address": "172.18.0.3",
    "os": "Debian GNU/Linux 11 (bullseye)",
    "kernel": "6.4.4-200.fc38.x86_64",
    "user": "alex@qbo.io",
    "cluster_id": "47bcd0d2-0de1-4bd7-b630-a515b79e4594"
}
{
    "name": "node-36c8ef09.localhost",
    "id": "81f5b88c6d7146f841e2355c4f79bc49359b3a565e69e8f323fbd5f807ec716b",
    "image": "kindest/node:v1.28.0",
    "cluster": "alex",
    "state": "ready",
    "address": "172.18.0.4",
    "os": "Debian GNU/Linux 11 (bullseye)",
    "kernel": "6.4.4-200.fc38.x86_64",
    "user": "alex@qbo.io",
    "cluster_id": "47bcd0d2-0de1-4bd7-b630-a515b79e4594"
}
{
    "name": "node-2124c4e8.localhost",
    "id": "47c35d9958c80d8d4bee70ff6ed320e02a632634315c4cb74efc2b16e4414638",
    "image": "kindest/node:v1.28.0",
    "cluster": "alex",
    "state": "ready",
    "address": "172.18.0.5",
    "os": "Debian GNU/Linux 11 (bullseye)",
    "kernel": "6.4.4-200.fc38.x86_64",
    "user": "alex@qbo.io",
    "cluster_id": "47bcd0d2-0de1-4bd7-b630-a515b79e4594"
}


> Configure kubeconfig
```bash
qbo get cluster alex -k | jq -r '.output[]?.kubeconfig | select( . != null)' > $HOME/.qbo/alex.cfg
```


> Export the KUBECONFIG to the directory where you saved the cluster configuration above
```bash
export KUBECONFIG=/home/alex/.qbo/alex.cfg
```


> At this point you should able to manage your cluster with `kubectl`
```bash
/usr/bin/kubectl get nodes
NAME STATUS ROLES AGE VERSION
control-458c3d98.localhost  Ready   control-plane   10m     v1.28.0
node-2124c4e8.localhost     Ready   <none>          10m     v1.28.0
node-36c8ef09.localhost     Ready   <none>          10m     v1.28.0
```


> Download and configure Istio
```bash
cd ~
```
```bash
curl -L https://istio.io/downloadIstio | sh -
```
```bash
cd $(ls -dt ~/istio* | head -1)
```
```bash
ISTIOCTL=$PWD/bin/istioctl
```

> Install Istio
```bash
$ISTIOCTL install --set profile=demo -y --set meshConfig.defaultConfig.tracing.zipkin.address=splunk-otel-collector.istio-system.svc.cluster.local:9411
```
```bash
kubectl label namespace default istio-injection=enabled
```
> Install bookstore
```bash
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```


> Setup an Istio gateway to access the bookstore
```bash
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```


> Get the public IP address of the bookstore. Save the IP address from the command output below as we'll use it in the next step.
```bash
kubectl get svc -n istio-system -o json | jq -r '.items[].spec.externalIPs[0] | select ( . != null)'
```
> Access the bookstore

> Look for the load balancer IP address on port 80 using the Qbo web UI (Under load balancers) or user the output of the command above
>
> Open a browser page by adding `/productpage`


http://192.168.1.201/productpage


