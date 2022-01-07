# UdaConnect
## Background
Conferences and conventions are hotspots for making connections. Professionals in attendance often share the same interests and can make valuable business and personal connections with one another. At the same time, these events draw a large crowd and it's often hard to make these connections in the midst of all of these events' excitement and energy. To help attendees make connections, we are building the infrastructure for a service that can inform attendees if they have attended the same booths and presentations at an event.

## The Task
You work for a company that is building an app that uses location data from mobile devices. Your company has built a Proof of concept (POC) application to ingest location data named UdaConnect. This POC was built with the core functionality of ingesting location and identifying individuals who have shared close geographic proximity.Below is the UdaConnect App diagram:
![image](https://user-images.githubusercontent.com/37039539/148497400-14c15b42-b107-4e36-b24d-c85eec221801.png)


Management loved the POC, so now that there is buy-in, we want to enhance this application. You have been tasked to enhance the POC application into a Minimum Viable Product (MVP) to handle the large volume of location data that will be ingested.

To do so, you will refactor this application into a microservice architecture using message passing techniques that you have learned in this course

# Pre-requisite
* 1. Setup The Environment
Install the tools to get our environment set up properly.
If you haven't already done so:
1. Install Docker
2. Set up a DockerHub account
3. Set up kubectl
4. Install VirtualBox with at least version 6.0
5. Install Vagrant with at least version 2.0

* 2. Initialize K3s
To run the application, you will need a K8s cluster running locally and interface with it via kubectl. We will be using Vagrant with VirtualBox to run K3s.

In this project's root, run vagrant up.
'vagrant up'
The command will take a while and will leverage VirtualBox to load an openSUSE OS and automatically install K3s. Some useful vagrant commands can be found in this cheatsheet. A good introduction to Vagrant can be found in this article.

* 3. Retrieve the Kubernetes config File
After vagrant up is done, you will SSH into the Vagrant environment and retrieve the Kubernetes config file used by kubectl. We want to copy this file's contents into our local environment so that kubectl knows how to communicate with the K3s cluster.

We want to copy this file's contents into our local environment so that kubectl knows how to communicate with the K3s cluster.

* 4. Configure kubectl
Create the file ~/.kube/config (or replace it if it already exists).

Paste the contents of the k3s.yaml output into the config file.

Test that kubectl works by running the command

kubectl describe services

* 5. Deploy kubectl
kubectl apply -f deployment/db-configmap.yaml - Set up environment variables for the pods
kubectl apply -f deployment/db-secret.yaml - Set up secrets for the pods
kubectl apply -f deployment/postgres.yaml - Set up a Postgres database running PostGIS
kubectl apply -f deployment/udaconnect-api.yaml - Set up the service and deployment for the API
kubectl apply -f deployment/udaconnect-app.yaml - Set up the service and deployment for the web app
sh scripts/run_db_command.sh <POD_NAME> - Seed your database against the postgres pod. (kubectl get pods will give you the POD_NAME)

'kubectl apply -f deployment/'

* 6. Seed the Database
The first time you run this project, you will need to seed the database with dummy data. Use the command:

sh scripts/run_db_command.sh <POD_NAME>
against the postgres pod. (kubectl get pods will give you the POD_NAME). Subsequent runs of kubectl apply to make changes to deployments or services will not require you to seed the database again!

* 7. Verify Your Deployment
Once the project is up and running, you should be able to see three deployments and three services in Kubernetes. To verify, run the following commands:

kubectl get pods
kubectl get services
Both should both return udaconnect-app, udaconnect-api, and postgres.

## These pages should also load on your web browser:

http://localhost:30001/ - OpenAPI Documentation

http://localhost:30001/api/ - Base path for API

http://localhost:30000/ - Frontend ReactJS Application

## Generating gRPC files
`pip install grpcio-tools`

`python -m grpc_tools.protoc -I./ --python_out=./ --grpc_python_out=./ order.proto`

##Summary_1
Protobuf File
proto3 is specified as the syntax
OrderMessage uses enums Status and Equipment to add string validation
Empty message is defined to help handle null values where an empty argument is passed to the service
OrderMessageList helps capture multiple OrderMessage's
OrderService defines Create and Get stubs
OrderService.Get has no argument so it uses the Empty message as its input


##Summary_2
Server Code
Server code is defined in main.py using mostly boilerplate code to set up a gRPC server
OrderServicer.Get returns hardcoded data by creating two order_pb2.OrderMessage objects and returning them in the OrderMessageList
OrderServicer.Create returns the same data that was passed in
Running the gRPC Server
python main.py will run the gRPC server and print its outputs
Create gRPC Client
Writer
Writer is used to demonstrate how OrderServicer.Create is called
Defined in writer.py
Creates an OrderMessage and passes it to OrderServicer.Create
Getter
Getter is used to demonstrate how OrderServicer.Get is called
Defined in getter.py
Creates an Empty message and passes it to OrderServicer.Get
Running gRPC Clients
python getter.py returns a hard-coded OrderMessageList
python writer.py returns the data that we passed in
gRPC server behavior can be validated as it prints the output
