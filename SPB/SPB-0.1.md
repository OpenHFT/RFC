# Size Prefixed Blob format

|      |                                                          |
|:---- | -------------------------------------------------------- |
| URL | https://github.com/OpenHFT/RFC/blob/master/SPB/SPB-0.1.md |
| Editor | Peter Lawrey |
| License | Apache 2.0 |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status | Raw. |

## Goals
SPB is designed to be a portable wire-framing format for opaque blobs of data carried over streaming protocols such as TCP/IP.

This format is intend to be used for streaming data such as TCP connections and streaming files.

## Format description
The file starts with a 64-bit value which can contain ASCII text and a version.  The value 0x0000000000000000 is defined to be invalid or unset.

After the header, a series of messages are prefixed by a 32-bit value which contains a 30-bit size and two bits used to make parsing the stream easier.

A length 32-bit word of 0x00000000 is defined as unset or invalid. To send a zero length message it must be a meta data message of 0 length.

## File Format bits
In file format;
 
 - the bit 31 indicates whether the current size is ready, 0 = ready, 1 = not ready.  
 - the bit 30 indicates the message contains meta data. 0 = user data, 1 = meta data,

### handling of incomplete messages

A message can start with two possible lengths as bits
 
 - 0b1M00 0000 0000 0000 0000 0000 0000 0000 - the length is not yet known.
 - 0b1MXX XXXX XXXX XXXX XXXX XXXX XXXX XXXX - the message is not complete, however the length is known.   

## TCP Connection Format bit
In a TCP socket connection;

 - the bit 31 indicates whether there is more data, 0 = no more data for this action, 1 = more data needs to be read.
 - the bit 30 indicates the message contains meta data. 0 = user data, 1 = meta data.

## Message length
The message length is a 30-bit unsigned length from 1 to 1006632960 bytes.  The following lengths have a special meaning.

 - 0x3C000000 to 0x3FFFFFFF - reserved
 
# ABNF description

```
stream              = header *blobs-with-length
header              = 8OCTET
blobs-with-length   = invalid | data | meta-data | not-ready-data | not-ready-meta-data | reserved
invalid             = 4%x00
data                = %x00 - %x3B 3OCTET message-body
meta-data           = %x40 - %x7B 3OCTET message-body
not-ready-data      = %x80 - %xBB 3OCTET message-body
not-ready-meta-data = %xC0 - %xFB 3OCTET message-body
message-body        = *OCTET
```

## References

[ABNF Wikipedia](http://en.wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_Form)
