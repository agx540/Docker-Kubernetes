# Docker and Kubernetes: The Complete Guide

This course in on Udemy.com.

<https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide>

How to open diagrams in draw.io. Example for chapter 1.
<https://www.draw.io/?mode=github#HStephenGrider%2FDockerCasts%2Fmaster%2Fdiagrams%2F01%2Fdiagrams.xml>

Chaper 3:\
<https://www.draw.io/?mode=github#HStephenGrider%2FDockerCasts%2Fmaster%2Fdiagrams%2F03%2Fdiagrams.xml>

Chapter 7:\
<https://www.draw.io/?mode=github#HStephenGrider%2FDockerCasts%2Fmaster%2Fdiagrams%2F07%2Fdiagrams.xml>

Chapter 8:\
<https://www.draw.io/?mode=github#HStephenGrider%2FDockerCasts%2Fmaster%2Fdiagrams%2F08%2Fdiagrams.xml>

Chapter 9:\
<https://www.draw.io/?mode=github#HStephenGrider%2FDockerCasts%2Fmaster%2Fdiagrams%2F09%2Fdiagrams.xml>

## Course Information

- Capital 3 - Video 35

## Problems with course

### postgres volume mount fail

I'm not able to mount volume to postgres container to store database.

## Dive into Docker

### Why Docker

It helps you deploy and run software in a known environment.
It makes it very easy to install and run software.

### What is docker

Docker is a platform or ecosystem around creating and running containers.

Docker Ecosystem:

- Docker Client
- Docker Server
- Docker Machine
- Docker Images
- Docker Hub
- Docker Compose

### Docker Client

1. run "docker run hello-world"
2. Docker Server looks at local image cache for this image
3. If it's not there it goes to Docker Hub
4. If it finds it there the image gets downloaded.
5. Image gets executed

![little docker run workflow](img/2020-03-12-10-33-09.png)

### Container vs Image

A container is a running image in Docker Server. Container in Docker Server
are seperated through Namespaces and Control Groups.
Each image has an Startup Command to define what program needs to be started
when a container is started.

![](img/2020-02-28-07-55-12.png)

**Namespaces** are used to separate and isolate resources per process or group of processes.

**Control Groups** limit amount of resources used per process.

### Docker Cache

Docker will cache the order and results of building images. It will cache the
different steps to enable faster rebuilds.

**Attention:** If you change command order in a dockerfile the cache can't be used.

### How it runs on your local Windows or MacOS Machine

![Docker on desktop OS](img/2020-02-28-08-47-30.png)

Docker run = docker create + docker start

Docker create:

Take the filesystem from an image and use it in an harddrive segment on the host.

Docker Start:

Runs start command of container.

### How you can communicate with a running processs in a container

![docker container system overview](img/2020-02-28-16-10-19.png)

use -it on command allows you to connect to STDIN on running processes in a container and pretty up output.

## Dockerfile

Configuration to define how our container should behave.

### Commands for a Dockerfile

Base syntax
> \<Instruction\> \<argument to the instruction\>

Specify base image
> FROM \<Base image name\>

Set a working directory for building the image and for running the container.
> WORKDIR /usr/app

Prepare custom image
> RUN \<some instructions\>

Setup start command for container
> CMD ["\<start command\>"]

### Workflow to create a Dockerfile

1. Specify baseimage
2. Run some commands to installer additional programs
3. Specify a command to run on container setup

Compared to installing a Chrome on an empty computer.
![Install Chrome on an empty computer](img/2020-02-29-14-46-38.png)

## Docker Client command line commands

### Base

get docker version
> docker version

Clear docker. Removes images, containers, networks or build cache
> docker system prune

### Build images

Build an image from a dockerfile in current directory
> docker build .

Build an image from a dockerfile in current directory
> docker build -f \<docker file name\> .

Build an image from a dockerfile and tag. Tag is version
> docker build -t \<docker id\>/\<repo/project name\>:\<Version\> \<buildcontext\>
> docker build -t alexsnyx/testproject:latest .

Change a running container and create a new image. -c applies a change to a running container.
> docker commit - c 'CMD ["redis-server"]' \<container id\>

