# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Remote access for Chronicle Map RFC                                     |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Map/Remote/Remote-Chronicle-Map-0.1.md |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
An RFC for library specific requirements for Remote access to a Chronicle Map. See the parent RFC for cross library requirements.

## Remote access.
This section details the services a Chronicle Map exposes and wire protocol for the methods.

### Services
| Service           | Relative URI                  |
| ----------------- | ----------------------------- |
| Unqualified Map   | map-name                      |
| Qualified Map     | map-name#Map                  |
| Keys of Map       | map-name#keySet               |
| Entries of Map    | map-name#entrySet             |
| Values of Map     | map-name#values               |
| Replication       | map-name#replication          |

These proxies can be returned by methods and should be serialized when sending to the server

### Method calls
In all the examples below the `csp:` can be replaced with a `cid:` which is a short token to improve the efficiency of the routing logic.

### Temporary Map
If the csp starts with //tmp then the map is deemed to be a temporary map which is not persisted or replicated.

```yaml
--- !!meta-data
csp://tmp/path/map-name#Map
# or
cid: 1212
tid: 123456789
```

#### Map proxies
For entrySet(), the client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
# or
cid: 1212
tid: 123456789
--- !!data
entrySet: { }
```

The server replies
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: !set-proxy { csp://server/path/map-name#entrySet, cid: 1234 }
```

For keySet(), the client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
# or
cid: 1212
tid: 123456789
--- !!data
keySet: { }
```

The server replies
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: !!proxy { csp://server/path/map-name#keySet, cid: 1235 }
```
The sample applies to `values()` returning `csp://server/path/map-name#values`


#### Map exmaple of a Void method reply
```yaml
--- !!meta-data
csp://marshallable-keys#MAP
tid: 1430477448916
--- !!data
clear: {  }
```

server responds

```yaml
--- !!meta-data
tid: 1430477448916
--- !!data
reply: {}
```

#### Map get
The client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
--- !!data
get: 1
```

The server replies with the payload.
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: Bonjour
```

#### Map put
If the client expects a reply
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
--- !!data
getAndPut: { key: 1, value: Hello }
```

The server sends back
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: !!null     # for no return
```

Or if the server gets an error
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: !IllegalArgumentException "Invalid key type"
```


or the server sends back the previous value
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: Bonjour
```

If the client doesn't expect a reply
```yaml
--- !!meta-data
csp://server/path/map-name#Map
--- !!data
put: { key: 1, value: Hello }
```

The server doesn't send anything.

#### Map putAll
Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
--- !!data
putAll: 
  - { key: 1, value: hello }
  - { key: 2, value: world }
```
 
The server doesn't send back a reply.

#### Map putAll of a proxy

Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
--- !!not-ready-data
putAll: csp://server/path/map-name2#Map
```

The server doesn't send back a reply.

#### Map size and isEmpty
To determine the size of the map or if the map is empty. `isEmpty()` can test `size() > 0`

Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
--- !!data
size: { }
```

The server replies with
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: 1212121
```

#### Map toString()
Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
--- !!data
toString: { }
```

For short replies, the server replies with
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: "{ 1=Hello, 2=World }"
```

For long replies, the server replies with
```yaml
--- !!meta-data
tid: 123456789
--- !!not-ready-data
reply: "{ 1=Hello,"
--- !!not-ready-data
reply-append: "2=World,"
--- !!data
reply-append: "3=Bye }"
```
#### Map remove
Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
--- !!data
remove: 12
```
or this if they want to send the data back

```yaml
--- !!meta-data
csp://server/path/map-name#Map
--- !!data
getAndremove: 12
```

or using the keySet()

```yaml
--- !!meta-data
csp://server/path/map-name#keySet()
tid: 123456789
--- !!data
getAndremove: 12
```

and the server replies
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: { 1=Hello, 2=World }
```

#### iterator()
The client asks the server for the number of segments, the client will then ask for all the data in each segment calling iterator(<segment number>), typically a server will have more than one segment however for this example we have simplified the server to a single segment.

client writes:

```yaml
--- !!meta-data
cid: 1
tid: 1430233660656
--- !!data
numberOfSegements: {  }
```
server writes '1' ( the number of segments ) :

```yaml
--- !!meta-data
tid: 1430233660656
--- !!data
reply: 1  
```

client the subsribes the requests all the records in the first segment:
```yaml
--- !!meta-data
cid: 1
tid: 1430233660660
--- !!data
iterator: 1
``` 

server replies:
```yaml
--- !!meta-data
tid: 1430233660660
--- !!data
reply: 
  - { key:2 value:B }
  - { key:1 value:A }
  - { key:3 value:C }
  - { key:5 value:E }
  - { key:4 value:D }
``` 

```yaml
--- !!meta-data
csp://server/path/map-name#entrySet
tid: 123456789
--- !!not-ready-data
iterator: { }
```
The server replies with
```yaml
--- !!meta-data
tid: 123456789
--- !!data
entry:
 - { key: 1, value: Hello1 }
 - { key: 2, value: Hello2 }
 - { key: 3, value: Hello3 }pu
 - { key: 4, value: Hello4 }
 - { key: 5, value: Hello5 }
```
Once the client has received this:
```yaml
--- !!meta-data
csp://server/path/map-name#entrySet
--- !!data
iterator-continue: { tid: 123456789 }
```
And the server give some more.
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply:
 - { key: 6, value: Hello6 }
 - { key: 7, value: Hello7 }
 - { key: 8, value: Hello8 }
 - { key: 9, value: Hello9 }
 - { key: 10, value: Hello10 }
