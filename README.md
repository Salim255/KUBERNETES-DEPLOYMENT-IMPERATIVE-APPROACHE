# KUBERNETES-DEPLOYMENT-IMPERATIVE-APPROACHE

# Steps:

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
