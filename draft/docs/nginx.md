## Nginx Ingress Coffee & Tea

In this demo we'll deploy a sample coffee & tea application with [kubernetes ingress controller](https://github.com/kubernetes/ingress-nginx) in Qbo.

> Qbo provides a demo repository with demos that can be deployed in Qbo for testing purposes. The demos are deployed using a typing bot. Commands below are provided for informational purposes only as they will be typed for you. All that is required is to start the bot and tap enter to continue. Keep in mind that the commands are not simulated but real commands that are just typed for you. The bot's name is `qbot`


1. Cofee & Tea demo components:
- Live endpoint 
- Load balancer
- Nignx ingress controller
- Application ingress
- Application secret


> Start the coffee bot
```
./qbot
>>> ./qbot {bookstore | coffee | kubeconfig}        -- Demo to run
./qbot coffee
```

> Verify you can connect to the API

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
  "qbo_api": "cloud-dev-4.3.0-76da24342",
  "host": "lux.cloud.qbo.io"
}
```

```bash
qbo get nodes alex | jq .nodes[]?
```
```json
{
  "name": "control-2b4e4848.localhost",
  "id": "0c20025cd1947252881eaefbbb2ebbd31893c3263fd28826b766fc3ce4fecd7d",
  "image": "kindest/node:v1.28.0",
  "cluster": "alex",
  "state": "ready",
  "address": "172.18.0.3",
  "os": "Debian GNU/Linux 11 (bullseye)",
  "kernel": "6.4.4-200.fc38.x86_64",
  "user": "alex@qbo.io",
  "cluster_id": "0a3db628-d9d3-46a5-81d8-281e727d8e6c"
}
{
  "name": "node-69fad8d5.localhost",
  "id": "bce293a5bad56200f2c1574919b0a67ce9761e0ee3071dc7530c02fc12f3ae79",
  "image": "kindest/node:v1.28.0",
  "cluster": "alex",
  "state": "ready",
  "address": "172.18.0.4",
  "os": "Debian GNU/Linux 11 (bullseye)",
  "kernel": "6.4.4-200.fc38.x86_64",
  "user": "alex@qbo.io",
  "cluster_id": "0a3db628-d9d3-46a5-81d8-281e727d8e6c"
}
{
  "name": "node-c0dbf9c8.localhost",
  "id": "2b2c2e39140d1bc61577f16907c93f0703fcc45352433f18578701e3e4c3ec19",
  "image": "kindest/node:v1.28.0",
  "cluster": "alex",
  "state": "ready",
  "address": "172.18.0.5",
  "os": "Debian GNU/Linux 11 (bullseye)",
  "kernel": "6.4.4-200.fc38.x86_64",
  "user": "alex@qbo.io",
  "cluster_id": "0a3db628-d9d3-46a5-81d8-281e727d8e6c"
}
```

> Get kubeconfig

```bash
qbo get cluster alex -k | jq -r '.output[]?.kubeconfig | select( . != null)' > /home/alex/.qbo/alex.cfg
export KUBECONFIG=/home/alex/.qbo/alex.cfg
 /usr/bin/kubectl get nodes
NAME                         STATUS   ROLES           AGE   VERSION
control-2b4e4848.localhost   Ready    control-plane   17m   v1.28.0
node-69fad8d5.localhost      Ready    <none>          16m   v1.28.0
node-c0dbf9c8.localhost      Ready    <none>          16m   v1.28.0
```

> Deploy the ingress controller 

```bash
/usr/bin/kubectl apply -f /home/alex/qbo-demo/coffee/ingress-nginx/deploy.yaml
```

> Deploy the application
```bash
/usr/bin/kubectl apply -f /home/alex/qbo-demo/coffee/ingress-nginx/cafe
```

> Get the load balancer external IP
```bash
 /usr/bin/kubectl get svc -n ingress-nginx -o json | jq -r '.items[].spec.externalIPs[0] | select ( . != null)'
```

> Test access to the application. If the response code `200` the deployment was successful.
```bash
/usr/bin/curl -k -m 3 --write-out '%{http_code}' --silent --output /dev/null -H 'host: cafe.example.com'  https://192.168.1.201/tea
200
```

