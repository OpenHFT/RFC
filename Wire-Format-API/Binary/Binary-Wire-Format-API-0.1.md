# Goals

A compact encoding that both desicrbes the the length of the fields and the type of the fields.

See example below

# First byte

For short fields, the first byte denotes the type of the field, for small numbers the value is encoded in the later 4 bits. For strings and fields the later 4 bits denote the length of the preceeding data. For large numerics or string/fields the special type is used see section below :

| comment                               | 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
| top three bits denotes a three Type   | x   | x   | x   |  x  |     |     |     |    |
| remaining bits dontes the size/value  |     |     |     |     | x   | x  |  x   | x  |


## Types ( for small values )

| type (\<value or length in bytes\>)   | 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0  |
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

| Control message                       | 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
|                                       | 1   | 0   | 0   | 0   |     |     |     |    | 
| (0x82) - Nested Block 32bit len 82    | 1   | 0   | 0   | 0   | 0   | 0   | 1   |  0 | 
| (0x8A) - byte[]                       | 1   | 0   | 0   | 0   | 1   | 0   | 1   |  0 | 
| (0x8D) - long[]                       | 1   | 0   | 0   | 0   | 1   | 1   | 0   |  1 | 
| (0x8E) - paddding with 32bit length   | 1   | 0   | 0   | 0   | 1   | 1   | 1   |  0 | 
| (0x8E) - single padded byte           | 1   | 0   | 0   | 0   | 1   | 1   | 1   |  1 | 


## Decimal

the types use the Control Messages

| Decimal message                       | 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0  |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | ---|
|                                       | 1   | 0   | 0   | 1   |     |     |     |    | 
| (0x90) - 32bit floating point         | 1   | 0   | 0   | 1   | 0   | 0   | 0   |  0 | 
| (0x91) - 64bit floating point         | 1   | 0   | 0   | 1   | 0   | 0   | 0   |  1 | 

