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
kubectl delete replicaset replicaset-1
```

The below command opens up the running configuration of the replicaset in a text editor.

```
kubectl edit replicaset my-app-replicaset
```

We can also scale the number of replicas without going to the configuration file.

```
kubectl scale replicaset my-app-replicaset --replicas=3
```

## Deployments

Deployments come higher than the replcasets in hierarchy.
When a deployment object is created , a replicaset and the specified number of pods (replicas) are also created simultaneously. 

Basic commands to deal with deployments

```
kubectl create -f deployment deployment-defintion.yaml
kubectl get deployments 
```