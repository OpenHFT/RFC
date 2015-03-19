# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Chronicle Map RFC                                                       |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Map/Chronicle-Map-0.1.md |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
An RFC for library specific requirements for Chronicle Map. See the parent RFC for cross library requirements.

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

#### Map proxies
For entrySet(), the client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
# or
cid: 1212
tid: 123456789
...
--- !!data
entrySet: { }
...
```

The server replies
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: !!set-proxy { csp://server/path/map-name#entrySet, cid: 1234 }
...
```

For keySet(), the client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
# or
cid: 1212
tid: 123456789
...
--- !!data
keySet: { }
...
```

The server replies
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: !!proxy { csp://server/path/map-name#keySet, cid: 1235 }
...
```
The sample applies to `values()` returning `csp://server/path/map-name#values`

#### Map get
The client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
...
--- !!data
get: { key: 1 }
...
```

The server replies with the payload.
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: Bonjour
...
```

#### Map put
If the client expects a reply
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
...
--- !!data
getAndPut: { key: 1, value: Hello }
...
```

The server sends back
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: !!null     # for no return
...
```

Or if the server gets an error
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: !IllegalArgumentException "Invalid key type"
...
```


or the server sends back the previous value
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: Bonjour
...
```

If the client doesn't expect a reply
```yaml
--- !!meta-data
csp://server/path/map-name#Map
...
--- !!data
put: { key: 1, value: Hello }
...
```

The server doesn't send anything.

#### Map putAll
Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
...
--- !!not-ready-data
put: { key: 1, value: Hello1 }
put: { key: 2, value: Hello2 }
put: { key: 3, value: Hello3 }
put: { key: 4, value: Hello4 }
put: { key: 5, value: Hello5 }
...
--- !!data
put: { key: 6, value: Hello6 }
put: { key: 7, value: Hello7 }
put: { key: 8, value: Hello8 }
put: { key: 9, value: Hello9 }
put: { key: 10, value: Hello10 }
...
```

The server doesn't send back a reply.

#### Map putAll of a proxy

Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
...
--- !!not-ready-data
putAll: { csp://server/path/map-name2#Map, cid: 2233 }
...
```

The server doesn't send back a reply.

#### Map size and isEmpty
To determine the size of the map or if the map is empty. `isEmpty()` can test `size() > 0`

Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
...
--- !!not-ready-data
size: { }
...
```

The server replies with
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: 1212121
...
```

#### Map toString()
Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
tid: 123456789
...
--- !!data
toString: { }
...
```

For short replies, the server replies with
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: "{ 1=Hello, 2=World }"
...
```

For long replies, the server replies with
```yaml
--- !!meta-data
tid: 123456789
...
--- !!not-ready-data
reply: "{ 1=Hello,"
...
--- !!not-ready-data
reply-append: "2=World,"
...
--- !!data
reply-append: "3=Bye }"
...
```
#### Map remove
Client sends
```yaml
--- !!meta-data
csp://server/path/map-name#Map
...
--- !!data
remove: { key: 12, reply: false }
...
```

or using the keySet()

```yaml
--- !!meta-data
csp://server/path/map-name#keySet()
tid: 123456789
...
--- !!data
remove: { element: 12, reply: true }
...
```

and the server replies
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: "{ 1=Hello, 2=World }"
...
```

#### Replication
The sender pushes an update
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
...
--- !!data
update: { key: 12, value: "Crazy", timestamp: 1438276872, id: 5 }
...
```

The sender pushes a remove
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
...
--- !!data
remove: { key: 12, timestamp: 1438276872, id: 5 }
...
```

The sender requests being notified of updates.
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
...
--- !!data
subscribe: { all: true }
...
```

or to unsubscribe All
```yaml
--- !!meta-data
csp://server/path/map-name#replication
# or
cid: 2121
...
--- !!data
unsubscribe: { all: true }
...
```

Future versions will need to subscribe selectively.
