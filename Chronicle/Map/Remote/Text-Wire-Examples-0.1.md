# Text Wire Examples

### Get
example of get(<key>) returning null, when the keys is not present in the map

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164086110
--- !!data
get: 42
```

receives:

```yaml
--- !!data
reply: !!null ""
```

### Contains Key _ Null Pointer Exception
c.containsKey(null) will throw a NullPointerException

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164090328
--- !!data
containsKey: !!null ""
```

receives:

```yaml
--- !!data
exception: !java.lang.NullPointerException {
  message: !!null "",
  stackTrace: [
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: nullCheck, file: MapWireHandler.java, line: 401 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1, method: lambda$accept$10, file: MapWireHandler.java, line: 264 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1$$Lambda$65/763926653, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: lambda$writeData$13, file: MapWireHandler.java, line: 425 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$$Lambda$66/1753875368, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.wire.Wires, method: writeData, file: Wires.java, line: 78 },
    { class: net.openhft.chronicle.wire.WireOut, method: writeDocument, file: WireOut.java, line: 68 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: writeData, file: MapWireHandler.java, line: 421 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1, method: accept, file: MapWireHandler.java, line: 201 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1, method: accept, file: MapWireHandler.java, line: 173 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: process, file: MapWireHandler.java, line: 78 },
    { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler, method: lambda$process$105, file: EngineWireHandler.java, line: 159 },
    { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler$$Lambda$27/116815206, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.wire.Wires, method: lambda$readData$38, file: Wires.java, line: 120 },
    { class: net.openhft.chronicle.wire.Wires$$Lambda$58/630778964, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.bytes.StreamingCommon, method: withLength, file: StreamingCommon.java, line: 52 },
    { class: net.openhft.chronicle.wire.Wires, method: readData, file: Wires.java, line: 120 },
    { class: net.openhft.chronicle.wire.WireIn, method: readDocument, file: WireIn.java, line: 75 },
    { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler, method: process, file: EngineWireHandler.java, line: 153 },
    { class: net.openhft.chronicle.network.WireTcpHandler, method: read, file: WireTcpHandler.java, line: 100 },
    { class: net.openhft.chronicle.network.WireTcpHandler, method: process, file: WireTcpHandler.java, line: 61 },
    { class: net.openhft.chronicle.network.TcpEventHandler, method: invokeHandler, file: TcpEventHandler.java, line: 91 },
    { class: net.openhft.chronicle.network.TcpEventHandler, method: runOnce, file: TcpEventHandler.java, line: 79 },
    { class: net.openhft.chronicle.network.event.VanillaEventLoop, method: runAllHighHandlers, file: VanillaEventLoop.java, line: 125 },
    { class: net.openhft.chronicle.network.event.VanillaEventLoop, method: run, file: VanillaEventLoop.java, line: 95 }
  ]
}
```

### Entry Set
The client asks the server for the number of segments, the client will then ask for all the data in each segment calling iterator(), typically a server will have more than one segment however for this example we have simplified the server to a single segment.

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164092575
--- !!data
entrySet: {
}
```

receives:

```yaml
--- !!data
reply: !set-proxy {
  csp: //test?view=entrySet,
  cid: 1
}
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164092581
--- !!data
numberOfSegments: {
}
```

receives:

```yaml
--- !!data
reply: 1
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164092586
--- !!data
iterator: 1
```

receives:

```yaml
--- !!data
reply: [
    {
    key: 4,
    value: D
},
    {
    key: 5,
    value: E
},
    {
    key: 2,
    value: B
},
    {
    key: 3,
    value: C
},
    {
    key: 1,
    value: A
}
]
```

### Replace Value 2
example of replace where the value is known

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164097049
--- !!data
replaceForOld: {
  key: 1,
  oldValue: A,
  newValue: Z
}
```

receives:

```yaml
--- !!data
reply: true
```

### Clear
sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164099285
--- !!data
clear: {
}
```

receives:

```yaml
--- !!data
reply: {
}
```

### Size
size on an empty map

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164110010
--- !!data
size: {
}
```

receives:

```yaml
--- !!data
reply: 0
```

size on a map with entries

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164110016
--- !!data
size: {
}
```

receives:

```yaml
--- !!data
reply: 5
```

### Contains Key
example of containsKey(<key>) returning true

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164116483
--- !!data
containsKey: 1
```

receives:

```yaml
--- !!data
reply: true
```

### Put If Absent
sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164118718
--- !!data
putIfAbsent: {
  key: 6,
  value: Z
}
```

receives:

```yaml
--- !!data
reply: !!null ""
```

### Contains Value
example of containsValue(<value>) returning true

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164120965
--- !!data
containsValue: A
```

receives:

```yaml
--- !!data
reply: true
```

Test 'net.openhft.chronicle.engine.map.RemoteChronicleMapTextWireTest.testValuesToArray' ignored
### Contains
when the key exists

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164123194
--- !!data
containsValue: A
```

receives:

```yaml
--- !!data
reply: true
```

when it doesnt exist

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164123195
--- !!data
containsValue: Z
```

receives:

```yaml
--- !!data
reply: false
```

### Replace 2
example of replace where the value is known

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164125428
--- !!data
replace: {
  key: 1,
  value: Z
}
```

receives:

```yaml
--- !!data
reply: A
```

### Put If Absent 2
replace(notPresent, "A", null will throw a NullPointerException

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164129903
--- !!data
putIfAbsent: {
  key: 1,
  value: Z
}
```

receives:

```yaml
--- !!data
reply: A
```

### Replace
example of replace where the value is not known

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164138968
--- !!data
replace: {
  key: 6,
  value: Z
}
```

receives:

```yaml
--- !!data
reply: !!null ""
```

### Get _ Null Pointer Exception
get(null) returns a NullPointerException

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164143432
--- !!data
get: !!null ""
```

