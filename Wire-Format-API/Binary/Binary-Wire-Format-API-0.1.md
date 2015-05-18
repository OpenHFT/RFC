# Goals

A compact encoding that both desicrbes the the length of the fields and the type of the fields.

See example below

# Encoding Byte

For short fields, the first byte denotes the type of the field, for small numbers the value is encoded in the later 4 bits. For strings and fields the later 4 bits denote the length of the preceeding data. For large numerics or string/fields the special type is used see section below :

| comment                               | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| top three bits denotes a three Type   | x   | x   | x   |  x  |     |     |     |    |
| remaining bits denotes the size/value |     |     |     |     | x   | x  |  x   | x  |


## Types ( for small values )

| type (\<value or length in bytes\>)   | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
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
| ( all higher bits )                   | 1   | 0   | 0   | 0   |     |     |     |    | 
| (0x82) - Nested Block 32bit len 82    | 1   | 0   | 0   | 0   | 0   | 0   | 1   |  0 | 
| (0x8A) - byte[]                       | 1   | 0   | 0   | 0   | 1   | 0   | 1   |  0 | 
| (0x8D) - long[]                       | 1   | 0   | 0   | 0   | 1   | 1   | 0   |  1 | 
| (0x8E) - paddding with 32bit length   | 1   | 0   | 0   | 0   | 1   | 1   | 1   |  0 | 
| (0x8E) - single padded byte           | 1   | 0   | 0   | 0   | 1   | 1   | 1   |  1 | 


## Decimal

| Decimal message                       | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits )                   | 1   | 0   | 0   | 1   |     |     |     |    | 
| (0x90) - 32bit floating point         | 1   | 0   | 0   | 1   | 0   | 0   | 0   |  0 | 
| (0x91) - 64bit floating point         | 1   | 0   | 0   | 1   | 0   | 0   | 0   |  1 | 

## Integer

| Integer message                      | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits )                   | 1   | 0   | 1   | 0   |     |     |     |    | 
| (0xA0) - 128bit uuid                  | 1   | 0   | 1   | 0   | 0   | 0   | 0   |  0 | 
| (0xA1) - unsigned 8bit int            | 1   | 0   | 1   | 0   | 0   | 0   | 0   |  1 |
| (0xA2) - unsigned 16bit int           | 1   | 0   | 1   | 0   | 0   | 0   | 1   |  0 |
| (0xA3) - unsigned 32bit int           | 1   | 0   | 1   | 0   | 0   | 0   | 1   |  1 |
| (0xA4) - signed 8bit int              | 1   | 0   | 1   | 0   | 0   | 1   | 0   |  0 |
| (0xA5) - signed 16bit int             | 1   | 0   | 1   | 0   | 0   | 1   | 0   |  1 |
| (0xA6) - signed 32bit int             | 1   | 0   | 1   | 0   | 0   | 1   | 1   |  0 |
| (0xA7) - signed 64bit int             | 1   | 0   | 1   | 0   | 0   | 1   | 1   |  1 |

## Special

| Special message                      | 7bit   | 6bit   | 5bit   | 4bit   | 3bit   | 2bit   | 1bit   | 0bit  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| ( all higher bits )                   | 1   | 0   | 1   | 1   |     |     |     |    | 
| (0xB0) - FALSE                        | 1   | 0   | 1   | 1   | 0   | 0   | 0   |  0 | 
| (0xB1) - TRUE                         | 1   | 0   | 1   | 1   | 0   | 0   | 0   |  1 |
| (0xB2) - time UTC (long)              | 1   | 0   | 1   | 1   | 0   | 0   | 1   |  0 |
| (0xB3) - Date (joda UTF8-Str)      | 1   | 0   | 1   | 1   | 0   | 0   | 1   |  1 |
| (0xB4) - DateTime (joda UTF8-Str)  | 1   | 0   | 1   | 1   | 0   | 1   | 0   |  0 |
| (0xB5) - ZonedDateTime (joda UTF8-Str) | 1   | 0   | 1   | 1   | 0   | 1   | 0   |  1 |
| (0xB6) - type ( \<type\> + UTF8-Str)  | 1   | 0   | 1   | 1   | 0   | 1   | 1   |  0 |
| (0xB7) - field (\<field\> + UTF8-Str)  | 1   | 0   | 1   | 1   | 0   | 1   | 1   |  1 |
| (0xB8) - string (\<string\> +  UTF8-Str) | 1   | 0   | 1   | 1   | 1   | 0   | 0   |  1 |
| (0xB9) - eventname (\<eventname\> +  UTF8-Str)        | 1   | 0   | 1   | 1   | 1   | 0   | 1   |  0 |
| (0xBA) - fieldNumber (\<fieldNumber\> + stopbit encoded) | 1   | 0   | 1   | 1   | 1   | 0   | 1   |  1 |
| (0xBB) - NULL              | 1   | 0   | 1   | 1   | 1   | 1   | 0   |  0 |

