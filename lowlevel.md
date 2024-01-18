# Low Level Stuff

* [Introduction](#intro)
* [Xor](#xor)


<h2 id="intro">Introduction</h2>

Bits represent powers of 2, because evey bit can store one of two values (0 or 1), the total number of values that can be stored by n bits is  2<sup>n</sup>.

Essentially, every time you add a bit, you double the previous number of posible arangements, or representations, by two.

Suppose you have one bit, it could be 0 or 1. So, two possible arangments:

```
0

1
```
 Now suppose you add another bit, every branch of our previous posibilities gets two otther branches:
  
```
  0
0 
  1
  
  0
1
 1
```
So possible arangements are 00, 01, or 10, 11.

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



So notice, if you want to represent exactly one of those numbers in an *8 bit*, **byte**, you just set that bit ot 1. For any other number, it's a combination of bits.

### What is the max number you can store?

If you set all bits to 1, you'll get `1 + 2 + 4 + ... 128`. What's that equal to?

Well, imagine you have one additional bit `2`<sup>`8`</sup> (256), and you want to represent that number, 256.

So all you do is set the last bit, 2<sup>8</sup> to one and you're done.

Now, if you substract one from 256 you get 255, and the binary representation of that is the 256 bit set to 0 and all bits beneath it set to one.

So for any number of _n_ bits, the greatest number that can be stored in _n_ bits is 2<sup>n+1</sup> -1.

So, if  `2`<sup>`n`</sup> is the total number of distinct values we can fit, they range from 0 up to `2`<sup>`n+1`</sup> `-1`


### Divide by two?

Since every bit is yet another power of two, if we shift to right (move all bits one position to right), we decrease by one power of two. One move one power.
This we efectiviley divide by two.


### Odd or even?

Since all powers of two are even, except 0, if the first bit is set to 1, we have na odd number, else it's even.


### How many bits do we need to store a number?

How many bits do you need to represent an arbitrary natural number _n_?

Well, if `2`<sup>`m`</sup> is _greater_ than your _n_, then `m` is your answer. So you keep raising `2` to differnet powers (`2`<sup>`1`</sup>, `2`<sup>`2`</sup>...) until you reach a _number_ greater than your initial _m_.

However, there's a math _operation_ called `log`, wich is the _opposite_ of raising to a power. If `2`<sup>`4`</sup>`=16`, think of `log`<sub>`2`</sub>`16` as being the question: What power should  `2` (the base) be raise at, so that it yieds `16`? The  answer is `4`, `log`<sub>`2`</sub>`16`=`4`.

So the answer to the initial problem is `log`<sub>2</sub>`n`,  and having round it up (if for example the result is `5.3` we needs `6` bits ).


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