receives:

```yaml
--- !!data
exception: !java.lang.NullPointerException {
  message: !!null "",
  stackTrace: [
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: nullCheck, file: MapWireHandler.java, line: 401 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1, method: lambda$accept$10, file: MapWireHandler.java, line: 280 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1$$Lambda$65/763926653, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: lambda$writeData$13, file: MapWireHandler.java, line: 425 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$$Lambda$66/1753875368, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.wire.Wires, method: writeData, file: Wires.java, line: 78 },
    { class: net.openhft.chronicle.wire.WireOut, method: writeDocument, file: WireOut.java, line: 68 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: writeData, file: MapWireHandler.java, line: 421 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1, method: accept, file: MapWireHandler.java, line: 201 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler$1, method: accept, file: MapWireHandler.java, line: 173 },
    { class: net.openhft.chronicle.engine.map.MapWireHandler, method: process, file: MapWireHandler.java, line: 78 },
    { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler, method: lambda$process$105, file: EngineWireHandler.java, line: 159 },
    { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler$$Lambda$27/116815206, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.wire.Wires, method: lambda$readData$38, file: Wires.java, line: 120 },
    { class: net.openhft.chronicle.wire.Wires$$Lambda$58/630778964, method: accept, file: !!null "", line: -1 },
    { class: net.openhft.chronicle.bytes.StreamingCommon, method: withLength, file: StreamingCommon.java, line: 52 },
    { class: net.openhft.chronicle.wire.Wires, method: readData, file: Wires.java, line: 120 },
    { class: net.openhft.chronicle.wire.WireIn, method: readDocument, file: WireIn.java, line: 75 },
    { class: net.openhft.chronicle.engine.server.internal.EngineWireHandler, method: process, file: EngineWireHandler.java, line: 153 },
    { class: net.openhft.chronicle.network.WireTcpHandler, method: read, file: WireTcpHandler.java, line: 100 },
    { class: net.openhft.chronicle.network.WireTcpHandler, method: process, file: WireTcpHandler.java, line: 61 },
    { class: net.openhft.chronicle.network.TcpEventHandler, method: invokeHandler, file: TcpEventHandler.java, line: 91 },
    { class: net.openhft.chronicle.network.TcpEventHandler, method: runOnce, file: TcpEventHandler.java, line: 79 },
    { class: net.openhft.chronicle.network.event.VanillaEventLoop, method: runAllHighHandlers, file: VanillaEventLoop.java, line: 125 },
    { class: net.openhft.chronicle.network.event.VanillaEventLoop, method: run, file: VanillaEventLoop.java, line: 95 }
  ]
}
```

### Entry Set To Array
map.entrySet().toArray() first gets the entry set and then converts it to an array

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164147911
--- !!data
entrySet: {
}
```

receives:

```yaml
--- !!data
reply: !set-proxy {
  csp: //test?view=entrySet,
  cid: 1
}
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164147912
--- !!data
numberOfSegments: {
}
```

receives:

```yaml
--- !!data
reply: 1
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164147914
--- !!data
iterator: 0
```

receives:

```yaml
--- !!data
reply: [
    {
    key: 4,
    value: D
},
    {
    key: 5,
    value: E
},
    {
    key: 2,
    value: B
},
    {
    key: 3,
    value: C
},
    {
    key: 1,
    value: A
}
]
```

### Replace Value
example of when then value was not replaced

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164154617
--- !!data
replaceForOld: {
  key: 1,
  oldValue: Z,
  newValue: Z
}
```

receives:

```yaml
--- !!data
reply: false
```

### Key Set
example of checking the size of a keyset

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164156854
--- !!data
keySet: {
}
```

receives:

```yaml
--- !!data
reply: !set-proxy {
  csp: //test?view=keySet,
  cid: 1
}
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164156855
--- !!data
size: {
}
```

receives:

```yaml
--- !!data
reply: 5
```

### Put All
sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164159098
--- !!data
entrySet: {
}
```

receives:

```yaml
--- !!data
reply: !set-proxy {
  csp: //test?view=entrySet,
  cid: 1
}
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164159099
--- !!data
numberOfSegments: {
}
```

receives:

```yaml
--- !!data
reply: 1
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164159100
--- !!data
iterator: 1
```

receives:

```yaml
--- !!data
reply: [
    {
    key: 4,
    value: D
},
    {
    key: 5,
    value: E
},
    {
    key: 2,
    value: B
},
    {
    key: 3,
    value: C
},
    {
    key: 1,
    value: A
}
]
```

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164159095
--- !!data
putAll: [
    {
    key: 4,
    value: D
},
    {
    key: 2,
    value: B
},
    {
    key: 3,
    value: C
},
    {
    key: 5,
    value: E
},
    {
    key: 1,
    value: A
}
]
```
 
receives:

```yaml
--- !!data
reply: {
}
```

### Is Empty
example of isEmpty() returning true, not it uses the size() method

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164163340
--- !!data
size: {
}
```

receives:

```yaml
--- !!data
reply: 0
```

### Remove
sends:

```yaml
--- !!meta-data
csp: //test?view=Map
--- !!data
remove: 5
```
### Values
example of getting the values and then calling size()

sends:

```yaml
--- !!meta-data
csp: //test?view=Map
tid: 1433164169816
--- !!data
values: {
}
```

receives:

```yaml
--- !!data
reply: !set-proxy {
  csp: //test?view=values,
  cid: 1
}
```

sends:

```yaml
--- !!meta-data
cid: 1
tid: 1433164169818
--- !!data
size: {
}
```

receives:

```yaml
--- !!data
reply: 5
```

