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

```C#
Subscriber<string> subscriber = obj => { Console.WriteLine(obj); }; // prints all messages
registerSubscriber<string>("group-A/name-1?bootstrap=true", subscriber);
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

```C#
var map = acquireMap<string, string>("group-A");
map.Add("Key-1", "Value-1");
map.Add("Key-1", "Value-2");
map.Remove("Key-1");
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

```C#
TopicSubscriber<string, string> subscriber = (topic, message) => { 
	Console.WriteLine("name: "+ topic + ", message: "+message);
};
registerTopicSubscriber<string, string>("group-A", subscriber);
```

This registers to all inserted, updated and removed events with the *current* values.  The current value on a remove is a *null*.

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```

```C#
var map = acquireMap<string, string>("group-A");
map.Add("Key-1", "Value-1");
map.Add("Key-1", "Value-2");
map.Remove("Key-1");
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

```C#
Subscriber<MapEvent<string, string>> subscriber = obj => { 
	Console.WriteLine(obj.ToString());
};
registerSubscriber<MapEvent>("group-A", subscriber);
```

This registers to all inserted, updated and removed events with previous values.

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```

```C#
var map = acquireMap<string, string>("group-A");
map.Add("Key-1", "Value-1");
map.Add("Key-1", "Value-2");
map.Remove("Key-1");
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

```C#
Subscriber<string> subscriber = obj => { 
	Console.WriteLine(obj.ToString());
};
registerSubscriber<string>("group-A", subscriber);
```

### Example
```java
Map<String, String> map = acquireMap("group-A", String.class, String.class);
map.put("Key-1", "Value-1");
map.put("Key-1", "Value-2");
map.remove("Key-1");
```

```C#
var map = acquireMap<string, string>("group-A");
map.Add("Key-1", "Value-1");
map.Add("Key-1", "Value-2");
map.Remove("Key-1");
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

```C#
var current = map["Key-1"];
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

```C#
var map = acquireMap<string, string>("group");
var publisher = acquirePublisher<string>("group/topic");
Subscriber<String> subscriber = obj => { ConsoleWriteLine(obj); };
registerPublisher<string>("group/topic", subscriber);

// subscriber will print
// Message-1
publisher.publish("Message-1");

Assert.AreEqual("Message-1", map["topic"]);

publisher.publish("Message-2");

Assert.AreEqual("Message-2", map["topic"]);
```

## Publish to any topic in a group
You can publish to any topic in a group. In this example, the *TopicPublisher* is bound to the *group*.

```java
Map<String, String> map = acquireMap("group", String.class, String.class);
TopicPublisher<String, String> publisher = acquireTopicPublisher("group", String.class, String.class);
List<String> values = new ArrayList<>();
TopicSubscriber<String, String> subscriber = (topic, message) -> values.add("{name: " + topic + ", message: " + message + "}");
registerTopicSubscriber("group", String.class, String.class, subscriber);

List<String> values2 = new ArrayList<>();
TopicSubscriber<String, String> subscriber2 = (topic, message) -> values2.add("{name: " + topic + ", message: " + message + "}");
publisher.registerSubscriber(subscriber2);

publisher.publish("topic-1", "Message-1");
assertEquals("Message-1", map.get("topic-1"));

publisher.publish("topic-1", "Message-2");
assertEquals("Message-2", map.get("topic-1"));
assertEquals("[{name: topic-1, message: Message-1}, {name: topic-1, message: Message-2}]", values.toString());
assertEquals("[{name: topic-1, message: Message-1}, {name: topic-1, message: Message-2}]", values2.toString());
```

```C#
var map = acquireMap<string, string>("group");
var publisher = acquireTopicPublisher<string, string>("group");
var values = new List<string>();
TopicSubscriber<string, string> subscriber = (topic, message) => {values.add("{name: " + topic + ", message: " + message + "}"); };
registerTopicSubscriber<string, string>("group", subscriber);

var values2 = new List<string>();
TopicSubscriber<string, string> subscriber2 = (topic, message) => { values2.add("{name: " + topic + ", message: " + message + "}"); };
publisher.registerSubscriber(subscriber2);

publisher.publish("topic-1", "Message-1");
Assert.AreEqual("Message-1", map["topic-1"]);

publisher.publish("topic-1", "Message-2");
Assert.AreEqual("Message-2", map["topic-1"]);
Assert.AreEqual("[{name: topic-1, message: Message-1}, {name: topic-1, message: Message-2}]", values.ToString());
Assert.AreEqual("[{name: topic-1, message: Message-1}, {name: topic-1, message: Message-2}]", values2.ToString());
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

```C#
var map = acquireMap<string, string>("group");
TopicSubscriber<string, string> subscriber = (topic, message) => {Console.WriteLine("name: "+ topic + ", message: "+message); };
registerTopicSubscriber<string, string>("group", subscriber);

// subscriber will print
// topic: topic-1, message: Message-1
map.Add("topic-1", "Message-1");

Assert.AreEqual("Message-1", map.get("topic-1"));

// subscriber will print
// topic: topic-1, message: null
map.Remove("topic-1");

string value;
Assert.IsFalse(map.TryGet("topic-1", out value));
```

## Reference to a topic.
You can acquire a *reference* to a topic and perform a *set*, *remove* or *get* on that reference.

```java
Map<String, String> map = acquireMap("group", String.class, String.class);
Reference<String> reference = acquireReference("group/topic", String.class);

List<String> values = new ArrayList<>();
Subscriber<String> subscriber = values::add;
registerSubscriber("group/topic", String.class, subscriber);

List<String> values2 = new ArrayList<>();
Subscriber<String> subscriber2 = values2::add;
reference.registerSubscriber(subscriber2);

reference.set("Message-1");
assertEquals("Message-1", reference.get());

assertEquals("Message-1", map.get("topic"));

reference.publish("Message-2");
assertEquals("Message-2", reference.get());
assertEquals("Message-2", map.get("topic"));
assertEquals("[Message-1, Message-2]", values.toString());
assertEquals("[Message-1, Message-2]", values2.toString());
```

```C#
var map = acquireMap<string, string>("group");
var reference = acquireReference<string>("group/topic");

var values = new List<string>();
Subscriber<string> subscriber = obj => { values.Add(obj); };
registerSubscriber<string>("group/topic", subscriber);

var values2 = new List<string>();
Subscriber<String> subscriber2 = obj => { values2.Add(obj); };
reference.registerSubscriber(subscriber2);

reference.set("Message-1");
Assert.AreEqual("Message-1", reference.get());

Assert.AreEqual("Message-1", map["topic"]);

reference.publish("Message-2");
Assert.AreEqual("Message-2", reference.get());
Assert.AreEqual("Message-2", map["topic"]);
Assert.AreEqual("[Message-1, Message-2]", values.ToString());
Assert.AreEqual("[Message-1, Message-2]", values2.ToString());
```


# References
[URI Wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_identifier)
