# Goals

A compact encoding that both encodes the length of the fields and the type of the fields.

# Encoding Byte

For short fields, the first byte denotes the type of the field, for small numbers the value is encoded in the later 4 bits. For strings and fields the later 4 bits denote the length of the preceeding data. For large numerics or string/fields the special type is used see section below :

| comment                               | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| top three bits denotes a three Type   | x   | x   | x   |  x  |     |     |     |    |
| remaining bits denotes the size/value |     |     |     |     | x   | x  |  x   | x  |


## Types ( for small values )

| type   | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| Num (0-15)                            | 0   | 0   | 0   | 0   |     |     |     |    | 
| Num (16-31)                           | 0   | 0   | 0   | 1   |     |     |     |    | 
| Num (32-47)                           | 0   | 0   | 1   | 0   |     |     |     |    | 
| Num (48-63)                           | 0   | 0   | 1   | 1   |     |     |     |    | 
| Num (64-79)                           | 0   | 1   | 0   | 0   |     |     |     |    | 
| Num (80-95)                           | 0   | 1   | 0   | 1   |     |     |     |    | 
| Num (96-111)                          | 0   | 1   | 1   | 0   |     |     |     |    | 
| Num (112-127)                         | 0   | 1   | 1   | 1   |     |     |     |    | 
| Control Message                       | 1   | 0   | 0   | 0   |     |     |     |    | 
| Decimal                               | 1   | 0   | 0   | 1   |     |     |     |    | 
| Integer                               | 1   | 0   | 1   | 0   |     |     |     |    | 
| Special                               | 1   | 0   | 1   | 1   |     |     |     |    | 
| Field (0-15)                          | 1   | 1   | 0   | 0   |     |     |     |    |
| Field (16-31)                         | 1   | 1   | 0   | 1   |     |     |     |    |
| String (0-15)                         | 1   | 1   | 1   | 0   |     |     |     |    |
| String (0-31)                         | 1   | 1   | 1   | 1   |     |     |     |    | 

## Types ( for larger values )

the types use the Control Messages

| Control message                       | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits are the same )                   | 1   | 0   | 0   | 0   |     |     |     |    | 
| (0x82) - Nested Block 32bit len 82    | 1   | 0   | 0   | 0   | 0   | 0   | 1   |  0 | 
| (0x8A) - byte[]                       | 1   | 0   | 0   | 0   | 1   | 0   | 1   |  0 | 
| (0x8D) - long[]                       | 1   | 0   | 0   | 0   | 1   | 1   | 0   |  1 | 
| (0x8E) - paddding with 32bit length   | 1   | 0   | 0   | 0   | 1   | 1   | 1   |  0 | 
| (0x8E) - single padded byte           | 1   | 0   | 0   | 0   | 1   | 1   | 1   |  1 | 


## Decimal

| Decimal message                       | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits are the same )                    | 1   | 0   | 0   | 1   |     |     |     |    | 
| (0x90) - 32bit floating point         | 1   | 0   | 0   | 1   | 0   | 0   | 0   |  0 | 
| (0x91) - 64bit floating point         | 1   | 0   | 0   | 1   | 0   | 0   | 0   |  1 | 

## Integer

| Integer message                      | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits are the same )                    | 1   | 0   | 1   | 0   |     |     |     |    | 
| (0xA0) - 128bit uuid                  | 1   | 0   | 1   | 0   | 0   | 0   | 0   |  0 | 
| (0xA1) - unsigned 8bit int            | 1   | 0   | 1   | 0   | 0   | 0   | 0   |  1 |
| (0xA2) - unsigned 16bit int           | 1   | 0   | 1   | 0   | 0   | 0   | 1   |  0 |
| (0xA3) - unsigned 32bit int           | 1   | 0   | 1   | 0   | 0   | 0   | 1   |  1 |
| (0xA4) - signed 8bit int              | 1   | 0   | 1   | 0   | 0   | 1   | 0   |  0 |
| (0xA5) - signed 16bit int             | 1   | 0   | 1   | 0   | 0   | 1   | 0   |  1 |
| (0xA6) - signed 32bit int             | 1   | 0   | 1   | 0   | 0   | 1   | 1   |  0 |
| (0xA7) - signed 64bit int             | 1   | 0   | 1   | 0   | 0   | 1   | 1   |  1 |

