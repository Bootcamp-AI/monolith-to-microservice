# Monolith to Microservices

## NOTE: This is not an officially supported Google product

## Introduction

### 1. Introduction

Running websites and applications is hard.

Things go wrong when they shouldn't, servers crash, increase in demand causes more resources to be utilized, and making changes without downtime is complicated and stressful.

Imagine a tool that could help you do all that and even allow you to automate it! With GKE, all of that is not only possible, it's easy! In this codelab, you assume the role of a developer running an ecommerce website for a fictional company—Fancy Store. Due to problems with scaling and outages, you're tasked with deploying your application to GKE!

The exercises are ordered to reflect a common cloud developer's experience:

    Create a GKE cluster.
    Create a Docker container.
    Deploy the container to GKE.
    Expose the container via a service.
    Scale the container to multiple replicas.
    Modify the website.
    Roll out a new version with zero downtime.

Architecture diagram

What you'll learn

    How to create a GKE cluster
    How to create a Docker image
    How to deploy Docker images to Kubernetes
    How to scale an application on Kubernetes
    How to perform a rolling update on Kubernetes

Prerequisites

    A Google Account with administrative access to create projects or a project with a project-owner role
    A basic understanding of Docker and Kubernetes (If you lack a basic understanding, then please review Docker and Kubernetes now.)


## 2. Environment setup
.....


## Setup

```
git clone https://github.com/googlecodelabs/monolith-to-microservices
cd monolith-to-microservices
./setup.sh
```

## Monolith

#### To run the monolith project use the following commands from the top level directory:

```
cd monolith
npm start
```

#### You should see output similar to the following:

```
Monolith listening on port 8080!
```

#### That's it! You now have a perfectly functioning monolith running on your machine!

### Docker

#### To create a Docker image for the monolith, execute the following commands:

```
cd monolith
docker build -t monolith:1.0.0 .
```

#### To run the Docker image, execute the following commands:

```
docker run --rm -p 8080:8080 monolith:1.0.0
```


## Microservices

### To run the microservices project use the following commands from the top level directory:

```
cd microservices
npm start
```

### You should see output similar to the following:

```
[0] Frontend microservice listening on port 8080!
[2] Orders microservice listening on port 8081!
[1] Products microservice listening on port 8082!
```

### That's it! You now have a perfectly functioning set of microservices running on your machine!

### Docker

#### To create a Docker image for the mmicroservices, you will have to create a Docker image for each service. Execute the following commands for each folder under the microservices folder.

```
cd microservices/src/frontend
docker build -t frontend:1.0.0 .

cd ../products
docker build -t products:1.0.0 .

cd ../orders
docker build -t orders:1.0.0 .
```

#### To run the Docker image, execute the following commands:

```
docker run -d --rm -p 8080:8080 monolith:1.0.0
docker run -d --rm -p 8081:8081 orders:1.0.0
docker run -d --rm -p 8082:8082 products:1.0.0
```

#### To stop the containers, you will need to find the CONTAINER ID for each and stop them individually. See the steps below:

```
docker ps -a

CONTAINER ID        IMAGE                        COMMAND                CREATED
4c01db0b339c        frontend:1.0.0               bash                   17 seconds ago
d7886598dbe2        orders:1.0.0                 bash                   17 seconds ago
d85756f57265        products:1.0.0               bash                   17 seconds ago

docker stop 4c01db0b339c
docker stop d7886598dbe2
docker stop d85756f57265
```


## React App

#### The react-app folder contains a React application created from `create-react-app`. You can modify this fronted, but afterwards, you will need to build and move the static files to the monolith and microservices project. You can do this by running the standard create-react-app build command below:

```
npm run build
```

#### This will run the build script to create the static files two times. The first will build with relative URLs and copy the static files to the monolith/public folder. The second run will build with the standard microservices URLs and copy the static files to the microservices/src/frontned/public folder.
