# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Remote access for Chronicle Map RFC                                     |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Map/Remote/Remote-Chronicle-Map-0.1.md |
| Editor  | Rob Austin                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
An RFC for library specific requirements for Remote access to a Chronicle Map. See the parent RFC for cross library requirements.

## Remote access.
This section details the services a Chronicle Map exposes and wire protocol for the methods.

### Services
| Service             | Relative URI                  |
| ----------------- | ----------------------------- |
| Unqualified Map  | map-name                      |
| Qualified Map     | map-name?view=Map                  |
| Keys of Map       | map-name?view=keySet               |
| Entries of Map    | map-name?view=entrySet             |
| Values of Map     | map-name?view=values               |
| Replication        | map-name?view=replication          |
| Publisher          | map-name/{key}?view=Publisher&elementType={keyType} |
| TopicPublisher   | map-name?view=TopicPublisher&keyType={keyType}&valueType |
| Subscription for a Map | map-name?view=Subscription        |
| Subscription for an Entry or Topic | map-name/{key}?view=Subscription |

These proxies can be returned by methods and should be serialized when sending to the server

### Method calls
In all the examples below the `csp:` can be replaced with a `cid:` which is a short token to improve the efficiency of the routing logic.

### Async Methods

All the methods are Synchronous, accept for put(<key>,<value) and remove(<key>,<value>) which for performance reasons are asynchronous.

### Examples

The following examples below are identical, showing the same examples in the same order, the only difference is tthe encoding format.
 
| Encoding Format  | URL                                                                                      |
| ---------------- | ---------------------------------------------------------------------------------------- |
| Text Wire        | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Map/Remote/Text-Wire-Examples-0.1.md    |
| Binary Wire      | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Map/Remote/Binary-Wire-Examples-0.1.md  |


### System Messages

If the first message that is sent is a —!data, this is deemed to be a system information message, part of the hand shaking

Also any message sent after a zero size --- !!meta-data is a system message.


in this example below we first send the user name

sends:

```yaml
--- !!data
userid: robaustin
```

followed by the rest of the data


sends:

```yaml
--- !!data
userid: robaustin
```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=Integer&valueType=String
--- !!data
put: {
  key: 1,
  value: hello
}
```
sends:
