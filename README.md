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
