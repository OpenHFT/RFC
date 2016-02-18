# Specification

|         |                                                                                         |
|:------- | --------------------------------------------------------------------------------------- |
| Title   | TCP Connection Modes                                                      |
| URL  | https://github.com/OpenHFT/RFC/blob/master/TCP-Modes/TCP-Modes-0.1.md |
| Editor  | Peter Lawrey                                                                            |
| License | Apache 2.0                                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration                         |
| Status  | Raw.                                                                                    |

# Goals
TCP Modes express the different varieties of connection and thread modes which may be useful in the interaction before client/server and between piers.

# Symmetry
Connections between client and server tend to be asymmetric. i.e. how the connection is handled and reused on the client and server tends to be different.

However for server-server connections between peirs, the handling of those connections should be symmetric as any inconsistency
between how servers handle the connection is a likely source of error and another use case to test. i.e. symmetric connection remove a dimension
 of combinations you need to test for.
 
# Connection binding
A number of connections between two node can be bound together to make a meta connection.  This is useful when mixing differnt types of traffic e.g.
low priority and high priority, RPC and subscription. Binding is performed by providing a unique id such as a UUID or configured hostId. When multiple
connections are identfied as coming from the same remote node, they can be bound together.

If an initiator want isolated connections, theyc an prodive multiple ids.
 
# Symmetric connections

## Plain TCP Socket.
Once a connection has been established, a socket connection behaves the same whether the end point was the initiator or the acceptor.

 The only difference might be whom is expected to reconnect in the event of a failure. If the relationship is fully symmetric, both end need to be able to
 initate and both ends accept.

## With one thread
When there is a thread at each end, there must at the very least be a Reading thread.  This could be a dedicated thread, or a shared thread.

 If there is only a reader thread, there could be any number of thread which can write.

 Note: if the reader also write a result/outcome to the same TCP connection, care need to be take to handle the buffer filling up.
 Without this the reader at both ends of the TCP connect can block waiting for the write at each end.  You can increase the buffer sizes
 or add additional buffers but there is always the risk of either a deadlock or the buffer getting too large waiting for the reader.

## With two threads
With two thread syou can have a queue of message to send writing with one thread and a reading thread consuming messages.
  While both the reading and writing threads could be shared, they shouldn't be shared with each other in this model.

## Heartbeats
In a symmetric connection heartbeats should go in both directions.  A heartbeat only needs to be sent when nothing has been read for
  some period of time.

# Asymmetric connections
With asymmetric connections, one end acts as a server which provides a service to a client.  The connection exists to serve the client.

Note: this is not the same as the initator which creates the connection and the acceptor which accept shte connection on a known port.  
Typically the client initiates the connection to a server which accepts, however the connection could be created the other way around.

## Plain RPC handler.
The simplest asymmetric connection is to have thread on the server side. This thread reads, processes the request and writes the response. This is the
  only thread which corrisponds with the thread.  Due to the locking model, this can be the lowest latency solution.
  
It is the clients respon

## Subscription handler
For a subscription handling connection, the only thread required is the reading consumer.  

The connection might have a short lived reader on the server side to set up the subscription, or subscriptions could be managed by a seperate RPC connections
 which is bound to this connection.
