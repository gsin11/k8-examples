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
$ kubectl apply -f mongot-secret.yaml

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

### <center>Happy coding</center>
