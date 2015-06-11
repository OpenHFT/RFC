# Specification

|         |                                                                                         |
|:------- | --------------------------------------------------------------------------------------- |
| Title   | Size Prefixed Blob format                                                               |
| URL  | https://github.com/OpenHFT/RFC/blob/master/Stop-Bit-Encoding/Stop-Bit-Encoding-0.1.md |
| Editor  | Peter Lawrey                                                                            |
| License | Apache 2.0                                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration                         |
| Status  | Raw.                                                                                    |

# Goals
Stop Bit Encoding compressed integer values with a minimum length.  It supports one byte per 7 bits, and short negative numbers with one extra byte.

## Positive numbers
Positive numbers can be encoded with one byte for each 7 bits.  The lowest bits are first, with the top bit 1 if there is more bytes to be read and a top bit of 0 if this is the last bytes.

```
0x00 - 0x7F => 0b0xxx_xxxx
0x80 - 0x3FFF => 0xb1xxx_xxxx 0xb0xxx_xxxx
0x4000 - 0x1FFFFFF =>  0xb1xxx_xxxx 0xb1xxx_xxxx 0xb0xxx_xxxx
etc
```

## Negative numbers
Negative numbers are the one's complement of the negative value as a positive value followed by a nul byte.  To decode this value, read the number as normal, but if the length is more than one and the last byte was 0 take the ~x instead of x.

```
-0x01 - -0x80 => 0b1xxx_xxxx 0x0000_0000
-0x81 - -0x4000 => 0xb1xxx_xxxx 0xb1xxx_xxxx  0x0000_0000
-0x4001 - -0x200000 =>  0xb1xxx_xxxx 0xb1xxx_xxxx 0xb1xxx_xxxx  0x0000_0000
etc
```

## Using a prefixed stop bit encoded length
The length of the data is prefixed before the data.

e.g. "key" can be written as
```
0x03 - length
0x6B 0x65 0x79 - "key"
```

