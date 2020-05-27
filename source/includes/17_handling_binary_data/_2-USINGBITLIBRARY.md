## Using the Bit Library

The bit library is a C library for Lua that is included in DriverWorks to perform bitwise operations on bit strings. Library operations include:

- `bit.bnot(a)` - Returns the one's complement of a. 
- `bit.band(w1,...)` - Returns the bitwise and of the w variables. 
- `bit.bor(w1,...)` - Returns the bitwise or of the w variables. 
- `bit.bxor(w1,...)` - Returns the bitwise xor of the w variables. 
- `bit.lshift(a,b)` - Returns a shifted left b places—padded with zeros. 
- `bit.rshift(a,b)` - Returns a shifted logically right b places. 
- `bit.arshift(a,b)` - Returns a shifted arithmetically right b places. 
- `bit.mod(a,b)` - Returns the integer remainder of a divided by b. 

A simple example of using the bit.bor operation follows. It assumes the following binary data set:

8    4    2    1     (byte positions)\`
\`1    1    0    0          (12)
1    0    1    1           (11)

Input:

`foo = 12`

`bar = 11`

`print(bit.bor(foo, bar))`

Output:

`15`



Here is another example using the bit.rshift operator based on the same binary data set:

Input:

`foo = 12`

`print(bit.rshift(foo, 2))`

Output:

`3`