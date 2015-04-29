# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Remote access for Chronicle Engine                                      |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Engine             |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Engine/Subscription |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Engine/Subscription/Chronicle-Engine-Subscription-0.1.md |
| Editor  | Daniel Shaya                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
An RFC for library specific requirements for Subscriptions in
Chronicle Engine. See the parent RFC for cross library requirements.

## Subscribe
When you want to subscribe to real-time updates for a piece of data.

To subscribe to all events:
```yaml
--- !!meta-data
csp:///service-lookup
tid: 1426502826520
--- !!data
subscribeAll: { }
```

To subscribe to events on a MAP for a specific key
```yaml
--- !!meta-data
csp:///path/map-name#map
tid: 1426502826520
--- !!data
subscribe:
  - EURUSD
```

To subscribe to events on a MAP for more than one key
```yaml
--- !!meta-data
csp:///path/map-name#map
tid: 1426502826520
--- !!data
subscribe:
  - EURUSD
  - GBPEUR
```

## Unsubscribe
When you want to unsubscribe to real-time updates for a piece of data.

To subscribe to all events:
```yaml
--- !!meta-data
csp:///service-lookup
tid: 1426502826520
--- !!data
unsubscribeAll: { }
```

To unsubscribe to events on a MAP for a specific key
```yaml
--- !!meta-data
csp:///path/map-name#map
tid: 1426502826520
--- !!data
unsubscribe:
  - EURUSD
```

To unsubscribe to events on a MAP for more than one key
```yaml
--- !!meta-data
csp:///path/map-name#map
tid: 1426502826520
--- !!data
unsubscribe:
  - EURUSD
  - GBPEUR
```

## Java API

The underlying API in Java could lookup like
```java
public interface Subscription<K, C> {
    public void subscribeAll();
    public void subscribe(K... keys);
    public void unsubscribeAll();
    public void unsubscribe(K... keys);

    public void setCallback(C callback);
}

//Example callback
public interface MapEventListener<K,V>{
    public void update(K key, V oldValue, V newValue);
    default void insert(K key, V value){ update(key, null, value);}
    default void remove(K key, V value){ update(key, value, null);}
}
```

