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
subscriber = System.out::println; // prints all messages
registerSubscriber("group-A/name-1?bootstrap=true", String.class, subscriber);
```
This will give one update, if a message has been sent before.

This registers to a specific topic "name-1" in the "group-A".  It also optionally bootstraps the data with the last message sent.

Note: the group "group-A" must exist.

## Subscription to a key-value store.
When you register a subscriber, it will listen to all the events for a specific topic.

```java
subscriber = System.out::println; // prints all entries
registerSubscriber("group-A?bootstrap=true", Entry.class, subscriber);
```

This registers to a specific topic "name-1" in the "group-A".  It also optionally bootstraps the data with the last message sent.

## Topic Subscription
# References
[URI Wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_identifier)
