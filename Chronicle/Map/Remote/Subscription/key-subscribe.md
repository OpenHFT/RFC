### Subscribe To Key Events [0]
Sets up a subscription to listen to key events. And subsequently puts and entry into the map, notice that the Key is subsequently received from the server

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
tid: 1434723217160
--- !!data
subscribe: !type String

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
tid: 1434723217160
```



receives:

```yaml
--- !!not-ready-data!
reply: Hello
```
