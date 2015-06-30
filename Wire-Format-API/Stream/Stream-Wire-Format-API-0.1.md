# Specification

|         |                                                                             |
|:------- | --------------------------------------------------------------------------- |
| Title   | A description of how to layout a file for an arbitrary reader                    |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/                 |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Stream           |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Stream/Stream-Wire-Format-API-0.1.md |
| Editor  | Peter Lawrey                                                                |
| License | Apache 2.0                                                                  |
| Change Process | Users issue Pull Requests for the Editor's consideration.            |
| Status  | Raw.                                                                        |

# Goals
This describes how to lay out a file so it can read by future versions.

This is not a new wire format, rather how it can be used for compatibility.

# Stream layout
The start of the file forms a deserializable object.  This object can then be used to read the remainder of the file.

# Continuation
After reading the header, the object deserialized will read the rest of the file.  This data could be in a different wire format.

### TEXT Format
```yaml
Æ\0\0@header: !FileFormat {
  version: 100,
  createdTime: 2015-06-30T10:05:11.336+01:00[Europe/London],
  creator: peter,
  identity: 80bd251a-e2d9-43c3-bbef-de5e901ab3d9,
  wireType: !WireType BINARY
}
```

### BINARY Format
```
00000000 9D 00 00 40 C6 68 65 61  64 65 72 B6 0A 46 69 6C ···@·hea der··Fil
00000010 65 46 6F 72 6D 61 74 82  85 00 00 00 C7 76 65 72 eFormat· ·····ver
00000020 73 69 6F 6E 64 CB 63 72  65 61 74 65 64 54 69 6D siond·cr eatedTim
00000030 65 B5 2C 32 30 31 35 2D  30 36 2D 33 30 54 31 30 e·,2015- 06-30T10
00000040 3A 30 35 3A 31 31 2E 33  35 37 2B 30 31 3A 30 30 :05:11.3 57+01:00
00000050 5B 45 75 72 6F 70 65 2F  4C 6F 6E 64 6F 6E 5D C7 [Europe/ London]·
00000060 63 72 65 61 74 6F 72 E5  70 65 74 65 72 C8 69 64 creator· peter·id
00000070 65 6E 74 69 74 79 A0 21  44 79 E7 9B C9 B8 6E 28 entity·! Dy····n(
00000080 9A 77 A0 FA 84 39 BE C8  77 69 72 65 54 79 70 65 ·w···9·· wireType
00000090 B6 08 57 69 72 65 54 79  70 65 E6 42 49 4E 41 52 ··WireTy pe·BINAR
000000a0 59                                               Y                
```

### FIELDLESS_BINARY Format
```
00000000 68 00 00 40 B6 0A 46 69  6C 65 46 6F 72 6D 61 74 h··@··Fi leFormat
00000010 82 57 00 00 00 64 B5 2C  32 30 31 35 2D 30 36 2D ·W···d·, 2015-06-
00000020 33 30 54 31 30 3A 30 35  3A 31 31 2E 33 35 38 2B 30T10:05 :11.358+
00000030 30 31 3A 30 30 5B 45 75  72 6F 70 65 2F 4C 6F 6E 01:00[Eu rope/Lon
00000040 64 6F 6E 5D E5 70 65 74  65 72 A0 A9 42 0E 24 11 don]·pet er··B·$·
00000050 EF 80 9A 37 B6 C7 64 56  78 B2 88 B6 08 57 69 72 ···7··dV x····Wir
00000060 65 54 79 70 65 E6 42 49  4E 41 52 59             eType·BI NARY    
```

### RAW Format
```
00000000 64 00 00 40 0A 46 69 6C  65 46 6F 72 6D 61 74 06 d··@·Fil eFormat·
00000010 68 65 61 64 65 72 4E 00  00 00 64 00 00 00 2C 32 headerN· ··d···,2
00000020 30 31 35 2D 30 36 2D 33  30 54 31 30 3A 30 35 3A 015-06-3 0T10:05:
00000030 31 31 2E 33 36 30 2B 30  31 3A 30 30 5B 45 75 72 11.360+0 1:00[Eur
00000040 6F 70 65 2F 4C 6F 6E 64  6F 6E 5D 05 70 65 74 65 ope/Lond on]·pete
00000050 72 F9 48 71 6B 17 29 88  1C AE 55 27 BD 8F 08 4B r·Hqk·)· ··U'···K
00000060 83 06 42 49 4E 41 52 59                          ··BINARY         
```