### Image repository

Push image into repository
> docker push \<image name\>

### Create to kill containers

Start image with default start command
> docker run \<image name\>

Start image and override start command
> docker run \<image name\> \<some command\>

Start image in background and print container id
> docker run -d \<image name\>

Create a container from an image and returns a container id
> docker create \<image name\>

Create a container from an image, overrides default command and returns a container id
> docker create \<image name\> \<command\>

Start container
> docker run \<container id\>

Start container an forward network traffic from localmachine port to container port
> docker run -p \<localmachine port\>:\< container port\> \<container id\>

Start container and forward container output to current console
> docker start -a \<container id\>

Startup a container and attach a shell after startup.
> docker run -it \<image name\> sh

Stop container.  Sends a SIGTERM to a container and give them some time to clean up.
> docker stop \<container id\>

Stops container immediately. It sends SIGKILL to container.
> docker kill \<container id\>

Remove container
> docker rm \<container id\>

### Interact with running containers

List running containers
> docker ps

List all container ever created
> docker ps --all

Get logs from a container
> docker logs \<container id\>

Execute a command in a running container. -it means connect to STDIN.
> docker exec -it \<container id\> \<command\>

Get access to command line from a container
> docker exec -it \<container id\> sh

Attach to STDIN of a running container
> docker attach  \<container id\>

    Note: The attach command will display the output of the ENTRYPOINT/CMD process. This can appear as if the attach command is hung when in fact the process may simply not be interacting with the terminal at that time.

### Interact with docker runtime

Connect to docker server shell. (MobyLinuxVM)
> docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm alpine /bin/sh

## Docker Compose

- Seperate CLI that gets installed along with Docker.
- Used to start up multiple Docker containers at the same time.
- Automates some of the long-winded arguments we were passing to 'docker run'

### docker-compose cli

Run docker-compose.yml file
> docker-compose up

Run docker-compose.yml file in background
> docker-compose up -d

Run docker-compose.yml file and rebuild containers
> docker-compose up --build

Stop container started with docker-compose
> docker-compose down

Get status of running container belong to docker-compose.yml file in same directory.
> docker-compose ps

### docker-compose.yml File language

- Use spaces after -
- Take care of indention
- use lower case for key words

#### Volumes (docker-compose)

    volumes:
      - /app/node_modules
      - .:/app

- Don't override /app/node_modules in container after mapping some volumes.
- Map current directory . to /app in container.

#### Restart policies

Exit code 0 means no error.
Exit code > 0 means error.

![Restart policies](img/2020-03-09-11-33-22.png)

## Volumes (docker cli)

Map local current directory into /app folder of container. Exclude node_modules folder from container, this means don't override folder in container by mapping volume into container.
> docker run -it -p 3000:3000 -v /app/node_modules -v ${pwd}:/app ac4

![](img/2020-03-09-15-07-06.png)

### Commands

Run pwd in your command to get present working directory.
> pwd

## Create development workflow

1. Develop
2. Test
3. Deploy
4. Start with 1. again

![DevOps Workflow](img/2020-03-09-13-11-56.png)

### Development Environment

### Test Environment

There are different approaches to start test environment.

1. Connect to dev container and execute tests
2. Create a second container in docker-compose and execute tests

But there is always a little problem to test a engine which needs some manual command line input, like in react.

### Production Environment

#### Multistep Build

![multistep build](img/2020-03-10-11-25-21.png)

## Continous Integration and Deployment with AWS and SINGLE CONTAINER

1. write some code
2. push it to github
3. Travis-ci gets triggered by code changes
4. downloads current sources
5. runs .travis.yml
6. build Dockerfile.dev
7. Create a container from 6. and executes tests
8. Deploy it to AWS Elastic Beanstalk
9. Builds new image from Dockerfile
10. Executes

![CI and deployment workflow](img/2020-03-10-13-20-12.png)

### Bring code online into GITHUB

1. Create online github repository
2. Create local git repo

![create local repo and connect to remote](img/2020-03-10-13-18-17.png)

### Setup Travis CI

