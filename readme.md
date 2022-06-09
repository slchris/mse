# Microservices example



# Environment

1. Docker for Mac 20.10.14
2. minikube version: v1.25.2
3. kubernetes version v1.23.3
4. kong version latest

# Build Docker Images


```shell
docker build -t ghcr.io/slchris/mse:0.1 .
```


# Deploy App

```shell
kubectl apply -f app.yaml
```

# Access the Services


```shell
kubectl port-forward service/app-service 5000:5000
```

Use `curl` command acces the mse container

```shell
curl localhost:5000
```

# Install Kong

Create `kong` namespace

```shell
kubectl create namespace kong
```

Install Kong for Kubernetes

```shell
kubectl apply -f https://bit.ly/kong-ingress-dbless
```

Get the `EXTERNAL-IP` value:
```shell
kubectl -n kong get service kong-proxy
```
> Tips: The EXTERNAL-IP cannot be directly obtained on the mac. It's alwyas in  pending state. We can use this command to solve the problem.

```shell
minikube tunnel
```

For now, let's export that IP number as an env variable:

```shell
export IP=127.0.0.1 # your own EXTERNAL-IP
```
now you can check that kong is working:

```shell
curl $IP
```
result:

```
{"message":"no Route matched with those values"}
```

# Configure Kong Gateway

```shell
kubectl apply -f app-ingress.yaml
```

Use `curl` to check:

```shell
curl $IP/app
```


result:

```
{"msg":"Hello from the foo microservice"}
```
