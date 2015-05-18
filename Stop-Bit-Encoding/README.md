# Specification

|         |                                                                                         |
|:------- | --------------------------------------------------------------------------------------- |
| Title   | Size Prefixed Blob format                                                               |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Stop-Bit-Encoding/                          |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Stop-Bit-Encoding/Stop-Bit-Encoding-0.1.md |
| Editor  | Peter Lawrey                                                                            |
| License | Apache 2.0                                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration                         |
| Status  | Raw.                                                                                    |

# Goals
Stop Bit Encoding compressed integer values with a minimum length.  It supports one byte per 7 bits, and short negative numbers with one extra byte.


