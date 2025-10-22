 # Low Level Stuff

* [Introduction](#intro)
* [Xor](#xor)
* [Endianness(#endianness)]

<h2 id="intro">Introduction</h2>

Think of a sigle _bit_ as a _container_ that can store/represent one of two values at a time: either `0` or `1`. That's it!

The convention then is to call a series of _8 bits_ taken together a  _byte_, but notice, that's just a convetion, though used very often, there is no logical reason to no have, for example a 9 bits be a byte, just a thing to keep in mind. From this simple definition, all sorts of mathematical properties arise.

For example,  every time you _increase_ the number of bits you have, you _double_ the previous number of representations (of unique combinations). 

So starting from one bit, that can have one of two values (`0` or `1`) you always double the numer of possible values your bits ca store/represent as you increase the number of bits you have at your disposal.
This is why Bbits represent powers of 2, if one bit stores $2^1$ values, two bits can store $2^2$ values and so on. The total number of values that can be stored by `n` bits is  $2^n$.


To represent this more visually, suppose you have one bit, it could be 0 or 1. So, two possible arrangements:

```
- one bit possibilites: 0      1
```
 Now suppose you add another bit, every branch of our previous posibilities gets two otther branches:
  
```
   - one bit possibilites:      0      1   - two values
   - two bits possibilites:   0  1    0  1 - four values
```
and so one:
```
   - one bit possibilites:            0                 1   - two values
   - two bits possibilites:      0         1       0         1 - four values
   - three bits possibilites:  0    1    0  1    0   1    0   1 - eight values
  // to obtain a unique representation you just traverse each level, starting top, and choose one direction/value per level
```

Every time we consider a new _bit_, we don't just _add_ two additional possible representations, rather we _double_ the previous number of possible representations, starting with a number of two possible representations, offered by _one bit_ that can either be `0` or `1`, then doubling that, from two possible representations to four. So then possible arrangements are `00`, `01`, or `10`, `11`.


This efective doubling means of each previous value with a new set of zero and one means matemathically that we raise to the power of two:

| Power of Two| Value         |
| ------------ | --- |
| 2<sup>0</sup>|1 |
| 2<sup>1</sup>|2 |
| 2<sup>2</sup>|4 |
| 2<sup>3</sup>|8 |
| 2<sup>4</sup>|16 |
| 2<sup>5</sup>|32 |
| 2<sup>6</sup>|64 |
| 2<sup>7</sup>|128 |



Notice, if you want to represent exactly one of those numbers in an *8 bit*, **byte**, you just set that bit ot 1. For any other number, it's a combination of bits.


If you're interested in understanding how does the magic of storing \verb|0|s or \verb|1|s happens at hardware level, a simple imagination exercise that may satisfy your curiosity is _electric charge_ . Imagine being able to _both_ measure the _charge_ of an _electronic component_ and charge/discharge it. If the component is charged, by convention, we treat that as being a `1`, and if it's discharged, we consider it `0`.

Having enough such minuscule components and combining them together allows us to store information, leaving it to us to assign meaning to each unique combination of zeros and ones.

### What is the max number you can store?

If you set all bits to 1, you'll get `1 + 2 + 4 + ... 128`. What's that equal to?

Well, imagine you have one additional bit  $2^8$ (256), and you want to represent that number, 256.

So all you do is set the last bit, 2<sup>8</sup> to one and you're done.

Now, if you substract one from 256 you get 255, and the binary representation of that is the 256 bit set to 0 and all bits beneath it set to one.

So for any number of _n_ bits, the greatest value that can be stored is $2^{(n+1)}-1$

But notice, that is the greatest number we can store, however the _range_ starts from `0`, not from `1` (zero is also a value), and theferore we can store $2^n$ _distinct_ numbers in `n` bits, ranging from 0 up to $2^{(n+1)-1}$


### Divide by two?

Since every bit is yet another power of two, if we shift to right (move all bits one position to right), we decrease by one power of two. One move one power.
This we efectiviley divide by two.


### Odd or even?

Since all powers of two are even, except 0, if the first bit is set to 1, we have na odd number, else it's even.


### How many bits do we need to store a number?

How many bits do you need to represent an arbitrary natural number _n_?

Well, if $2^m$ is _greater_ than your _n_, then `m` is your answer. So you keep raising `2` to increasing powers ( $2^0$, $2^1$, $2^2$... $2^m$ ) until you reach a _number_ (_m_) greater than your desired _n_.

However, there's a math _operation_ called `log`, wich is the _opposite_ of raising to a power. If $2^4=16$, think of $\log{_2}{16}$ (read as log base 2 of 16) as being the question: What power should  $2$ (the base) be raise at, so that it yieds $16$? The  answer is $4$,  $\log{_2}{16}=4$, because it 4 is the power of two that yields 16, $2^4=16$

So the answer to the initial problem is $\log{_2}{n}$, also having the answer rouded-up( for example if the result is $5.3$ we needs $6$ bits).


<h2 id="xor">XOR</h2>

XOR, also known as **exclusive OR** is just like **OR**, execpt it's only true if operands are distinct, hecne the exclusivity.


This is the **OR** truth table:

| Operand 1| Operand 2 | Is it true?|
| ------------ | --- |------|
| 0|0 |0 |
| 0|1 |1 |
| 1|0 |1 |
| 1|1 |1 |


This is the **XOR** truth table:

| Operand 1| Operand 2 | Is it true?|
| ------------ | --- |------|
| 0|0 |0 |
| 0|1 |1 |
| 1|0 |1 |
| 1|1 |0 |

 It's usually denoted in programing lanauges by the operator `^`. Xor has some interesting properties usefull in enctpytion:
 
 ```swift
let a = 15
let b = 8
let c = 10

let d = a ^ b // 7, different number
// if we xor it back with any of the two (a or b) we get the other one back:
d ^ a == b // true
d ^ b == a // true
 ```




<h2 id="endianness">The Big & Little Endian</h2>

Within a _single_ byte, we always read the bits right to left, so the least significant bit is the rightmost:

|0|0|0|0|0|0|1|0|
|-|-|-|-|-|-|-|-|
|2^7|2^6|2^5|2^4|2^3|2^2|2^1|2^0|

However, when you have a series of octets (bytes), say the (hexadecimal) number `0x000A` (formed of bytes `00` and `0A`) in what order do you read those bytes?
Well that depends on the way the CPU was designed (some where designed to support both ways; by commuting between them) and there are advantages to both ways of doing things.
`x86_64` (Intel & AMD) are both _little endian_ (meaning they first read the right most `0A` byte) while traditionally, RISC arhitectures (Like IBM _Power-PC_) are _big endian_:

Here's a small Swift programm that proves the point:

```Swift
let number: Uint16 = 0x0100 // force cast to Uint16 to prevent additional filling with bytes that would have been caused by a 32bit value
print(number.littleEndian) // 265 as first 8 positions are 0  [0 0 0 0] <- first hexa 0 (a hexa letter is 4 bits)  [0 0 0 0] <- second hexa 0; ergo the first bit to be 1 is 2^8
print(number.bigEndian) // 1 as first octet is hexa `01` so 0 0 0 0 0 0 0 1 
```
