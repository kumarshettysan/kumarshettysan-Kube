For Creating a pod and service in kubernetes cluster where service can expose a “Hello World” web page running in the Pod. And in the Pod, there are 2 containers. “Hello World” container and “builder” container(init containers)Cri

Prerequisite: Make sure Kube cluster running with containerd as container runtime 

Step1: We have two dockerfiles , one for builder and another one hello-world using nginx web server

Step 2: Then we have build script that performs the build steps for our application , copies deafult index.html to local

Step 3: Build docker images for both conatiners

Step 4: Create and run the kube pod configuration
 
kubectl apply -f kube-hello-world.yaml

Step 5: Check pod status and then port-forward the Nginx container to test hello-world Webpage
kubectl port-forward hello-world-pod 8080:80

Step 6: Open http://localhost:8080 to see the "Hello, NGINX!" web page served by the NGINX container.


Kubernetes Device Plugins:
Create a device plugin which can be
deployed as DaemonSet in the Kubernetes cluster.
Extra Notes
The DaemonSet gets a list of devices from Environment Variable. Then the device plugin running on a
node can manage and allocate devices to pods per their request. If your DaemonSet functions correctly,
a pod can mount and use those devices (can be fake devices).

Step1: Creating simple Device plugin using Go code

Step2: Build the device plugin using Go 
go build kube-device-plugin.go

Step3: Create a Dockerfile and build docker image 
docker build -t kumarshetty.artifactory/dockerlocal-artifactory/fake-device-plugin:latest .

Note: dockerlocal-artifactory is a local docker registry created in a artifactory instance hosted locally 

Step4: Pushing the docker image to registry 
docker push kumarshetty.artifactory/dockerlocal-artifactory/fake-device-plugin:latest

Step5: Creating a daemonset and deploy
kubectl apply -f device-daemonset.yaml

Step6: Verify device plugin
kubectl logs -n kube-system device-plugin

Step7:  Test the Device Plugin by creating test pod
kubectl apply -f test-pod.yaml

Step8: Validate device plugin running as a daemon set in kube cluster
kubectl logs test-pod
