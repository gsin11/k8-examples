# k8-examples

This repository consists of some Kubernetes examples.

## First step is to create credentials

Since all the credentials are being stored in base64 string we need to convert them first.

```
$ cd mongo-db-app // you should inside mongo-db-app directory, not necessary for following command but for upcoming commands.

$ echo -n 'username' | base64
$ echo -n 'password' | base64
```

## Create Secrets

Secrets are responsible to store sensitive information like usernames, password, tokens, etc.

```
$ kubectl apply -f mongo-secret.yaml

$ kubectl get secret  // You can check if secrets have been created successfully
```

## Create Mongo Deployment and Service

Now we have mongo db related credentials created, we can now go for deployments.

```
$ kubectl apply -f mongo.yaml // It will deploy mongo db pod and respective service to use it
```

## Deploy mongo-express app

Now that we have deployed mongodb service and pod, we need to create express service to use it.

Similar to Secrets we now need to deploy `ConfigMap` which will help our express application to run by using environment variables.

```
$ kubectl apply -f mongo-configmap.yaml
$ kubectl apply -f mongo-express.yaml // It will deploy mongo-express pod and create a corresponding service type=LoadBalancer to make the application accessible from outside
```

## Final step

Once we have everything running inside kubernetes, we now need to expose it to MiniKube.

`$ minikube service mongo-express-service`

Note: If you won't get message showing-up with your express IP please follow the command.

`$ minikube service --all` // check your service name and select its IP address and port and open it on browser.

## Namespaces
These are virtual clusters inside a cluster. Are are 4 out of the box namespaces.
### kube-system
We should not use this namespace, since it is being used internally by kubernetes.
### kube-public
This namespace is being used to make your nodes public.
### kube-node-lease
- It holds information for heartbeats of nodes.
- each node has associated lease object in namespace.
- It also determines the *availablity of a node*
### default
Last but not least, this namespace holds all nodes which are not having any custom namespace.

*It is better to use custom namespace, it acts like a project inside k8s. It helps you to group your resources. Example: database namespace holds database related resources.*

### Commands
```
$ kubectl create namespace my-namepsace // to create namespace
$ kubectl get namespace // It will show all the namespaces
```

There is no out of the box way to change default namespace. Perhaps, you can install a package `brew install kubectx` and use following commands to make your own namespace default.

It comes with `kubens` package which will do nothing but list all the namepsace with default hightlight.

By running `kubens my-namespace` you can make `my-namepsace` as a default namespace.


## Ingress
It is not but gives option to expose your application on a particular domain. from `http://12.12.22.1:30000` to `http://my-domain.com`

### Ingress controller
- Evalute all the rules
- Manage redirections
- Entrypoint to cluster
- Third party integrations

To enable ingress in minikube, perform `minikube addons enable ingress`, it will enable nginx ingress controller.

### <center>Happy coding</center>
