# RFC Template

|         |                                                             |
|:------- | ----------------------------------------------------------- |
| Title   | Remote connectivity for Chronicle Queue                     |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Queue  |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Queue/Remote/Remote-Chronicle-Queue-0.1.md |
| Editor  | Peter Lawrey                                                |
| License | Apache 2.0                                                  |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status  | Raw.                                                        |

# Goals
An RFC for platform specific requirements for Remote access to a Chronicle Queue.

## Related RFCs
The file format uses;

 - [Size Prefixed Blob](https://github.com/OpenHFT/RFC/blob/master/Size-Prefixed-Blob/) for the overall file structure
 - [Wire Format API](https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/) for individual messages and meta data

# Meta data wire messages.
All messages added for the implementation of the queue will be marked as "meta-data". All user provided messages will be marked as "data".
A reader which needs to scan all the user messages can do so by skipping all the meta data messages.

## Services
| Service              | Relative URI                  |
| -------------------- | ----------------------------- |
| Unqualified Queue    | queue-name                    |
| Qualified Queue      | queue-name#Queue              |
| Appender for a queue | queue-name#appender           |
| Tailer for a queue   | queue-name#tailer             |

### Queue Appender
If the client sends
```yaml
--- !!meta-data
csp://server/path/queue-name#Queue
# or
cid: 1212
tid: 123456789
--- !!data
createAppender: { }
```

The server replies with a proxy
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: !<fully_qualified_path>.queue-appender { csp://server/path/map-name#appender, cid: 1234 }
```

get the last index of the appender

```yaml
cid: 1234
tid: 123456789
--- !!data
lastWrittenIndex: { }  // gets the last index of the appender not the chronicle
```


#### Appending the next
If the client sends
```yaml
--- !!meta-data
csp://server/path/queue-name#appender
# or
cid: 1212
tid: 123456789
--- !!data
submit: !net.openhft.MyObject{ field1: one, field2: two }
```

The server replies with the index of the entry.
```yaml
--- !!meta-data
tid: 123456789
--- !!data
index: 1286348638468324
```
### Queue Tailer
If the client sends
```yaml
--- !!meta-data
csp://server/path/queue-name#Queue
# or
cid: 1212
tid: 123456789
--- !!data
createTailer: { }
```

The server replies with a proxy
```yaml
--- !!meta-data
tid: 123456789
--- !!data
reply: !<fully_qualified_path>queue-tailer { csp://server/path/map-name#tailer, cid: 12345, start: 1286348000000000, end: 1286348638469999 }
```
Note: a unique tailer is required for each 

#### getting the next
If the client sends
```yaml
--- !!meta-data
csp://server/path/queue-name#tailer/12345
# or
cid: 1212
tid: 123456789
--- !!data
hasNext: { index: 1286348638468324, batch: 4 }
```

The server replies with a batch
```yaml
--- !!meta-data
tid: 123456789
--- !!data
index: 1286348638468324 # the actual index read.
reply: 
  - { field1: one, field2: two }
  - !!binary 981273kwajduy81e2hkjhsd=
  - { field1: three, field2: four }
  - !!binary jsagdiy981273kajduy81e32hkjhsd==
```

Once the client has consumed these messages it can call `hasNext:` again.

## References
[RFC Home](https://github.com/OpenHFT/RFC/blob/master/)

[RFC Naming](https://github.com/OpenHFT/RFC/blob/master/RFC-Naming/)
