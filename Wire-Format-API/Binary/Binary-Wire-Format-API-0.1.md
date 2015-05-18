# Goals

A compact encoding that both desicrbes the the length of the fields and the type of the fields.

See example below

# Examples

these examples are based on bits

| comment                          | 7 | 6 | 5 | 4 | 3 | 2 | 1 |
| -------------------------------- | - | - | - | - | - | - | - |
| top three bits denotes  a string | 1 | 1 | 1 |   |   |  |    |


| Service           | Relative URI                  |
| ----------------- | ----------------------------- |
| Unqualified Map   | map-name                      |
| Qualified Map     | map-name#Map                  |
| Keys of Map       | map-name#keySet               |
| Entries of Map    | map-name#entrySet             |
| Values of Map     | map-name#values               |
| Replication       | map-name#replication          |
