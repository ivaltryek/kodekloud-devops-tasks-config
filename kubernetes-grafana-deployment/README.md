# Grafana Deployment on Kubernetes

- Create deployment of Grafana with `grafana-deployment-devops` name.
- Expose the deployment using `NodePort` Service with port `32000`.

## Solution

- Create grafana deployment file.
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: grafana
    name: grafana-deployment-devops
  spec:
    selector:
      matchLabels:
        app: grafana
    template:
      metadata:
        labels:
          app: grafana
      spec:
        securityContext:
          fsGroup: 472
          supplementalGroups:
            - 0
        containers:
          - name: grafana
            image: grafana/grafana:8.4.4
            imagePullPolicy: IfNotPresent
            ports:
              - containerPort: 3000
                name: http-grafana
                protocol: TCP
            readinessProbe:
              failureThreshold: 3
              httpGet:
                path: /robots.txt
                port: 3000
                scheme: HTTP
              initialDelaySeconds: 10
              periodSeconds: 30
              successThreshold: 1
              timeoutSeconds: 2
            livenessProbe:
              failureThreshold: 3
              initialDelaySeconds: 30
              periodSeconds: 10
              successThreshold: 1
              tcpSocket:
                port: 3000
              timeoutSeconds: 1
            resources:
              requests:
                cpu: 250m
                memory: 750Mi

  ```

- Create NodePort type service.
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: grafana
  spec:
    ports:
      - port: 3000
        protocol: TCP
        targetPort: http-grafana
        nodePort: 32000
    selector:
      app: grafana
    sessionAffinity: None
    type: NodePort
  ```

- Apply the files
  ```
  kubectl apply -f *.yaml
  ```

Browse the grafana service from the grafana port check if displays the output.