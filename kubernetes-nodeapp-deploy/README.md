# Node App deployment in Kubernetes

# Task Description 📓

Create deployment using `gcr.io/kodekloud/centos-ssh-enabled:node` image. You can name any labels as per your need. Replica count should be `2`. Create NodePort type service and use `30012` port as nodeport and use `8080` as a target port.

## Solution

- Create deployment file
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: myapp
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: myapp
    template:
      metadata:
        labels:
          app: myapp
      spec:
        containers:
        - name: myapp
          image: gcr.io/kodekloud/centos-ssh-enabled:node
          resources:
            limits:
              memory: "128Mi"
              cpu: "500m"
          ports:
          - containerPort: 8080
  ```

- Create NodePort type service
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: myapp-service
  spec:
    type: NodePort
    selector:
      app: myapp
    ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30012
  ```

> **Note** <br>
> You should be able to access the site from the top button in interface.