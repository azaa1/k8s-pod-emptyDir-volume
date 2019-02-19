# Kubernetes Pod and 'emptyDir' Volume.

### An 'emptyDir' volume, as the name suggest, is an empty directory that gets created in the pod. This directory can be shared with pods. Note if the location to which you connect the 'emptyDir' has data, the existing data are masked and become hidden. 

#### * In this example: 

- A pod named 'nginx-pod' is created. 

- A label 'app: nginx-app' gets attached to it. 

- RestartPolicy is set to 'Never', which means the container will not be restarted regardless of why it exited. 

- Under volumes, it creates 'emptyDir' volume named 'shared-volume'. Which in the background creates an empty folder on the node that the pod gets assigned to. The location of the folder on the node is : /var/lib/kubernetes/pods/<id-of-the-pod>/volumes/kubernetes.io~empty-dir/

- Under containers, 2 containers are created. 

### * Container 1: Nginx-Container

- The image 'nginx' is pulled from Docker Hub. 
- Port 80, protocol TCP is, is exposed. 
- Under volumeMounts, the directoy (inside container) '/usr/share/nginx/html' is mounted to the volume (shared-volume).

### * Container 2: Debian-Container 

- The image 'debian' is pulled from Docker Hub. 

- Under volumes, volume 'shared-volume' is attached to /pod-data folder inside the container.
- This container runs a '/bin/sh' command, which creates a custom 'index.html' file and goes to sleep state. 

#### * Note: Since both containers are sharing the same volume (emptyDir), the 'index.html' created by Debian-Container will be visible to 'Nginx-Container' under '/usr/share/nginx/html' folder. 

#### * To test, on your master node:

- Get the IP of the node by running:
- kubectl get pods -o wide 

- Then run curl command: 
- curl < ip-of-node >

### * You should see the message:  "Hello from the Debian container"