## Special

Note for\<string\> the string encode by default is a stop bit encoded len folloed by a ISO-8851-9 string, see more on this at https://github.com/OpenHFT/RFC/blob/master/Stop-Bit-Encoding/

| Special message                      | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits are the same )                  | 1   | 0   | 1   | 1   |     |     |     |    | 
| (0xB0) - FALSE                        | 1   | 0   | 1   | 1   | 0   | 0   | 0   |  0 | 
| (0xB1) - TRUE                         | 1   | 0   | 1   | 1   | 0   | 0   | 0   |  1 |
| (0xB2) - time UTC (long)              | 1   | 0   | 1   | 1   | 0   | 0   | 1   |  0 |
| (0xB3) - Date (joda UTF8-Str)      | 1   | 0   | 1   | 1   | 0   | 0   | 1   |  1 |
| (0xB4) - DateTime (joda UTF8-Str)  | 1   | 0   | 1   | 1   | 0   | 1   | 0   |  0 |
| (0xB5) - ZonedDateTime (joda  \<string\>) | 1   | 0   | 1   | 1   | 0   | 1   | 0   |  1 |
| (0xB6) - type ( \<type\> +  \<string\>)  | 1   | 0   | 1   | 1   | 0   | 1   | 1   |  0 |
| (0xB7) - field (\<field\> +  \<string\>)  | 1   | 0   | 1   | 1   | 0   | 1   | 1   |  1 |
| (0xB8) - string (\<string\> +   \<string\>) | 1   | 0   | 1   | 1   | 1   | 0   | 0   |  1 |
| (0xB9) - eventname (\<eventname\> +  \<string\>)        | 1   | 0   | 1   | 1   | 1   | 0   | 1   |  0 |
| (0xBA) - fieldNumber (\<fieldNumber\> + stopbit encoded) | 1   | 0   | 1   | 1   | 1   | 0   | 1   |  1 |
| (0xBB) - NULL              | 1   | 0   | 1   | 1   | 1   | 1   | 0   |  0 |



# Example

using the encodings abouve, the following YAML 
``` yaml
--- !!meta-data
csp: //path/service
tid: 123456789
--- !!data
put: {
  key: key-1,
  value: value-1
}
``` 

sent as binary wire would, would encode to :
```
00000000 1C 00 00 40 C3 63 73 70  EE 2F 2F 70 61 74 68 2F ···@·csp ·//path/
00000010 73 65 72 76 69 63 65 C3  74 69 64 A3 15 CD 5B 07 service· tid···[·
00000020 21 00 00 00 C3 70 75 74  82 18 00 00 00 C3 6B 65 !····put ······ke
00000030 79 E5 6B 65 79 2D 31 C5  76 61 6C 75 65 E7 76 61 y·key-1· value·va
00000040 6C 75 65 2D 31                                   lue-1       
```

this is the java code that created this data 

``` java
@Test
public void testSequence() {
    Wire wire = createWire();
    writeMessage(wire);
    wire.flip();
    System.out.println(wire.bytes().toHexString());

    Wire twire = new TextWire(Bytes.elasticByteBuffer());
    writeMessage(twire);
    twire.flip();
    System.out.println(Wires.fromSizePrefixedBlobs(twire.bytes()));
}

private void writeMessage(Wire wire) {
    wire.writeDocument(true, w -> w
            .write(() -> "csp").text("//path/service")
            .write(() -> "tid").int64(123456789));
    wire.writeDocument(false, w -> w
            .write(() -> "put").marshallable(m -> m
                    .write(() -> "key").text("key-1")
                    .write(() -> "value").text("value-1")));
}
```
