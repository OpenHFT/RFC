# RFC Template

|         |                                                             |
|:------- | ----------------------------------------------------------- |
| Title   | General Template for RFCs                                   |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Chronicle-0.1.md |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Queue/Chronicle-Queue-0.1.md |
| Editor  | Peter Lawrey                                                |
| License | Apache 2.0                                                  |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status  | Raw.                                                        |

# Goals
An RFC for platform specific requirements for Chronicle Queue solutions.

## Description
A Chronicle Queue is an implementation of a persistent queue. This queue is only appended and entries are not removed.
This Queue can also be thought of as a Journal.

## Related RFCs
The file format uses;

 - [Size Prefixed Blob](https://github.com/OpenHFT/RFC/blob/master/Size-Prefixed-Blob/) for the overall file structure
 - [Wire Format API](https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/) for individual messages and meta data

# Meta data wire messages.
All messages added for the implementation of the queue will be marked as "meta-data". All user provided messages will be marked as "data".
A reader which needs to scan all the user messages can do so by skipping all the meta data messages.

## The header message
The first message will be the header.  This header has the following structure in Text Wire format.
```
header: {

}
```
## The index2index message
The second message will be the index2index message.
This message has a primary level lookup to support random access of entries in the queue.
This message contains the byte offset index of "index" message in the queue.

## The index message
The index message contains the offset of every N messages.  N is likely to be a power of two. e.g. 64.

To perform a random lookup of a message,
 - look at the "header" to find the "index2index" message
 - look in the "index2index" message to find the appropriate "index" message.
 - look in the "index" to find the start of a sequence of messages e.g. every 64th message.
 - scan along the sizes of each message to find the actual message.

The "header" and "index2index" message should be in cache after performing a random access.
This means there is at least non cached 2 page accesses to find a random message.

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
...
--- !!data
createAppender: { }
...
```

The server replies with a proxy
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: !!queue-appender { csp://server/path/map-name#appender, cid: 1234 }
...
```

#### Appending the next
If the client sends
```yaml
--- !!meta-data
csp://server/path/queue-name#appender
# or
cid: 1212
tid: 123456789
...
--- !!data
submit: { field1: one, field2: two }
...
```

The server replies with the index of the entry.
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
index: 1286348638468324
...
```
### Queue Tailer
If the client sends
```yaml
--- !!meta-data
csp://server/path/queue-name#Queue
# or
cid: 1212
tid: 123456789
...
--- !!data
createTailer: { }
...
```

The server replies with a proxy
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
reply: !!queue-tailer { csp://server/path/map-name#tailer, cid: 12345, start: 1286348000000000, end: 1286348638469999 }
...
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
...
--- !!data
hasNext: { index: 1286348638468324, batch: 4 }
...
```

The server replies with a batch
```yaml
--- !!meta-data
tid: 123456789
...
--- !!data
index: 1286348638468324 # the actual index read.
reply: 
  - { field1: one, field2: two }
  - !!binary 981273kwajduy81e2hkjhsd=
  - { field1: three, field2: four }
  - !!binary jsagdiy981273kajduy81e32hkjhsd==
...
```

Once the client has consumed these messages it can call `hasNext:` again.

## References
[RFC Home](https://github.com/OpenHFT/RFC/blob/master/)

[RFC Naming](https://github.com/OpenHFT/RFC/blob/master/RFC-Naming/)
