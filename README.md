# Kubernetes-

1. Kubernetes Concepts : Pods, ReplicaSets, Deployment, Services
2. Networking in Kubernetes
3. Kubernetes Management - kubectl


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
kubectl describe deployments frontend-deployment | grep -i image
kubectl apply -f deployment-defintion.yaml
```

Creating a deployment from the command line without using the kubernetes definition file. 

```
kubectl create deployment httpd-frontend --image=httpd:2.4-alpine
kubectl scale deployment --replicas=3 httpd-frontend

```

### Updates and Rollback in a deployment

Rollout and Versioning

When you first create a deployment, it triggers a rollout, a new roolout creates a new deployment revision, say Revision 1.
In the future, when the application is upgraded, meaning when the container version is updated to a new one, a new rollout is triggered and a new deployment revision is created, say Revision 2. 
This helps us to keep track of the changes made to the deployment and rollback to a previous deployment is needed in the future.

This command gives you the status of the current rollout.
```
kubectl rollout status deployment/myapp-deployment
```

This command gives the revisions and the history of the rollout.
```
kubectl rollout history deployment/myapp-deployment
```

Deployment Strategies : Recreate and Rolling update.

### Update deployments

```
kubectl apply -f deployment-definition.yaml
```

This updates the image of the deployment but it results in having a definition file with a different configuration.
```
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
```

What happens under the hood during a rolling upgrade ?
1. A new replica set is created. 
2. New pods are created in the new replicaset while simultaneously killing old pods in the old replicaset.


### Rollback

```
kubectl rollout undo deployment/myapp-deployment
```

## Services

There are three kinds of services in k8s, namely
1. NodePort
2. ClusterIP
3. Load Balancer

basic commands

```
kubectl create -f service-definition-1.yml
kubectl describe svc myapp-service | grep -i image
```

Another way to create a service without editing the service definition file
```
kubectl expose deployment simple-webapp-deployment --name=webapp-service --target-port=8080 --type=NodePort --port=8080 --dry-run=client -o yaml > svc.yaml

Now edit the svc.yaml file , add the Nodeport

kubectl apply -f svc.yaml
```

NodePort is an external service while clusterIP is an internal service.


## Deploying Microservices Application on Docker

Example code in python on the voting app
```
def get_redis():
    if not hasattr(g,'redis'):
        g.redis= Redis(host='redis',db=0,socket_timeout=5)
    return g.redis   
```     

Note : The -d flag ensures the execution happens in the background.
```
docker run -d --name=redis redis
docker run -d --name=db postgres:9.4
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app

# The above command creates an entry in the etc/hosts file on the voting app container

docker run -d --name=result -p 5001:80 --link db:db result-app
docker run -d --name=worker --link redis:redis --link db:db worker

```

## Deploying Microservices Application on Kubernetes

Implemented this on a simple voting app.

1. Deploy the pods ( voting-app-pod, redis-pod, worker-app-pod, postgres-pod, result-app-pod)
2. Deploy the services (...)

Services establishes the required connectivity between the pods.

## Deploying Microservices Application on Kubernetes with Deployments

Deploying our application just as pods has its own challenges.
Deploying just pods doesn't help in scaling our application, or in updating the application.
We choose deployments over replicasets as replica sets are automatically created with deployments.
Deployment helps us perform rolling updates, revisions, rollback, etc.

1. Create and deploy all the five deployments 
2. Deploy the services

Then we can further scale up or scale down the deployment using the kubectl scale command.
```
kubectl scale deployment voting-app-deploy --replicas=3
```

