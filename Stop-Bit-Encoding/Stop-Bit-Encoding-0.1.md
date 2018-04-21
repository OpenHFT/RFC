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
Stop Bit Encoding compressed integer values with a minimum length.  
It supports one byte per 7 bits, and short negative numbers with one extra byte.

This RFC support encoding for signed integers OR 64-bit floating point.  They cannot be used interchangeably.

# Signed Integers
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

# 64-bit floating point.
Like integers, a [64-bit floating point](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) can be encoded using a byte per 7 bit.

Instead of encoding from the lower bits up, a double is encoded from the top bits down.

## Top bytes
The top bit of the floating point has the sign and the top of the exponent.

```
0b0seeeeee - sign plus top 6 bits of the exponent.
0b1seeeeee 0eeeeemm - sign,11 exponent bits, and top 2 bits of the mantissa.
0b1seeeeee 1eeeeemm 0mmmmmmm - sign, 11 exponent bits, and top 9 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm 0mmmmmmm - sign,11 exponent bits, and top 16 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm 1mmmmmmm 0mmmmmmm - sign, 11 exponent, and top 23 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm x3 0mmmmmmm - sign, 11 exponent, and top 30 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm x4 0mmmmmmm - sign, 11 exponent, and top 37 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm x5 0mmmmmmm - sign, 11 exponent, and top 44 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm x6 0mmmmmmm - sign, 11 exponent, and top 51 bits of the mantissa.
0b1seeeeee 1eeeeemm 1mmmmmmm x7 0mmmmmmm - sign, 11 exponent, and top 52-58 bits of the mantissa.
```
All the trailing bits are assumed to be 0s.

### Examples

| double value  | Stop Bit encoded in hexidecimal     |
|---------------:|:---------------------------------------|
|         -0.0      | 40                                              |
|         -1.0      | DF 7C                                         |
|  -12345678    | E0 D9 F1 C2 4E                            |
|          0.0       | 00                                             |
|          1.0      | 9F 7C                                          |
|        1024      | A0 24                                         |
|    1000000     | A0 CB D0 48                               |
|          0.1       | 9F EE B3 99 CC E6 B3 99  4D        |
| Double.NaN    | BF 7E                                         |

