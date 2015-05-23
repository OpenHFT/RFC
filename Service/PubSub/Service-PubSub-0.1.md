# Specification

|         |                                                                     |
|:------- | ------------------------------------------------------------------- |
| Title   | Service Messages RFC                                                |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Service/Messages/Service-PubSub-0.1.md |
| Editor  | Peter Lawrey                                                        |
| License | Apache 2.0                                                          |
| Change Process | Users issue Pull Requests for the Editor's consideration.    |
| Status  | Raw.                                                                |

# Goals
RFC on to publish and subscribe to named topics.

Publishing and Subscription can be to a fix name, or a collection of names.  This should be integrated with the Key Value Store. 

# Publish and Subscription types.

## Subscription to a topic.
You can register to a message

```java
Subscriber<String> subscriber = System.out::println; // prints all messages
registerSubscriber("group-A/name-1?bootstrap=true", String.class, subscriber);
```
This will give one update, if a message has been sent before.

This registers to a specific topic "name-1" in the "group-A".  It also optionally bootstraps the data with the last message sent.

Note: the group "group-A" must exist.

### Example
```java
Map<String, String> map = acquireMap("map-name", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
Will produce the events
```
Value-1
Value-2
null
```

## Subscription to multiple topics in a group.
When you register a TopicSubscriber, it will listen to all the events for a specific group of topics.

```java
TopicSubscriber<String, String> subscriber = (topic, message) -> System.out.println("name: "+ topic + ", message: "+message);
registerTopicSubscriber("group-A?bootstrap=true", String.class, subscriber);
```

This registers to all inserted, updated and removed events with the *current* values.  The current value on a remove is a *null*.

### Example
```java
Map<String, String> map = acquireMap("map-name", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
Will produce the events
```
name: Key-1, message: Value-1
name: Key-1, message: Value-2
name: Key-1, message: null
```

## Subscription to changes in entries key-value store.
When you register a subscriber, it will listen to all the events for a specific topic.

```java
Subscriber<MapEvent<String, String>> subscriber = System.out::println; // prints all entries
registerSubscriber("group-A?bootstrap=true", MapEntry.class, (Subscriber) subscriber);
```

This registers to all inserted, updated and removed events with previous values.

### Example
```java
Map<String, String> map = acquireMap("map-name", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
Will produce the events
```
InsertedEvent{ key: Key-1, value: Value-1 }
UpdatedEvent{ key: Key-1, oldValue: Value-1, value: Value-2 }
RemovedEvent{ key: Key-1, value: Value-2 }
```

## Subscription to topics changed
When you subscribe to the topic names, you just the changes to those topics.  This means you need to get() an updated value if needed.

```java
Subscriber<String> subscriber = System.out::println; // prints all entries
registerSubscriber("group-A?bootstrap=true", String.class, subscriber);
```

### Example
```java
Map<String, String> map = acquireMap("map-name", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
Will produce the events
```
Key-1
Key-1
Key-1
```
To get the latest value you can poll it with

```java
String current = map.get("Key-1");
```

## Topic Subscription
# References
[URI Wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_identifier)
