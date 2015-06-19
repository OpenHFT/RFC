### Subscribe To Key Events And Remove Key  
Sets up a subscription to listen to key events. And subsequently puts and entry into the map followed by a remove, notice that the 'reply: Hello' is received twice, one for the put and one for the remove.

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
tid: 1434724697542
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
tid: 1434724697542
```

receives:

```yaml
--- !!not-ready-data!
reply: Hello
```

receives:

```yaml
--- !!meta-data
tid: 1434724697542
```

receives:

```yaml
--- !!not-ready-data!
reply: Hello
```

 
