# Specification

|         |                                                                                         |
|:------- | --------------------------------------------------------------------------------------- |
| Title   | TCP Connection Modes                                                                    |
| URL     | https://github.com/OpenHFT/RFC/blob/master/TCP-Modes/TCP-Modes-0.1.md                   |
| Editor  | Peter Lawrey. Full edit by Neil Clifford 26/07/2017                                     |
| License | Apache 2.0                                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration                         |
| Status  | Raw.                                                                                    |

# Goals
TCP modes express the different varieties of connection, and thread modes, which may be useful in the interaction between clients/servers, and between peers.

# Symmetry
Connections between client and server tend to be asymmetric. That is, the way that a connection is handled and reused on the client, and on the server, tends to be different.

However for server to server connections between peers, the handling of those connections should be symmetric. Any inconsistency between how servers handle the connection is a likely source of error, and  becomes another use-case that requires testing.

# Connection binding
Multiple connections between two nodes can be bound together to make a meta-connection.  This is useful when mixing different types of traffic. For example, low priority and high priority, or RPC and subscription. Binding is performed by providing a unique identifier, such as a UUID, or a configured hostId. When multiple connections are identfied as coming from the same remote node, they can be bound together.

If an initiator requires isolated connections, they can provide multiple identifiers.

# Symmetric connections

## Plain TCP Socket.
When a connection has been established, a socket connection behaves the same whether the end point was the initiator, or the acceptor.

The only difference might be who is expected to reconnect in the event of a failure. If the relationship is fully symmetric, both ends need to be able to initate and accept.

## With one thread
When there is a thread at each end, there must be, at the very least, a reader thread.  This could be a dedicated thread, or a shared thread.

If there is only a reader thread, there could be any number of threads that can write.

Note: If the reader also writes a result/outcome to the same TCP connection, care needs to be taken to handle the buffer filling up.
Without this, the reader at both ends of the TCP connection can block, waiting for the write at each end.  You can increase the buffer sizes, or add additional buffers, but there is always the risk of either a deadlock, or the buffer getting too large, while waiting for the reader.

## With two threads
With two threads you can have a queue of messages to send;  a writing thread, and a reading thread consuming messages. Using this model, while both the reading and writing threads could be shared, they shouldn't be shared with each other.

## Heartbeats
In a symmetric connection, heartbeats should go in both directions.  A heartbeat only needs to be sent when nothing has been read for some period of time.

# Asymmetric connections
With asymmetric connections, one end acts as a server which provides a service to a client. The connection exists to serve the client.

Note: This is not the same as the initator which creates the connection, and the acceptor which accepts the connection on a known port.
Typically, the client initiates the connection to a server, which in turn accepts it. However, the connection could be created the other way around.

## Plain RPC handler.
The simplest asymmetric connection is to have thread on the server side. This thread reads, processes the request, and writes the response. This is the only thread which corresponds with the other. Due to the locking model, this can be the lowest latency solution.

## Subscription handler
For a subscription handling connection, the only thread required is the reading-consumer thread.
The connection might have a short-lived reader on the server side to set up the subscription; or subscriptions could be managed by a separate RPC connection which is bound to it.

## A combined RPC and Subscription
The connection multiplexes both RPC and subscriptions.

## Heartbeats
For asymmetric connections, the only assumed thread is a reading thread; this is either for the client (Subscription), or the server (RPC). As such, the heartbeat needs to be sent from the other end, or by using another bound connection.

# Explicit Flow control.
In the case of either, a buffered message, or stored content, explicit flow control could be employed.
Before sending data, the other end requests the method of flow control to be used.
This can be specified in terms of MB per second, or messages, or some combination of the two.

# Handshaking
When an initator starts a connection it must provide:
- the type of connection expected:
   - Symmetric or Asymmetric
   - no thread, reader thread, or reader & writer thread
- the unique id, if binding is required
- the heartbeat interval that it expects, if required