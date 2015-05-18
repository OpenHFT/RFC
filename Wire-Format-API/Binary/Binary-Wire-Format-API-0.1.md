# Goals

A compact encoding that both desicrbes the the length of the fields and the type of the fields.

See example below

# Examples

these examples are based on bits

| comment                          | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| -------------------------------- | -- | -- | -- | -- | -- | -- | -- | -- |
| top three bits denotes  a string | 1 | 1 | 1 |   |   |   |   |   |


| comment           | Relative URI                  |
| ----------------- | ----------------------------- |
|   top three bits denotes  a string  | map-name                      |
|      | map-name#Map                  |
|        | map-name#keySet               |
|     | map-name#entrySet             |
|       | map-name#values               |
|      | map-name#replication          |
