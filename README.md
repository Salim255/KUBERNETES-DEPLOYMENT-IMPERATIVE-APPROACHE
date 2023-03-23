# KUBERNETES-DEPLOYMENT-IMPERATIVE-APPROACHE

# Steps for minukube cluster:

## 1: Build the docker image

### run docker build -t kub-fisrt-app .

## 2: Send the image to the Kubernetes Cluster

### a) Check that the cluster is up and running by running: minikube status or minikube start --driver=vbox to restart it

### b) Send the instruction to create a deployment to the running Cluster

#### Check the deployment object: $kubectl get deployments

#### To see all the posd: $kubectl get pods

#### To delete deployment : $kubectl delete deployment first-app

#### We tag build image in order to send to dockerhub $docker tag kub-first-app crawan/kub-first-app

#### We run push to push the build image to respository image name:$ docker push crawan/kub-first-app

#### We use kubectl to create adeployment object, then it will be automatically send to the cluster: $ kubectl create deployment first-app --image=crawan/kub-first-app

#### We (run kubectl dashboard) in order to see the schema of or cluster in browser we run: $minikube dashboard

## Master node(Control Plane):

### Is responsible for creating all the things we need in our cluster and for example distributing the pods across worker node, by analyzes currently running Pods and finds the best Node for the new Pod(s)

## Worker node:

### Has what we called kubelet to manages the Pod and Containers and check it's health

## Pod: containe a container based in the image specified when we create the deployment object

## The Service Object

### To reach a pod and a container running in a pod we need a service, and Service object is an other object that Kubernetes need in order to expose the pods to other pods in the cluster or to visitors outside of the cluster.

### Point to a pod with it's IP address it's not the right wolution, because the IP of pod changes, with every new pod, so Service group the pods and gave them a shared IP address, then we can access the pods througth that IP address and also we can share that IP address to allow external access to Pods from outside of the cluster, by default it's access internal onley , but with service we can overide this, so without service pods is very hard to reach even internally, because of that IP address changing all the time, and from outside the cluster the pods are not reachable at all without this Serivce object

### Setup Service object: $kubectl expose deployment first-app --type=LoadBalancer --port=8080(the default type is ClusterIP, which means it's onley be available from inside the cluster, or NodePort, which means this deployment should be exposed with the help of an ip address of a Worker node on which it's running)

### So the LoadBalancer will generate a unique address for this service, also distibute all incoming traffic accross all Pods which are part of this service

### To check if service was created: $kubectl get service

### To get the external address in case minikube or te see the running service run: $ minikube service first-app

# Scaling in action run: kubectl scale deployment/first-app --replicas=3

# Update a deployment

## 1\ Applay the modification to your soucre code

## 2\ Re build your image: $ docker build -t crawan/kub-first-app .

## 3\ Push the updated image to docker hub: $ docker push crawan/kub-first-app

## 4\ About the deployment: $ kubectl set image deployment/first-app kub-first-app(nam of the container)=crawan/kub-first-app

# 🔥🔥 New image will not be used after the updating steps if we use the same image's name in the current container, so in order to allow the container to use the new version of the image that have the same name, we must add a tag to the built image ( docker build -t crawan/kub-first-app: 2 . ;docker push crawan/kub-first-app:2 ; kubectl set image deployment/first-app kub-first-app=crawan/kub-first-app:2 )

# To apply the curent updating status : kubectl rollout status deployment/first-app

# 🔥 Kubernetes protect your running application, so we try to update the image of the running container with an image that doesnt exist, kubernetes will always try first to create a new pod, in ordrer to confirm that the image is correct and work just fine, once this step, verified, kubernetes delete the old pods and replace them with news pods with the updated image else we need to make rollback to step this step(pulling wrong image)by running: $ kubectl rollout undo deployment/first-app

# To go back to older deployment:

## 1\ Look at the deployment history by run: $kubectl rollout history deployment/first-app; So we get a list revision with there id, from where can review the exact revision by running :$kubectl rollout history deployment/first-app --revision=revision-number; where we can see which image was used in that revision

# 2\ The the command to rollback to exat revision running: $ kubectl rollout undo deployment/first-app --to-revision=revisionnumber

# Deleting a service by running: $kubectl delete service first-app

# Delete deployment: $kubectl delete deployment first-app
