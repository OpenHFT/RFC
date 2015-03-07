# Size Specified Blob format

URL: https://github.com/OpenHFT/RFC/blob/master/SPB/SPB-0.1.md
Editor: Peter Lawrey
License: Apache 2.0
Change Process: Users issue Pull Requests for the Editor's consideration.

## Goals
SPB is designed to be a portable wire-framing format for opaque blobs of data carried over streaming protocols such as TCP/IP.

This format is intend to be used for streaming data such as TCP connections and streaming files.

## Format description
The file starts with a 64-bit value which can contain ASCII text and a version.  The value 0x0000000000000000 is defined to be invalid or unset.

After the header, a series of messages are prefixed by a 32-bit value which contains a 30-bit size and two bits used to make parsing the stream easier.

### File Format bits
In file format;
 
 - the bit 31 indicates whether the current size is ready, 0 = ready, 1 = not ready.  
 - the bit 30 indicates the message contains meta data. 0 = user data, 1 = meta data, 

### TCP Connection Format bit
In a TCP socket connection;

 - the bit 31 indicates whether there is more data, 0 = no more data for this action, 1 = more data needs to be read.
 - the bit 30 indicates the message contains meta data. 0 = user data, 1 = meta data, 

### Message length
The message length is a 30-bit unsigned length in bytes.  The following lengths have a special meaning.

 - 0x00000000 - invalid message
 - 0x3FFFFFFE - length not know (only appropriate for File Format)
 - 0x3FFFFFFF - length not know (only appropriate for File Format)