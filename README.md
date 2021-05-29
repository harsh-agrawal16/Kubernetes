# Kubernetes-

## Pods:

version : v1

Some basic commands for dealing with pods 

```
kubectl create -f pod-definition.yaml
kubectl get pods
kubectl describe pod my-app-pod
kubectl apply -f pod-definition.yaml
kubectl delete pod my-app-pod
```

## ReplicaSets:

version : apps/v1

Some basic commands for dealing with replicasets

```
kubectl create -f replicaset.yaml
kubectl get replicasets
kubectl describe replicaset my-app-replicaset
```

The below command opens up the running configuration of the replicaset in a text editor.

```
kubectl edit replicaset my-app-replicaset
```