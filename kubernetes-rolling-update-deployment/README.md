# Rolling Updates And Rolling Back Deployments in Kubernetes

## Task Description 📔

There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.

Create a namespace nautilus. Create a deployment called httpd-deploy under this new namespace, It should have one container called httpd, use httpd:2.4.27 image and 3 replicas. The deployment should use RollingUpdate strategy with maxSurge=1, and maxUnavailable=2. Also create a NodePort type service named httpd-service and expose the deployment on nodePort: 30008.

Now upgrade the deployment to version httpd:2.4.43 using a rolling update.

Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution

- Create `deployment.yaml` file.
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    namespace: nautilus
    labels:
      app: httpd-deploy
    name: httpd-deploy
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: httpd-deploy
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxUnavailable: 2
        maxSurge: 1
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: httpd-deploy
      spec:
        containers:
        - image: httpd:2.4.27
          name: httpd
          ports:
            - containerPort: 80
  ```

- Apply the file
  ```bash
  kubectl apply -f deployment.yaml
  ```

- Create nodeport service `service.yaml`
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    namespace: nautilus
    labels:
      app: httpd-deploy
    name: httpd-service
  spec:
    ports:
    - port: 80
      protocol: TCP
      nodePort: 30008
      targetPort: 80
    selector:
      app: httpd-deploy
    type: NodePort
  ```

- Apply the file
  ```bash
  kubectl apply -f service.yaml
  ```

- Update the deployment image 
  ```bash
  kubectl set image deployment httpd-deploy  httpd=httpd:2.4.43 --namespace nautilus --record=true
  ```

- Once the pods are up and running rollback the update
  ```bash
  kubectl rollout undo deployment httpd-deploy  -n nautilus
  ```