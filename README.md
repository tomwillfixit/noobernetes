# noobernetes

Getting started with Kubernetes for absolute noobs (Approximately 5 minutes end-to-end)

This walkthrough uses [microk8s](https://microk8s.io/) which is a [Snap](https://snapcraft.io/) based install of Kubernetes.

We will use [Kompose](https://github.com/kubernetes/kompose) to translate a simple docker-compose file into Kubernetes resources. This requirement will be updated in future with the arrival of [Compose on Kubernetes](https://github.com/docker/compose-on-kubernetes), announced at [DockerCon EU 2018](https://medium.com/@thomas.shaw78/dockercon-eu-2018-review-a5c6cd2344bf). 

## Why microk8s ?

Microk8s is easy to install, installs on 42 flavours of Linux, easy to remove, easy to install different versions of Kubernetes and doesn't require messing around with Virtualbox or minikube.

## Walkthrough

This walkthrough takes a few minutes and is very basic but will cover the following :

- Install microk8s
- Install Kompose
- Translate docker-compose file to Kubernetes resources
- Start the Dogs vs Cats app
- Test it works
- Remove microk8s
- Remove Kompose

### Install microk8s

```
# snap install microk8s --classic

# microk8s.status --wait-ready

Turn on/enable DNS and Storage services in microk8s :

# microk8s.enable storage dns

These are required for services which need to create volumes on the host and DNS is used for service discovery.
```

### Install Kompose

```
# curl -L https://github.com/kubernetes/kompose/releases/download/v1.17.0/kompose-linux-amd64 -o /usr/bin/kompose && chmod 777 /usr/bin/kompose

```
### Translate docker-compose file to Kubernetes resources 

```
# kompose convert --file compose/docker-compose.yml

```
### Start the Dogs vs Cats app 

The full code for the Dogs vs Cats app can be found [here](https://github.com/dockersamples/example-voting-app).

```
# kubectl create -f .

It'll take a few minutes for everything to start up. To watch progress run :

# watch kubectl get all --all-namespaces

microk8s has it's own version of Docker installed inside the snap and you can use familiar docker commands to get logs, exec into containers etc.

# microk8s.docker ps

# microk8s.docker logs 

# microk8s.docker exec -it 

```
### Test it works 

```
To find the ports being used by the vote and results services run :

# kubectl get all --all-namespaces

Example Output :

service/result       ClusterIP   10.152.183.130   <none>        5001/TCP   19m
service/vote         ClusterIP   10.152.183.253   <none>        5000/TCP   19m

In this example to test the app is working we would open 2 browser tabs and navigate to :

http://10.152.183.130:5001
http://10.152.183.253:5000

Try clicking a Vote button and verify that the results % is updated.
```

### Remove microk8s

```
# snap remove microk8s

```

### Remove Kompose 

```
# rm /usr/bin/kompose

```


# Summary

This is a fairly quick and clean way to install a local Kubernetes, deploy a simple microservice example, verify it works and tear it down. Since microk8s has packaged up multiple versions of Kubernetes you could script this to verify your compose files work against multiple versions of Kubernetes. 
