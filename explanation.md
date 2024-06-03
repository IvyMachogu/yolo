base image node:13-alphine on both containers client and backend 
navigate to client  in the terminal run 
create a docker file with the following 
### touch Dockerfile 

### touch Dockerfile

### FROM node:13-alpine 

### WORKDIR /usr/src/app

### COPY package*.json ./

### COPY . .

### expose  3000

### RUN npm install

### CMD ["npm", "start", "--", "--host", "0.0.0.0", "--port", "3000"]

### docker build -t ivymachogu/yolo-client:v1.1.1 .

running the container 

### docker run -p 3000:3000 ivymachogu/yolo-client:v1.1.1

incase you encounter an error  run
### docker run  -it -p 3000:3000 ivymachogu/yolo-client:v1.1.1

navigate to backend 

create a dockerfile with the following 
### touch Dockerfile

### FROM node:13-alpine 

### WORKDIR /usr/src/app

### COPY package*.json ./

### COPY . .

### expose  3000

### RUN npm install

### CMD ["npm", "start", "--", "--host", "0.0.0.0", "--port", "3000"]

creating base image  

### docker run -p 5000:5000 ivymachogu/yolo-backend:v1.1.1 .

running the container 


### docker run  -it -p 5000:5000 ivymachogu/yolo-backend:v1.1.1

navigate to yolo create docker-composer.yaml
 add the following 

 version: '3.8'

services:
  yolo.client:
    build: ./client
    ports:
      - "3000:3000"
    networks:
      -  yolo_network

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    networks:
      - yolo_network

networks:
  yolo_network:
    driver: bridge


   *** run the command in the terminal to up the composer  
### docker compose up  --build 

incase you get the port confilicting run 
###  docker ps -a
 ### docker stop "container id "
and  then run 
### docker composer up --build 




# KUBERNETES
This project is about running the yolo client, backend and database container on GKE cluster 

 
## Step 1 - Setup default computer zone
* This is the regional location where the cluster will operate
* Setup default region
    - gcloud config set compute/region us-east1

* Set up default zone
   - gcloud config set compute/zone us-east1-d

## Step 2 - Create cluster 
   - gcloud container clusters create --machine-type=e2-medium --zone=us-east1-d yolo-cluster

## Step 3 - Authenticate for cluster created
   - gcloud container clusters get-credentials yolo-cluster

## Step 4 - Clone yolo repo into GCP 

## Step 5 - Run the manifest files 

// creating pvc 
 kubectl apply -f pvc.yaml

//creating deployments
 kubectl apply -f yolo-backend.yaml 
 kubectl apply -f yolo-client.yaml 

//creating statefulset for mongodb 
 kubectl apply -f  mongodb.yaml 

//creating services 
 kubectl apply -f backend_svc.yaml 
 kubectl apply -f client_svc.yaml 
 kubectl apply -f mongodb_svc.yaml





