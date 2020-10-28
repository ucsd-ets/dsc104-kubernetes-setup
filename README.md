# dsc104-kubernetes-setup

## Setup

`git clone https://github.com/ucsd-ets/dsc104-kubernetes-setup.git`

Watch this repository for updates. Please don't modify launch template without checking with ETS.

## Redis

SSH into dsmlp-login.ucsd.edu

`$ cd ~/dsc104-kubernetes-setup`

`$ kubectl create -f launch/redis.yaml`

`$ kubectl get pods`

Wait for the pod is in a `READY` state. Go to datahub.ucsd.edu and spawn a DSC104 notebook.

```python
import redis
r = redis.Redis(host='my-redis', port=6379, db=0)
r.set('foo', 'bar')
```

To delete the pod, on dsmlp-login: 

`$ kubectl delete -f launch/redis.yaml`

## Postgres

SSH into dsmlp-login.ucsd.edu

`$ cd ~/dsc104-kubernetes-setup`

`$ kubectl create -f launch/postgresql-pod.yaml`

`$ kubectl get pods`

Wait for the pod is in a `READY` state. Go to datahub.ucsd.edu and spawn a DSC104 notebook.

```python
import psycopg2
conn = psycopg2.connect(
    host="my-postgres",
    database="postgres",
    user="postgres",
    password="password"
)
```

To delete the resources: 

`$ kubectl delete -f launch/postgresql-pod.yaml`


## Neo4j

SSH into dsmlp-login.ucsd.edu

`$ cd ~/dsc104-kubernetes-setup`

`$ kubectl create -f launch/neo4j-pod.yaml`

`$ kubectl get pods`

Wait for the pod is in a `READY` state. Go to datahub.ucsd.edu and spawn a DSC104 notebook.

```python
from neo4j import GraphDatabase
uri = 'bolt://my-neo4j:7687'
driver = GraphDatabase.driver(uri)
```

To delete the resources: 

`$ kubectl delete -f launch/neo4j-pod.yaml`


## Mongo

SSH into dsmlp-login.ucsd.edu

`$ cd ~/dsc104-kubernetes-setup`

`$ kubectl create -f launch/mongo-pod.yaml`

`$ kubectl get pods`

Wait for the pod is in a `READY` state. Go to datahub.ucsd.edu and spawn a DSC104 notebook.

```python
from pymongo import MongoClient
client = MongoClient("mongodb://my-mongo", 27017, username='admin', password='password')
db = client.admin
```

To delete the resources: 

`$ kubectl delete -f launch/mongo-pod.yaml`

## Cassandra

SSH into dsmlp-login.ucsd.edu

`$ cd ~/dsc104-kubernetes-setup`

Create service first

`$ kubectl create -f cassandra-svc.yaml`

Create statefulset of 3 pods

`$ sed "s/{{NAMESPACE}}/$USER/g" launch/cassandra-statefulset.yaml | kubectl create -f - `

`$ kubectl get statefulset`

Wait for the statefulset `READY` state to be `3/3`. It takes 5-10 minutes to complete. Use `kubectl get events` to monitor pod launch status. Use `kubectl logs cassandra-0 -f` to watch the individual pod logs. Go to datahub.ucsd.edu and spawn a DSC104 notebook.

```python
from cassandra.cluster import Cluster
cluster = Cluster(['cassandra'])
session = cluster.connect()
```

To delete the resources: 

`$ kubectl delete -f launch/cassandra-statefulset.yaml`
