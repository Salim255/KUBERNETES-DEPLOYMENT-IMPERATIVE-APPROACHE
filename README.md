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

#### We run kubectl dashboard in order to see the schema of or cluster in browser we run: $kuberctl dashboard

## Master node(Control Plane):

### Is responsible for creating all the things we need in our cluster and for example distributing the pods across worker node, by analyzes currently running Pods and finds the best Node for the new Pod(s)

## Worker node:

### Has what we called kubelet to manages the Pod and Containers and check it's health

## Pod: containe a container based in the image specified when we create the deployment object

## The Service Object

### To reach a pod and a container running in a pod we need a service, and Service object is an other object that Kubernetes need in order to expose the pods to other pods in the cluster or to visitors outside of the cluster.

### Point to a pod with it's IP address it's not the right wolution, because the IP of pod changes, with every new pod, so Service group the pods and gave them a shared IP address, then we can access the pods througth that IP address and also we can share that IP address to allow external access to Pods from outside of the cluster, by default it's access internal onley , but with service we can overide this, so without service pods is very hard to reach even internally, because of that IP address changing all the time, and from outside the cluster the pods are not reachable at all without this Serivce object
