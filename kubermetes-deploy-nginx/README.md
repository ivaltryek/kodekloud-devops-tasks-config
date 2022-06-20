# Deploy nginx server on kubernetes

## Task Description 📔

Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:

Create a `deployment` using `nginx` image with latest tag only and remember to mention the tag i.e `nginx:lates`t. Name it as `nginx-deployment`. The container should be named as `nginx-container`, also make sure `replica` counts are `3`.

Create a `NodePort` type service named nginx-service. The `nodePort` should be `30011`.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

## Solution

- Create deployment file
  ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    creationTimestamp: null
    labels:
        app: nginx-deployment
    name: nginx-deployment
    spec:
    replicas: 3
    selector:
        matchLabels:
        app: nginx-deployment
    strategy: {}
    template:
        metadata:
        creationTimestamp: null
        labels:
            app: nginx-deployment
        spec:
        containers:
        - image: nginx:latest
            name: nginx-container
            ports:
            - containerPort: 80
    ```

- Apply the file
  ```bash
  kubectl apply -f deployment.yaml
  ```

- Create service file
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    creationTimestamp: null
    labels:
      app: nginx-deployment
    name: nginx-service
  spec:
    ports:
    - nodePort: 30011
      protocol: TCP
      port: 80
    selector:
      app: nginx-deployment
    type: NodePort
  ```

- Apply the file
  ```bash
  kubectl apply -f svc.yaml

- Test the app if its running by clicking on kodekloud GUI to open the website.