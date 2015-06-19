### Subscribe To Map Events And Remove Key

Sets up a subscription to listen to key events. And subsequently puts and entry into the map followed by a remove, notice that the 'reply: Hello' is received twice, one for the put and one for the remove.
 
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
tid: 1434725375948
--- !!data
subscribe: !type net.openhft.chronicle.engine.api.map.MapEvent

```
puts an entry into the map so that an event will be triggered

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
--- !!data
put: {
  key: Hello,
  value: World
}
```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
--- !!data
remove: Hello
```

receives:

```yaml
--- !!meta-data
tid: 1434725375948
```



receives:

```yaml
--- !!not-ready-data!
reply: !InsertedEvent {
  assetName: /test,
  key: Hello,
  value: World
}
```



receives:

```yaml
--- !!meta-data
tid: 1434725375948
```



receives:

```yaml
--- !!not-ready-data!
reply: !RemovedEvent {
  assetName: /test,
  key: Hello,
  oldValue: World
}
```

