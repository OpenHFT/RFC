## Publish / Subscription

### Publish to a topic
Publish is asynchronous by default. In future there might be a `syncPublish`

sends asynchronously:

```yaml
--- !!meta-data
csp: //group/topic-1?view=Publisher
--- !!data
publish: 42
```

sends asynchronously:

```yaml
--- !!meta-data
csp: //group/topic-1?view=Publisher
--- !!data
publish: !mypackage.MyClass { field1: value1, field2: 2 }
```

### Publish to any topic

sends asynchronously:

```yaml
--- !!meta-data
csp: //group?view=TopicPublisher
--- !!data
publish: { topic: topic-1, message: 42 }
```

sends asynchronously:

```yaml
--- !!meta-data
csp: //group?view=TopicPublisher
--- !!data
publish: { topic: topic-1, message: !mypackage.MyClass { field1: value1, field2: 2 } }
```

### Subscribe to a group of topics
You need to subscribe with a `tid` and each message will have the same `tid`.

sends:

```yaml
--- !!meta-data
csp: //group?view=Subscription
tid: 1433164169818
--- !!data
registerTopicSubscriber: { keyType: String, valueType: String }
```

receives a bootstrap message for the same `tid`.

```yaml
--- !!meta-data
tid: 1433164169818
--- !!data-not-ready
reply: { topic: topic-1, message: 42 }
```

and a later message with the same `tid`.

```yaml
--- !!meta-data
tid: 1433164169818
--- !!data-not-ready
reply: { topic: topic-1, message: 42 plus more }
```

finally the server ends the subscription, the last update 

```yaml
--- !!meta-data
tid: 1433164169818
--- !!data
reply: { topic: topic-1, message: 42 and no more }
```

Note: the `!!data-not-ready` means this `tid` has not finished and there can be more data.  The `!!data` tag means this is the last piece of data and you can cleanup after this.

### Unregister from a group of topics.
You need to unsubscribe with the same `tid` you subscribed with

sends:
```yaml
--- !!meta-data
csp: //group?view=Subscription
tid: 1433164169818
--- !!data
unregisterTopicSubscriber: { }
```

Note: you may get more data after this unregister, however you should soon get a message with a tag of `!!data` to confirm the subscription has concluded.


### Subscribe to a specific topic
You need to subscribe with a `tid` and each message will have the same `tid`.

sends:

```yaml
--- !!meta-data
csp: //group/topic-1?view=Subscription
tid: 1433164169821
--- !!data
registerSubscriber: { type: String }
```

receives a bootstrap message for the same `tid`.

```yaml
--- !!meta-data
tid: 1433164169821
--- !!data-not-ready
reply: 42
```

and a later message with the same `tid`.

```yaml
--- !!meta-data
tid: 1433164169821
--- !!data-not-ready
reply: 42 plus more
```

finally the server ends the subscription, the last update 

```yaml
--- !!meta-data
tid: 1433164169821
--- !!data
reply: 42 and no more
```

Note: the `!!data-not-ready` means this `tid` has not finished and there can be more data.  The `!!data` tag means this is the last piece of data and you can cleanup after this.

### Unregister from a specific topic.
You need to unsubscribe with the same `tid` you subscribed with

sends:
```yaml
--- !!meta-data
csp: //group?view=Subscription
tid: 1433164169818
--- !!data
unregisterSubscriber: { }
```

Note: you may get more data after this unregister, however you should soon get a message with a tag of `!!data` to confirm the subscription has concluded.