1. Go to travis-ci.com
2. login with your github account
3. Allow travis-ci to connect to your github repositories
4. Create at trigger to build after

### Setup Elastic Beanstalk (=AWS : Docker)

Elastic Beanstalk lets you setup an application of one container. It's also
can scale up if traffic goes up.

1. Create an Elastic Beanstalk application
2. Choose Docker as runtime
3. Create IAM (Identity and Access Management) account for Travis to access beanstalk
4. Copy "Access Key" and "Secret Key" to Travis.ci docker-react project into "Environment Variables". Do not store this into source code.
5. Create deploy configuration in .travis.yml file for aws

![Elastic Beanstalk Overview](img/2020-03-11-09-35-47.png)

### Work with features branches

1. git checkout -b <\branchname>\
2. change code
3. git add .
4. git commit -m "\<some commit information\>"
5. git push origin <\branchname>\

## MULTI CONTAINER HANDLING

1. Boot up browser
2. visit website
3. nginx webserver do some routing either to show some website (React) or call some api(Express)
4. React is serving the website
5. Express Server is hosting the api
6. Redis is an inmemory memory store
7. Postgres is a database

![system overview](img/2020-03-11-11-12-46.png)

System flow to submit a request for a calculation.
![](img/2020-03-11-13-03-36.png)

Two pages will be needed for GUI.
![websites needs to be created](img/2020-03-11-13-16-20.png)

left one is coded in Fib.js.
Right one is coded in OtherPage.js

### Setup containers for development

### Setup docker-compose

![docker-compose setup](img/2020-03-11-14-28-05.png)

#### Enviroment Variables at runtime

![different environment variables](img/2020-03-11-14-45-47.png)

> variableName=value

This variables are created and set when the container is started.

> variableName

This variables are loaded from your local computer.

#### nginx

The browser calls only one backend and don't know about different services. nginx helps us to route a url to a service which can handle the request.
This is also a configuration without a port. So a lot easier to handle, because ports can change.

So ngnix watches for incoming request and route them of to the appropriate backend service.

![nginx setup](img/2020-03-11-15-14-40.png)

##### nginx config

![nginx configuration](img/2020-03-11-15-19-33.png)

### CI

![CI workflow](img/2020-03-12-10-14-25.png)

![production setup](img/2020-03-12-10-28-38.png)

### Deploy to AWS Elastic Beanstalk

Dockerrun.aws.json tell Elastic Beanstalk how to setup our Multi-Container Environment.

![difference between docker-compose aws beanstalk](img/2020-03-12-14-10-25.png)

How Elastic Beanstalk handles Docker

![Beanstalk Docker handling](img/2020-03-12-14-13-09.png)

Links allows containers to talk to each other. They are unidirectional. ngnix -> client or nginx -> api (=server)

![Create links](img/2020-03-12-14-49-31.png)

Create a Multi-Container App on AWS.

How system is splitted between stateless services and data storage.

![How system is splitted between stateless services and data storage](img/2020-03-12-15-13-46.png)

![AWS Elastic Cache](img/2020-03-12-15-15-23.png)

![AWS Relational Database Service](img/2020-03-12-15-16-17.png)

VPC: A VPC is a virtual private cloud. It's distinct for each region of AWS.

By default container in you Elastic Beanstalk can not connect AWS Elastic Cache or AWS Relational Database Service. Because they are running in different VPC.

![Container cannot by default talk to redis or postgres services on AWS](img/2020-03-12-15-29-56.png)

![VPC](img/2020-03-12-15-26-57.png)

Security Groups are Firewall rules for your VPC.

![Security Group](img/2020-03-12-15-34-08.png)

Create Postgres DB in AWS RDS:

- DB instance identifier: multi-docker-postgres
- Master username: postgres
- Master password: postgrespassword
- Database name: fibvalues

Create Redis in AWS ElastiCache:

- Name: multi-docker-redis
- Nodetype: cache.t2.micro
- Port: 6379
- Subnet name: redis-group
- Select all subnets

Wire all up with security group:

- Name: multi-docker
- Select default VPC

Create Inbound rule in VPC

- Port Range: 5432-6379

Add new created VPC to:

