# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Remote access for Chronicle Map RFC                                     |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Map/Remote/Remote-Chronicle-Map-0.1.md |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
An RFC for library specific requirements for Remote access to a Chronicle Map. See the parent RFC for cross library requirements.

## Remote access.
This section details the services a Chronicle Map exposes and wire protocol for the methods.

### Services
| Service           | Relative URI                  |
| ----------------- | ----------------------------- |
| Unqualified Map   | map-name                      |
| Qualified Map     | map-name#Map                  |
| Keys of Map       | map-name#keySet               |
| Entries of Map    | map-name#entrySet             |
| Values of Map     | map-name#values               |
| Replication       | map-name#replication          |

These proxies can be returned by methods and should be serialized when sending to the server

### Method calls
In all the examples below the `csp:` can be replaced with a `cid:` which is a short token to improve the efficiency of the routing logic.

### Async Mehtods

All the methods are Synchronous, accept for put(<key>,<value) and remove(<key>,<value>) which for performance reasons are asynchronous.



