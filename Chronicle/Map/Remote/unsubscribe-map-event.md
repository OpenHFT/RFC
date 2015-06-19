### Un Subscribe Stop Events Being Published [0]
subscribes to changes to the map

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
tid: 1434721140869
--- !!data
subscribe: !type net.openhft.chronicle.engine.api.map.MapEvent

```
unsubscribes to changes to the map

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=java.lang.String
tid: 1434721140869
--- !!data
unSubscribe: ""
```

receives:

```yaml
--- !!meta-data
tid: 1434721140869
```



receives:

```yaml
--- !!data
reply: !!null ""
```


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
tid: 1434721142127
--- !!data
size: {
}
```

receives:

```yaml
--- !!meta-data
tid: 1434721142127
```
