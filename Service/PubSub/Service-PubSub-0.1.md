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

# Subscription types.

## Subscription to a topic.
You can register to a message. In this example, the *subscriber* will print all messages after a bootstrap of the previous message if it exists. 

```java
Subscriber<String> subscriber = System.out::println; // prints all messages
registerSubscriber("group-A/name-1?bootstrap=true", String.class, subscriber);
```
This will give one update, if a message has been sent before.

This registers to a specific topic "name-1" in the "group-A".  It also optionally bootstraps the data with the last message sent.

Note: the group "group-A" must exist.

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
The above *subscriber* will produce the events
```
Value-1
Value-2
null
```

## Subscription to a group of topics.
When you register a TopicSubscriber, it will listen to all the events for a specific group of topics.

```java
TopicSubscriber<String, String> subscriber = (topic, message) -> System.out.println("name: "+ topic + ", message: "+message);
registerTopicSubscriber("group-A", String.class, String.class, subscriber);
```

This registers to all inserted, updated and removed events with the *current* values.  The current value on a remove is a *null*.

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
The above *subscriber* will produce the events
```
name: Key-1, message: Value-1
name: Key-1, message: Value-2
name: Key-1, message: null
```

## Subscription to changes in entries key-value store.
When you register a subscriber, it will listen to all the events for a specific topic. In this example, the subscriber prints the events 

```java
Subscriber<MapEvent<String, String>> subscriber = System.out::println; // prints all entries
registerSubscriber("group-A", MapEntry.class, (Subscriber) subscriber);
```

This registers to all inserted, updated and removed events with previous values.

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
The above *subscriber* will produce the events

```
InsertedEvent{ key: Key-1, value: Value-1 }
UpdatedEvent{ key: Key-1, oldValue: Value-1, value: Value-2 }
RemovedEvent{ key: Key-1, value: Value-2 }
```

## Subscription to topics changed
When you subscribe to the topic names, you just the changes to those topics.  This means you need to *get()* the latest value if needed.

```java
Subscriber<String> subscriber = System.out::println; // prints all entries
registerSubscriber("group-A", String.class, subscriber);
```

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```
The above *subscriber* will produce the events

```
Key-1
Key-1
Key-1
```
To get the latest value you can poll it with

```java
String current = map.get("Key-1");
```

# Publishing types

## Publish to a set topic
You can publish to a specific topic with a Publisher. In this example, the *Publisher* is bound to *topic* is "topic" in the *group* "group"

```java
Map<String, String> map = acquireMap("group", String.class, String.class);
Publisher<String> publisher = acquirePublisher("group/topic", String.class);
Subscriber<String> subscriber = System.out::println; 
registerPublisher("group/topic", String.class, subscriber);

// subscriber will print
// Message-1
publisher.publish("Message-1");

assertEquals("Message-1", map.get("topic"));

publisher.publish("Message-2");

assertEquals("Message-2", map.get("topic"));
```

## Publish to any topic in a group
You can publish to any topic in a group. In this example, the *TopicPublisher* is bound to the *group*.

```java
Map<String, String> map = acquireMap("group", String.class, String.class);
TopicPublisher<String, String> publisher = acquireTopicPublisher("group", String.class, String.class);
TopicSubscriber<String, String> subscriber = (topic, message) -> System.out.println("name: "+ topic + ", message: "+message);
registerTopicSubscriber("group", String.class, String.class, subscriber);

// subscriber will print
// topic: topic-1, message: Message-1
publisher.publish("topic-1", "Message-1");

assertEquals("Message-1", map.get("topic-1"));
```

## Update the map view
You can manipulate topics as a map.  In this example, the group is viewed as a *Map* and *put* performs the publish.

```java
Map<String, String> map = acquireMap("group", String.class, String.class);
TopicSubscriber<String, String> subscriber = (topic, message) -> System.out.println("name: "+ topic + ", message: "+message);
registerTopicSubscriber("group", String.class, String.class, subscriber);

// subscriber will print
// topic: topic-1, message: Message-1
map.put("topic-1", "Message-1");

assertEquals("Message-1", map.get("topic-1"));

// subscriber will print
// topic: topic-1, message: null
map.remove("topic-1");

assertEquals(null, map.get("topic-1"));
```


# References
[URI Wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_identifier)
