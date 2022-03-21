# ElasticSearch On Kubernetes

## Deploy elasticsearch on minikube node

### 1. Run Minikube

Elasticsearch requires more cpu and memory \
Run the following command.
```bash
minikube start --cpus=3 --memory=5g
```

### 2. Create resources


```bash
kubectl apply -f namespace.yml
kubectl apply -f storeageclass.yml
kubectl apply -f nodeport_svc.yml
kubectl apply -f headless_svc.yml
kubectl apply -f elastic_statefulset.yml
```

### 3. Check all pods are in running status

We need to define the namespace that we are using

```bash
kubectl get pods -n es-namespace
```

**Output will look like**

```text
NAME             READY   STATUS    RESTARTS   AGE
node-cluster-0   1/1     Running   0          45s
node-cluster-1   1/1     Running   0          30s
```
### 4. To check the API data use 

either do a port forward
```
kubectl port-forward master-0 9200:9200
```
**Output will look like:**

```
Forwarding from 127.0.0.1:9200 -> 9200
Forwarding from [::1]:9200 -> 9200


```

### 5. Check on your broswer

```
localhost:9200/_cat/nodes?v=true

```
or use the NodePort configuration 
NodeIP:NodePort


```
minikube ip

```
output


```
192.168.49.2

```
on browser
```
192.168.49.2:31000/_cat/nodes?v=true

```
**It should lool like this if all are good**

```
ip         heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
172.17.0.4           25          88   6    0.71    1.07     1.35 mdi       -      node-cluster-1
172.17.0.3           25          88   6    0.71    1.07     1.35 mdi       *      node-cluster-0
```
**Similarly to check health**

on browser
```
192.168.49.2:31000/_cat/health?v=true

```
if all good output will be like this with green state
```
epoch      timestamp cluster     status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1647881327 16:48:47  clusterlogs green           2         2      0   0    0    0        0             0                  -                100.0%
```