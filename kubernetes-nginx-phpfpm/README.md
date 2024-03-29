#  Deploy Nginx and Phpfpm on Kubernetes

The Nautilus Application Development team is planning to deploy one of the php-based application on Kubernetes cluster. As per discussion with DevOps team they have decided to use nginx and phpfpm. Additionally, they shared some custom configuration requirements. Below you can find more details. Please complete the task as per requirements mentioned below:

1.) Create a `service` to expose this app, the service type must be `NodePort`, `nodePort` should be `30012`.

2.) Create a config map `nginx-config` for `nginx.conf` as we want to add some custom settings for nginx.conf.

a) Change default port `80` to `8095` in `nginx.conf`.

b) Change default document root `/usr/share/nginx` to `/var/www/html` in nginx.conf.

c) Update directory `index to index index.html index.htm index.php` in nginx.conf.

3.) Create a pod named `nginx-phpfpm` .

b) Create a shared volume `shared-files` that will be used by both containers (nginx and phpfpm) also it should be a `emptyDir` volume.

c) Map the ConfigMap we declared above as a volume for nginx container. Name the volume as nginx-config-volume, mount path should be `/etc/nginx/nginx.conf` and subPath should be `nginx.conf`

d) Nginx container should be named as `nginx-container` and it should use `nginx:latest` image. PhpFPM container should be named as `php-fpm-container` and it should use `php:7.0-fpm` image.

e) The shared volume `shared-files` should be mounted at `/var/www/html` location in both containers. Copy` /opt/index.php` from jump host to the `nginx document root` inside nginx container, once done you can access the app using App button on the top bar.

Before clicking on finish button always make sure to check if all pods are in running state.

You can use any labels as per your choice.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution

- Create Config Map `cm.yaml`
  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: nginx-config
  data:
    nginx.conf: |
      events {} 
      http {
        server {
          listen 8095;
          index index.html index.htm index.php;
          root  /var/www/html;
        }
      }
  ```

- Create pod manifest `pod.yaml`
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-phpfpm
    labels: 
      app: nginx-phpfpm
  spec:
    volumes:
      - name: shared-files
        emptyDir: {}
      - name: nginx-config-volume
        configMap:
          name: nginx-config
    containers:
      - name: nginx-container
        image: nginx:latest
        ports:
          - containerPort: 8095
        volumeMounts:
          - name: shared-files
            mountPath: /var/www/html
          - name: nginx-config-volume
            mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
      - name: php-fpm-container
        image: php:7.0-fpm
        volumeMounts:
          - name: shared-files
            mountPath: /var/www/html
  ```

- Create nodeport service `nodeportsvc.yaml`
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-phpfpm
  spec:
    selector:
      app: nginx-phpfpm
    ports:
    - protocol: TCP
      port: 8095
      nodePort: 30012
    type: NodePort
  ```

- Apply all manifests file
  ```bash
  kubectl apply -f .
  ```

- Copy the file from `/opt/index.php` in the Pod.
  ```kubectl
  kubectl cp /opt/index.php nginx-phpfpm:/var/www/html -c nginx-container
  ```

- Browse the application by clicking on the App button at top of the terminal and if you see `It works!` or whatever your content in index.php file then It is a success.