```

The client could prefetch an additional block from the start.
```yaml
--- !!meta-data
csp://server/path/map-name#entrySet
tid: 123456789
--- !!not-ready-data
iterator: { }
iterator-continue: { tid: 123456789 }
```
This causes two blocks to be in flight at once reducing the stop-start behaviour for downloading.

#### Replication
The sender pushes an update
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
--- !!data
update: { key: 12, value: "Crazy", timestamp: 1438276872, id: 5 }
```

The sender pushes a remove
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
--- !!data
remove: { key: 12, timestamp: 1438276872, id: 5 }
```

The sender requests being notified of updates.
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
--- !!data
subscribe: { all: true }
```

or to unsubscribe All
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
--- !!data
unsubscribe: { all: true }
```

Future versions will need to subscribe selectively.

#### Key and Values as Maps

example using getAndPut :

```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
--- !!data
getAndPut: { key: 1, value: !!cmap
  - { key: key1, value: value1 }
  - { key: key2, value: value2 }
  - { key: key3, value: value3 }
  - { key: key4, value: value4 }
}
```


#### Exceptions

and example of a Java exception in YAML

```yaml
client reads
--- !!meta-data
tid: 1431103012234
--- !!data
exception: !java.lang.NullPointerException { message: !!null }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor, method: nullCheck, file: MapWireHandlerProcessor.java, lineNumber: 397 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor, method: access$700, file: MapWireHandlerProcessor.java, lineNumber: 51 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor$2, method: lambda$accept$9, file: MapWireHandlerProcessor.java, lineNumber: 269 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor$2$$Lambda$28/1591363695, method: accept, file: !!null, lineNumber: -1 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor, method: lambda$writeData$12, file: MapWireHandlerProcessor.java, lineNumber: 426 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor$$Lambda$29/377100951, method: accept, file: !!null, lineNumber: -1 }
- { class: net.openhft.chronicle.wire.Wires, method: writeData, file: Wires.java, lineNumber: 57 }
- { class: net.openhft.chronicle.wire.WireOut, method: writeDocument, file: WireOut.java, lineNumber: 71 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor, method: writeData, file: MapWireHandlerProcessor.java, lineNumber: 422 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor, method: access$200, file: MapWireHandlerProcessor.java, lineNumber: 51 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor$2, method: accept, file: MapWireHandlerProcessor.java, lineNumber: 212 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor$2, method: accept, file: MapWireHandlerProcessor.java, lineNumber: 201 }
- { class: net.openhft.chronicle.wire.Wires, method: lambda$readData$48, file: Wires.java, lineNumber: 102 }
- { class: net.openhft.chronicle.wire.Wires$$Lambda$26/1776558482, method: accept, file: !!null, lineNumber: -1 }
- { class: net.openhft.chronicle.bytes.StreamingCommon, method: withLength, file: StreamingCommon.java, lineNumber: 52 }
- { class: net.openhft.chronicle.wire.Wires, method: readData, file: Wires.java, lineNumber: 102 }
- { class: net.openhft.chronicle.wire.WireIn, method: readDocument, file: WireIn.java, lineNumber: 77 }
- { class: net.openhft.chronicle.wire.map.MapWireHandlerProcessor, method: process, file: MapWireHandlerProcessor.java, lineNumber: 78 }
- { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler, method: process, file: EngineWireHandler.java, lineNumber: 128 }
- { class: net.openhft.chronicle.network.WireTcpHandler, method: processMessage, file: WireTcpHandler.java, lineNumber: 95 }
- { class: net.openhft.chronicle.network.WireTcpHandler, method: process, file: WireTcpHandler.java, lineNumber: 55 }
- { class: net.openhft.chronicle.network.TcpEventHandler, method: invokeHandler, file: TcpEventHandler.java, lineNumber: 88 }
- { class: net.openhft.chronicle.network.TcpEventHandler, method: runOnce, file: TcpEventHandler.java, lineNumber: 76 }
- { class: net.openhft.chronicle.network.event.VanillaEventLoop, method: runAllHighHandlers, file: VanillaEventLoop.java, lineNumber: 123 }
- { class: net.openhft.chronicle.network.event.VanillaEventLoop, method: run, file: VanillaEventLoop.java, lineNumber: 94 }
- { class: java.util.concurrent.Executors$RunnableAdapter, method: call, file: Executors.java, lineNumber: 511 }
- { class: java.util.concurrent.FutureTask, method: run, file: FutureTask.java, lineNumber: 266 }
- { class: java.util.concurrent.ThreadPoolExecutor, method: runWorker, file: ThreadPoolExecutor.java, lineNumber: 1142 }
- { class: java.util.concurrent.ThreadPoolExecutor$Worker, method: run, file: ThreadPoolExecutor.java, lineNumber: 617 }
- { class: java.lang.Thread, method: run, file: Thread.java, lineNumber: 745 }
```