- Redis
- Postgres
- Elastic Beanstalk

Add Environment Variables to Elastic Beanstalk

![Environment variables for Elastic Beanstalk](img\2020-03-13-08-49-16.png)

Create a new User in AWS  IAM:

- Username: multi-docker-deployer
- Access type: pogrammatic access
- Add all Beanstalk policies

Add two new Environment Variables on TRAVIS CI

AWS_ACCESS_KEY
AWS_SECRET_KEY

and use values from AWS user above.



## Kuberenetes

Kubernetes is a system to deploy containerized apps.

![Kubernetes Overview](img/2020-03-15-14-50-32.png)

![What and Why Kubernetes](img/2020-03-15-14-51-52.png)

kubectl: Use for managing containers in the node.
minikube: User for managing the VM for lokal Kubernetes cluster.

![Kubernetes development vs production environment](img/2020-03-15-14-56-07.png)

### Transfer Knowledge from docker-compose to Kubernetes

![Docker-Compose.yaml](img/2020-03-15-15-52-46.png)

- Kubernetes needs startup ready images
- We need to do a networking manuall

![Docker Compose vs Kubernetes](img/2020-03-15-15-54-08.png)

### Kubernetes Big Picture

![kubernetes big picture](img/2020-03-16-14-33-44.png)

![Kubernetes big picture description](img/2020-03-16-14-34-42.png)

![Kubernetes Architecture](img/2020-03-21-16-25-03.png)

### Controller

A controller is an element which transforms the current state of the system to the desired state which is configured.
It tries to creates objects from configuration.

### Configuration

A configuration is the desired state Kubernetes needs to reach.

apiVersion decides what object you are able to create

![Kubernetes apiVersion](img/2020-03-16-09-33-41.png)

- StatefulSet
- ReplicaController
- Pod: A Pod is used to run a container
- Service: Used to setup some networking in a cluster.

![Kubernetes Object Types](img/2020-03-17-14-00-51.png)

#### Pod

Only add containers to the same pod if they are very tightly integrated
and have a close relationship. Otherwise each container should have a own pod.

![pod with more than one container](img/2020-03-16-09-39-16.png)

#### Service

Is used to setup some amount of networking in Kubernetes.

![Kubernetes Services](img/2020-03-16-10-05-01.png)

A service sit in front of one to many pods and routes traffic to pods.

##### NodePort Service

A NodePort Service is used only for development purpose.

![Kubernetes network setup on a dev machine](img/2020-03-16-10-07-40.png)

- kube-proxy: each node of a Kubernetes cluster has a kube-proxy to route traffic from a node to the outside world.
- Service NodePort: used to setup network rules for pods, cluster, ...

![Kubernetes selector](img/2020-03-16-10-16-34.png)

Kubernetes uses a "Labels Selector System".
A selector is used to reference a key value pair in the labels section of config file.

![Kubernetes Port configuration](img/2020-03-16-10-33-06.png)

- port: It's used inside the cluster.
- targetPort: send all incoming traffic to the pod.dir
- nodePort: is the port which is used to call the cluster and get forwarded to the pod.

![Imperative and declarative configuration approach](img/2020-03-16-15-41-26.png)

Kubernetes supports declarative and imperative approach

##### Cluster IP

Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default ServiceType.

##### LoadBalancer (deprecated)

Exposes the Service externally using a cloud provider’s load balancer. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.

##### External Name

Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.

##### Ingress

An API object that manages external access to the services in a cluster, typically HTTP. Ingress may provide load balancing, SSL termination and name-based virtual hosting.

![different nginx ingress projects](img/2020-03-18-14-59-11.png)

