### Subscribe To Changes To a Map
Sets up a subscription to listen to map events. And subsequently puts and entry into the map, notice that the InsertedEvent is received from the server

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
tid: 1434717981538
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

receives:

```yaml
--- !!meta-data
tid: 1434717981538
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
