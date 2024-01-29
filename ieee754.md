# IEEE 754 - Or how fractional numbers are stored in a PC

If _binary_ solves us the problem of storing _natural_ numbers, as many as _memory_ allows us to, how do we solve the problem of floating point numbers, or of fractions?
The [IEEE](https://www.ieee.org) defined a standard for this named [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754), which has been updated a couple of times after its foundtion, but the bulk remians unchanged:


Let's say we have the number 19.59375 in decimal (aka, base 10). How do we store it?


1. We begine by computing the _significand (or mantisa)_, wich is the natural part of the number (i.e 19) in binary. We already know how to do this. We repedetly divide by two:

     ```
      19 / 2 = 9 | 1
      9 / 2 = 4  | 1
      4 / 2 = 2  | 0  
      2 / 2 = 1  | 0
      1 / 2 = 0  | 1   
   ```


We write the _binary_ result top to bottom: `11001`, so we have the $2^0 + 2^1 + 2^4 = 1 + 2 + 16 = 19$.


For the floating poin part, we go down in terms of powers of two. So instead of considering our bits $2^0$,$2^1$,$2^2$...
We acually count them as $2^{-1}$, $2^{-2}$, $2^{-3}$, etc.

  ```
      0.59375 * 2 = 1.18750 | 1
      0.18750 * 2 = 0.375   | 0
      0.375   * 2 = 0.75    | 0  
      0.75    * 2 = 1.5     | 1  
      0.5     * 2 = 1.0     | 1
   ```

When reaching a fractional part of `0`, we stop, so he the fractional value is `10011`

The whole number then is 

`1 1 0 0 1 . 1 0 0 1 1`

 However, we want to bring this to [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation), wich means only one digit should come after the period:

 `1 . 1 0 0 1  1 0 0 1 1`

 This look good, but it's not our original number. To keep track of the period place, and not to modify our number we can write the original number as an end result of the diseried number, multiplied with, 
 in our case 2 (since we are in base 2) at the power of how many digits we moved our period over, so:
 $$1 1 0 0 1 . 1 0 0 1 1 = 1 . 1 0 0 1  1 0 0 1 1 * 2^4 $$

 And we have our representation. `32bit` floating point numbers use:
 - 1 bit for the sign (if our deisred number were negtive, the bit sign is 1, else 0, both ways we can always make the further math as if the number is positive)
 - 8 bits for the mantisa, 4, in our case (because of the $2^4$), however there is an offset of 127(see [here](#off))
 - remaining 23 bits for the fractional part

   
 ## <a name="off"></a> The offset

We usually think of bit patterns as represeting powers of two, starting with `0`. However `IEEE 754` also wants the _mantisa_ to represent negative values.
Thefore, but patterns actually start with $2^-5$. Usually, when we represent signed integers, in say `C` a system named [Two's Complement](https://en.wikipedia.org/wiki/Two%27s_complement) is used.
This splits the available range of values into _two equal_ parts. Half negative, half pozitive.
IEEE 754 sotres the number in the mantisa as an unsigned integer, however the acual stored number is the initial number + 127 (in case of single precision floats).
This is done in order to store both negative and positive numbers, without using two's complement.
