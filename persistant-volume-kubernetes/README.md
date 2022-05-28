# Persistent Volume Kubernetes

# Task Description 📓

- Create `PV` named `pv-xfusion` with `storage class` as `manual`, `capacity` of `4Gi`, `accessMode` should be `ReadWriteOnce`, `hostPath` set to `/mnt/itadmin`

- Create `PVC` named `pvc-xfusion` with `storage class` as `manual`, `capacity` request resource of `3Gi`, `accessMode` should be `ReadWriteOnce`
  
- Create `Pod` named `pod-xfusion` with attached `pvc`: `pvc-xfusion` and `containerName` should be `container-xfusion`, use the `image` of `httpd:latest`. `Mount` path should be directory root

- Create `svc` named `web-xfusion` with type `NodePort`, expose it on port `30008`

## Solution

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-xfusion
spec:
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  hostPath:
    path: /mnt/itadmin

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-xfusion
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 3Gi

---

apiVersion: v1
kind: Pod
metadata:
  name: pod-xfusion
  labels:
    app: httpd
spec:
  volumes:
    - name: storage-xfusion
      persistentVolumeClaim:
        claimName: pvc-xfusion
  containers:
    - name: container-xfusion
      image: httpd:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: storage-xfusion
          mountPath:  /usr/local/apache2/htdocs/

---

apiVersion: v1                                                                                                
kind: Service                                                                                                 
metadata:                                                                                                     
  name: web-xfusion                                                                                         
spec:                                                                                                         
   type: NodePort                                                                                             
   selector:                                                                                                  
     app: httpd                                                                                     
   ports:                                                                                                     
     - port: 80                                                                                               
       targetPort: 80                                                                                         
       nodePort: 30008

```

- Apply the configuration
  
  ```bash
  kubectl apply -f configuration.yaml
  ```