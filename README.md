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
