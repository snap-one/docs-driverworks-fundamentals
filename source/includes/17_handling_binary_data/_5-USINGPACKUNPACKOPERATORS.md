## Using Pack and Unpack Format Operators

Pack/unpack supports the encoding and decoding of values from binary data. The pack/unpack functions take a format string to encode and decode binary data. The operators of the format string follow.

bin.lib Format String Operators

| Operator | Description |
| --- | --- |
| z | zero-terminated string |
| h | short |
| H | unsigned short |
| b | byte = unsigned char |
| p | string preceded by length byte |
| P | string preceded by length word |
| a | string preceded by length size t |
| A | string |
| f | float |
| d | double |
| n | Lua number |
| c | char |
| i | int |
| I | unsigned int |
| l | long |
| L | unsigned long |
| \< | little endian |
| \> | big endian |
| = | native endian |

Note that the endian operators work as modifiers to all the characters following them in the format string.