![ingress-nginx depends on environment where it's hosted](img/2020-03-18-15-01-12.png)

We use in the course: <https://github.com/kubernetes/ingress-nginx>

![Ingress Controller](img/2020-03-18-15-12-44.png)

![Ingress Controller and thing that routes traffic](img/2020-03-18-15-14-31.png)

Additional reading on Ingress Nginx
<https://www.joyfulbikeshedding.com/blog/2018-03-26-studying-the-kubernetes-ingress-system.html>

###### Ingress-Nginx on Google Cloud

![Ingress-Nginx on Google Cloud](img/2020-04-01-11-18-15.png)

- When you setup a ngnix-controller you get default-backend pod.

###### How to setup Ingress-Nginx

<https://kubernetes.github.io/ingress-nginx/deploy/>

#### Deployment

Difference between Pods and Deployment

![Difference between Pods and Deployment](img/2020-03-16-16-08-20.png)

![Deployment pod template](img/2020-03-16-16-10-03.png)

#### Volume

On-disk files in a Container are ephemeral, which presents some problems for non-trivial applications when running in Containers. First, when a Container crashes, kubelet will restart it, but the files will be lost - the Container starts with a clean state. Second, when running Containers together in a Pod it is often necessary to share files between those Containers. The Kubernetes Volume abstraction solves both of these problems.

![volume differences Docker and Kubernetes Volume object](img/2020-03-18-09-50-06.png)

![volume differences Docker and Kubernetes Volume object](img/2020-03-18-09-56-06.png)

There a lot of adapters to third party storage systems like:

- azureDisk
- azureFile
- awsElasticBlockStore
- iscsi
- nfs
- fc (fibre channel)
- ...

##### Persistent Volume

A persistent volume lives outside a pod and stays there. A Kubernetes Volume lives in a
pod and dies with a pod and all data is gone.

##### StorageClass

A StorageClass provides a way for administrators to describe the “classes” of storage they offer. Different classes might map to quality-of-service levels, or to backup policies, or to arbitrary policies determined by the cluster administrators. Kubernetes itself is unopinionated about what classes represent. This concept is sometimes called “profiles” in other storage systems.

List storage provisioner current configured in Kubernetes instance.
> kubectl get storageclass

Gives more information about storage provisioner current configured in Kubernetes instance.
> kubectl describe storageclass

###### Access Modes

![persistent volumes access modes](img/2020-03-18-10-12-11.png)

##### Persistent Volume Claim PVC

Is an advertisement of storage options.

#### Secret

Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys. Storing confidential information in a Secret is safer and more flexible than putting it verbatim in a Pod definition or in a container image. See Secrets design document for more information.

Create a secret from command line
> kubectl create secret generic \<secret name\> -from-literal \<key\>=\<value\>

### How to deploy a new image version for a deployment

![three possibilities to update an image of a deployment](img/2020-03-16-17-31-04.png)

> docker build -t alexsnyx/multi-client:v6 .
> docker push alexsnyx/multi-client:v6
> kubectl set image deployment/client-deployment client=alexsnyx/multi-client:v6

### How to deploy complex example to Google Cloud

![google cloud complex system overview](img/2020-03-18-15-17-01.png)

### kubectl

Cheatsheet see here: <https://kubernetes.io/de/docs/reference/kubectl/cheatsheet/>

Change the configuration of your local cluster. You only can do updates to a pod when you change:
    - container image
    - initContainer: image
    - spec.activeDeadlineSeconds
    - spec.tolerations
> kubectl apply -f \<filename\>\
or\
> kubectl apply -f \<folder\>

Remove an object
> kubectl delete -f \<filename\>

Remove a pod
> kubectl delete pod \<pod name\>

Remove a service
> kubectl delete service \<service name\>

Get pods from one namespace
> kubectl get pods --namespace=demo

Get status of pods
> kubectl get pods

Get status of services
> kubectl get services

Get current deployments
> kubectl get deployments

Get detailed info about an object
> kubectl describe \<objecttype\> \<objectname\>

Imperative command to update image
> kubectl set image \<object tye\> / \<object name\>  \<container name\> = \<new image to use\>
Exapmple:
>kubectl set image deployment/client-deployment client=alexsnyx/multi-client:v6

List namespaces
> kubectl get namespaces

Get logs from a pod
> kubectl logs \<pod name\>

List actual used persistent volumes
> kubectl get pv

List volumes claims
> kubectl get pvc

List storage provisioner current configured in Kubernetes instance.
> kubectl get storageclass

Gives more information about storage provisioner current configured in Kubernetes instance.
> kubectl describe storageclass

Create a secret from command line
> kubectl create secret generic \<secret name\> -from-literal \<key\>=\<value\>

List certificates
> kubectl get certificates

Get more information of a certificate
> kubectl describe certificates

### minikube

Start minikube
> minikube start --vm-driver hyperv --hyperv-virtual-switch "Minikube Switch"

Get minikube ip (run powershell in admin mode)
> minikube ip

Open dashboard
> minikube dashboard

Get a console to minikube
> minikube ssh

Get docker information running inside minikube
> minikube docker-env

Change docker client to connect to docker in minikube
> & minikube -p minikube docker-env | Invoke-Expression

Get minikube logs
> minikube logs

### AKS Azure Kubernetes Service

#### Connect command line to use kubectl

> az login
> az account set --subscription "Microsoft Azure"
> az aks get-credentials --resource-group compute --name synx-compute

#### Connect to Kubernetes dashboard

> az login
> az account list
> az account set --subscription "Microsoft Azure"
> az aks browse --resource-group compute --name synx-compute

### How to connect to Docker in minikube

run & minikube -p minikube docker-env | Invoke-Expression

### Setup complex environment locally

![complex setup architecture](img/2020-03-17-13-58-02.png)

### Production Deployment to Google Cloud

![production deployment workflow](img/2020-04-01-13-46-30.png)

#### Create Github repo

…or create a new repository on the command line
git init
git add .
git commit -m "first commit"
git remote add origin \<github repo uri\>
git push -u origin master

…or push an existing repository from the command line
git remote add origin \<github repo uri\>
git push -u origin master

#### Tell travis Ci to watch for code changes

- Refresh git repos
- create trigger to watch repo

#### Create Google Cloud objects

Goto google cloud console: <https://console.cloud.google.com/>

1. Create a new project named complexK8
2. Wait until the project is created and select it.
3. Enable billing for this project if it's not done already
4. Create a new Kubernetes Cluster
    1. Select the location of the cluster
    2. Create a 3 node cluster with standard machine configuration

#### Add deployment script tell Travis to deploy to Google Cloud

![Travis workflow to deploy to Google Cloud](img/2020-04-01-14-58-09.png)

This is more an overview. There are some steps missing.

#### Create a Google Cloud service account

![create a Google Cloud service account](img/2020-04-01-15-30-30.png)

- docker run -it -v ${pwd}:/app ruby:2.3 sh
- gem install travis --no-rdoc --no-ri
- gem install travis
- travis login
- Copy json file into the 'volumed' directory so we can use it in the container
- travis encrypt-file service-account.json -r agx540/complexK8_4 --com

#### Create a image version based on git SHA

![multitag docker images with git sha](img/2020-04-02-09-13-22.png)

#### Create secrets at google cloud kubernetes

> gcloud config set project complexk8
> gcloud config set compute/zone europe-west1-b
> gcloud container clusters get-credentials mult-cluster
> kubectl create secret generic pgpassword --from-literal PGPASSWORD=mypgpassword123

#### Using Helm

Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

Charts are easy to create, version, share, and publish — so start using Helm and stop the copy-and-paste.

For more information see <https://github.com/helm/helm>

![helm and tiller](img/2020-04-02-09-45-52.png)

##### Install helm FROM SCRIPT

> curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3\
> chmod 700 get_helm.sh\
> ./get_helm.sh

##### RBAC Role Based Access Control is activated by default in GKE (Google Kubernestes Engine)

![role based access control (rbac) control ](img/2020-04-02-09-55-38.png)

![rbac objects](2020-04-02-09-58-33.png)

kube-system namespace is more a administration namespace.

***How to allow tiller to change our cluster***

1. Create a Service Account
2. Create a ClusterRoleBinding
3. Tie the ClusterRoleBinding to the Service Account
4. Assign the Service Account to the tiller pod

> helm repo add stable https://kubernetes-charts.storage.googleapis.com/
> helm install my-nginx stable/nginx-ingress --set rbac.create=true

![created nginx objects](img/2020-04-02-10-45-14.png)

Ingress-controller reads config file and setup nginx.
Default-Backend is a default backend with health check in it.

![loadbalancer ip address](img/2020-04-02-10-48-02.png)

http://35.195.226.53/ is the ip to the default backend.

![google cloud load balancer which is created by ingress](img/2020-04-02-10-51-09.png)

Here you see google cloud load balancer.

### A Workflow for Changing in Production

![A Workflow for Changing in Production](img/2020-04-03-10-19-23.png)

1. Checkout a branch

> git checkout -b \<name of branch\>

example
> git checkout -b development

2. Change code and save

3. See if git recognizes your changes

> git status

4. Add changes to commit

> git add .

5. Commit changes

> git commit -m "some commit message"

6. Push commit to remote

> git push -u origin development

7. Travis builds development branch

8. Goto Github and create a pull request

9. Merge Pullrequest on Github to master

10. Travis now deploys it to GKE

### Setting up HTTPS

![LetsEncrypt Workflow](img/2020-04-03-11-12-12.png)

1. First you need an domain

2. Configure DNS

If you have domain you need to point it to your kubernetes ingress ip.\
To do so:

    2.1 Create a "A" record and point it to your ingress address

![DNS A RECORD](img/2020-04-03-14-17-34.png)

    2.2 Create a "CNAME" record and point it to your domain name
![DNS CNAME RECORD](img/2020-04-03-14-18-48.png)

3. Install Cert Manager

<www.github.com/jetstack/cert-manager>

see documentation how install cert-manager

![cert manager](img/2020-04-03-15-23-20.png)

- Cert Manager is a pod
- Certificate is an object in kuberenetes
- Issuer is an object in kuberenetes

4. Issuer config file

## -

## --

## ---

## ----

## -----

## ------

## -------

## --------

### Certified Kubernetes Application Developer (CKAD)

Maybe an useful link.

<https://medium.com/bb-tutorials-and-thoughts/practice-enough-with-these-questions-for-the-ckad-exam-2f42d1228552>

#### Core concepts 13%

#### Configuration 18%

#### Multi-Container Pods 10%

#### Observability 18%

#### Pod Design 20%

#### Services & Networking 13%

#### State Persistence 8%

## Linux Directory Structure

![Linux directory structure](img/2020-03-09-08-39-59.png)
<https://www.tecmint.com/linux-directory-structure-and-important-files-paths-explained/>

Each of the above directory (which is a file, at the first place) contains important information, required for booting to device drivers, configuration files, etc. Describing briefly the purpose of each directory, we are starting hierarchically.

- /bin : All the executable binary programs (file) required during booting, repairing, files required to run into single-user-mode, and other important, basic commands viz., cat, du, df, tar, rpm, wc, history, etc.
- /boot : Holds important files during boot-up process, including Linux Kernel.
- /dev : Contains device files for all the hardware devices on the machine e.g., cdrom, cpu, etc
- /etc : Contains Application’s configuration files, startup, shutdown, start, stop script for every individual program.
- /home : Home directory of the users. Every time a new user is created, a directory in the name of user is created within home directory which contains other directories like Desktop, Downloads, Documents, etc.
- /lib : The Lib directory contains kernel modules and shared library images required to boot the system and run commands in root file system.
- /lost+found : This Directory is installed during installation of Linux, useful for recovering -files which may be broken due to unexpected shut-down.
- /media : Temporary mount directory is created for removable devices viz., media/cdrom.
- /mnt : Temporary mount directory for mounting file system.
- /opt : Optional is abbreviated as opt. Contains third party application software. Viz., Java, etc.
- /proc : A virtual and pseudo file-system which contains information about running process with a particular Process-id aka pid.
- /root : This is the home directory of root user and should never be confused with ‘/‘
- /run : This directory is the only clean solution for early-runtime-dir problem.
- /sbin : Contains binary executable programs, required by System Administrator, for Maintenance. -Viz., iptables, fdisk, ifconfig, swapon, reboot, etc.
- /srv : Service is abbreviated as ‘srv‘. This directory contains server specific and service -related files.
- /sys : Modern Linux distributions include a /sys directory as a virtual filesystem, which stores -and allows modification of the devices connected to the system.
- /tmp :System’s Temporary Directory, Accessible by users and root. Stores temporary files for user and system, till next boot.
- /usr : Contains executable binaries, documentation, source code, libraries for second level -program.
- /var : Stands for variable. The contents of this file is expected to grow. This directory contains log, lock, spool, mail and temp files.

## Linux System Files

Linux is a complex system which requires a more complex and efficient way to start, stop, maintain and reboot a system unlike Windows. There is a well defined configuration files, binaries, man pages, info files, etc. for every process in Linux.

- /boot/vmlinuz : The Linux Kernel file.
- /boot/vmlinuz : The Linux Kernel file.
- /dev/hda : Device file for the first IDE HDD (Hard Disk Drive)
- /dev/hdc : Device file for the IDE Cdrom, commonly
- /dev/null : A pseudo device, that don’t exist. Sometime garbage output is redirected to /dev/null, so that it gets lost, forever.
- /etc/bashrc : Contains system defaults and aliases used by bash shell.
- /etc/crontab : A shell script to run specified commands on a predefined time Interval.
- /etc/exports : Information of the file system available on network.
- /etc/fstab : Information of Disk Drive and their mount point.
- /etc/group : Information of Security Group.
- /etc/grub.conf : grub bootloader configuration file.
- /etc/init.d : Service startup Script.
- /etc/lilo.conf : lilo bootloader configuration file.
- /etc/hosts : Information of Ip addresses and corresponding host names.
- /etc/hosts.allow : List of hosts allowed to access services on the local machine.
- /etc/host.deny : List of hosts denied to access services on the local machine.
- /etc/inittab : INIT process and their interaction at various run level.
- /etc/issue : Allows to edit the pre-login message.
- /etc/modules.conf : Configuration files for system modules.
- /etc/motd : motd stands for Message Of The Day, The Message users gets upon login.
- /etc/mtab : Currently mounted blocks information.
- /etc/passwd : Contains password of system users in a shadow file, a security implementation.
- /etc/printcap : Printer Information
- /etc/profile : Bash shell defaults
- /etc/profile.d : Application script, executed after login.
- /etc/rc.d : Information about run level specific script.
- /etc/rc.d/init.d : Run Level Initialisation Script.
- /etc/resolv.conf : Domain Name Servers (DNS) being used by System.
- /etc/securetty : Terminal List, where root login is possible.
- /etc/skel : Script that populates new user home directory.
- /etc/termcap : An ASCII file that defines the behaviour of Terminal, console and printers.
- /etc/X11 : Configuration files of X-window System.
- /usr/bin : Normal user executable commands.
- /usr/bin/X11 : Binaries of X windows System.
- /usr/include : Contains include files used by ‘c‘ program.
- /usr/share : Shared directories of man files, info files, etc.
- /usr/lib : Library files which are required during program compilation.
- /usr/sbin : Commands for Super User, for System Administration.
- /proc/cpuinfo : CPU Information
- /proc/filesystems : File-system Information being used currently.
- /proc/interrupts : Information about the current interrupts being utilised currently.
- /proc/ioports : Contains all the Input/Output addresses used by devices on the server.
- /proc/meminfo : Memory Usages Information.
- /proc/modules : Currently using kernel module.
- /proc/mount : Mounted File-system Information.
- /proc/stat : Detailed Statistics of the current System.
- /proc/swaps : Swap File Information.
- /version : Linux Version Information.
- /var/log/lastlog : log of last boot process.
- /var/log/messages : log of messages produced by syslog daemon at boot.
- /var/log/wtmp : list login time and duration of each user on the system currently.

## Linux command line

exit command line (like ctrl + c)
> ctrl + d

List directory
> ls

List current running processes (ps = process status)
> ps

Create a new file
> touch \<filename\>

See current directory
> pwd

## Windows Tools

List open ports on local machine
> netstat -an -b

Check Tcp port
> test-netconnection \<ip\> -Port \<Port\>

## Node.js sample project

Create a node.js web application. Run it in a container and
access it from browser in hosting environment.

see .\4.Node.js_project folder for project source code

![steps to create sample project](img/2020-03-02-09-40-16.png)
