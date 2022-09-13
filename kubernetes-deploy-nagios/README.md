# Deploy Nagios on Kubernetes

## Task Description 📔

The Nautilus DevOps team is planning to set up a Nagios monitoring tool to monitor some applications, services etc. They are planning to deploy it on Kubernetes cluster. Below you can find more details.

1) Create a deployment `nagios-deployment` for Nagios core. The container name must be `nagios-container` and it must use `jasonrivers/nagios` image.

2) Create a user and password for the Nagios core web interface, user must be `xFusionCorp` and password must be `LQfKeWWxWD`. (you can manually perform this step after deployment)

3) Create a service `nagios-service` for Nagios, which must be of targetPort type. `nodePort` must be `30008`.

You can use any labels as per your choice.

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.

## Solution

- Create Deployment.yaml
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nagios-deployment
  spec:
    selector:
      matchLabels:
        app: nagios-deployment
    template:
      metadata:
        labels:
          app: nagios-deployment
      spec:
        containers:
        - name: nagios-container
          image: jasonrivers/nagios
          resources:
            limits:
              memory: "128Mi"
              cpu: "500m"
          ports:
          - containerPort: 8080
  ```

- Create Service.yaml
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: nagios-service
  spec:
    selector:
      app: nagios-deployment
    type: NodePort
    ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30008
  ```

- Apply the manifests
  ```bash
  kubectl apply -f *.yaml
  ```

- Once the deployments are up, `exec` into the pod
  ```bash
  kubectl exec -ti <pod-id> -- bash
  ```
- Create the User
  ```bash
  htpasswd /opt/nagios/etc/htpasswd.users xFusionCorp
  ```

  > **Note** <br>
  You'll be prompt to enter password twice.

- Check the nagios deployment by opening the nagios web interface.