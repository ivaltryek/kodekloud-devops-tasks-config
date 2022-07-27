# Manage Secrets in Kubernetes

## Task Description 📔

The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

We already have a secret key file `ecommerce.txt` under `/opt` location on jump host. Create a generic secret named `ecommerce`, it should contain the password/license-number present in `ecommerce.txt` file.

Also create a `pod` named `secret-devops`.

Configure pod's spec as container name should be `secret-container-devops`, image should be `ubuntu` preferably with `latest` tag (remember to mention the tag with image). Use `sleep` command for `container` so that it remains in running state. Consume the created secret and mount it under `/opt/apps` within the container.

To verify you can exec into the container `secret-container-devops`, to check the secret key under the mounted path `/opt/apps`. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution

- Create secret
  ```bash
  kubectl create secret generic ecommerce --from-file=/opt/ecommerce.txt
  ```

- create `pod.yaml`
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      run: secret-devops
    name: secret-devops
  spec:
    volumes:
      - name: secret-volume-devops
        secret: 
          secretName: ecommerce
    containers:
    - image: ubuntu:latest
      name: secret-container-devops
      command: ["/bin/bash", "-c", "sleep 20000"]
      volumeMounts:
        - name: secret-volume-devops
          readOnly: true
          mountPath: /opt/apps
  ```

- Apply manifest and wait for the pod to be in running state
  ```bash
  kubectl apply -f pod.yaml
  ```

- Exec into the pod to see whether the secret is mounted or not.
  ```bash
  kubectl exec secret-devops -c secret-container-devops -- cat /opt/apps/ecommerce.txt
  ```

- if it shows the content, then it is working as expected.