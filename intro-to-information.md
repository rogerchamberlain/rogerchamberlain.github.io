---
layout: default
title: Introduction to Information Representation
author: Ben Stolovitz
---
{% include nav.html %}

# {{ page.title }}

![========](images/line.gif)

Computers are very different from humans<sup>[citation needed]</sup>. We've learned from experience that getting them to solve problems for us requires an extreme amount of luck and translation, and popular culture likes to attribute that to computers "thinking differently" from us. While that's certainly true, a lot of problems come from how we *store* information differently.

This guide explains how machines represent things, from the ground up, which is something we can't quite explain for human information. First, we talk about the electrical signals we use for binary.

![========](images/line.gif)

## Binary

At their very cores, modern (digital) machines store **bits** of information in the form of an electrical signal: on or off, `HIGH` or `LOW`, `1` or `0`. This sort of **boolean** information---information that can only be one of two values---is useful for questions like "yes or no?", "shaken or stirred?", "Pawnee or Eagleton?", but not much else. Thankfully, we can use binary as a **number system**, just like decimal.

In order to *really* understand that, consider these two questions:

- What is the number 4?
- What is the number 42?

In the first case, the illuminating answer is very similar to the obvious one. `4` is a symbol we know to illustrate the concept of four items, just like the word `four` means the same thing.

In the second case, however, the interesting answer is a little more silly than the obvious one. `42` is two symbols: a `4` followed by a `2`. In this context, `4` *actually* means "four tens." It is in the tens place. `2` still means "two," or, "two ones," since it is in the ones place.

You may recall an equation like this from grade school:

$$42 = 4 \times 10 + 2 \times 1$$

There's a tacit understanding that each consecutive digit in a number, from right to left, represents the next "place": first the 1's place, then the 10's place, 100's, 1,000's, etc.

But where does this come from? You probably know offhand that we count in "base 10," and therein lies our answer. "Base 10" implies that we have 10 counting symbols: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, and `9`. After that, our numerical system is, by itself, incapable of representing larger numbers. Hence this **positional notation**, where each consecutive number represents a different "place."

### Positional Notation

Unlike Roman numerals or even tally marks, which both stipulate simple addition of consecutive numbers (e.g. \$XVII = X + V + I + I = 10 + 5 + 1 + 1 = 17\$) , each consecutive number (from right to left) represents a multiple of a certain "place," determined by the base you are counting in.

For example, in base 10, each "place" is an increasing power of 10: the number `452` represents two ones ($10^0$) plus five tens ($10^1$) plus four hundreds ($10^2$), because we have 10 symbols and cannot represent anything larger than 9 with just one number.

That same series of digits in base 6, then (to pick a random base), would be two ones ($6^0$), five *sixes* ($6^1$), and four *thirty-sixes* ($6^2$). This is because base 6 has *six* symbols, not 10, and *runs out of symbols* after 5. Because we are using positional notation, we therefore advance to the next place, which represents the next consecutive number.

Then apply this to binary---base 2. The number `1001` in base 2 represents the number we know of as `9` in base 10:

$$ 1001_2 = 1 \times 2^3 + 0 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 = 9$$

This should lead you to a couple interesting conclusions:

- There must be `n` symbols for any base `n`. Base 2 has 2 symbols.
- Every place is exactly one greater than the maximum value of all the previous places combined: $$ 100_{10} $$ is one greater than $$ 99_{10} $$, and $1000_2$ is one greater than $111_2$.
- Shifting a number by adding or removing a place ($1,523 \leftrightarrow 15,230$) multiplies or divides it by the current base.
- It is tedious to count in base 2.

If any of these observations do not make sense, I suggest spending more time thinking about different bases and how counting works.

![========](images/line.gif)

## Hexadecimal

Computer scientists have long known the tediousness of base 2. Simple numbers become very long: it takes 10 places to represent all the numbers less than $1000_{10}$! So most of the time when dealing with computer-numbers we write in **hexadecimal**---hex. 

Hexadecimal is base 16. Base 16 requires 16 symbols, so we take all the base 10 digits, 0-9, and add `A=10`, `B=11`, `C=12`, `D=13`, `E=14`, and `F=15`. Since the base is 16, each place is a power of 16: $$ 100_{16} = 256_{10} $$. Therefore, the number $$ 1B_{16} = 27_{10} $$.

We typically prefix hex numbers with `0x` to identify them, like `0x4a41`. You can convert any binary to hex by taking each block of 4 binary digits and finding the corresponding hex digit, starting from the right:

$$1 1010 1110 1001 1001 = 0x1AE99$$

![========](images/line.gif)

## Ints and the things you're used to

Now that we've dispensed with the more abstract mathematics, reality comes into play. Consider why we use *binary* in computing: we only have two possible signals, `HIGH` and `LOW`. It is not physically possible to use any higher base.

We also run into another problem. How can we indicate when a number *ends*? In the human world, this is not a problem: we simply stop writing, make a new line, and write another number. However, this secretly introduces another symbol---some way of demarcating the end of a number. We do not have this extra symbol, therefore, we assign limits to the size of our numbers.

This is the reason we have `int`s. `int`s store whole numbers in base 2 but have a maximum size. Java is more exacting: `int`s are 4 bytes (a **byte** is a 8 **bits**, digits of binary: $4 \times 8 = 32$ bits), whereas C `int`s are *at least* 2 bytes ($2 \times 8 = 16$ bits). If you want 4 bytes in C, you use a `long`.

![========](images/line.gif)

## Signed, unsigned, and fixed point

We have two other non-digit symbols that we need to represent with our two symbols: the **negative sign** and the **decimal point**.

### Negativity

#### Unsigned numbers

The simplest solution is to ignore negative numbers entirely. Don't store anything negative. That is what `unsigned` numbers do, in both Java and C: an `unsigned int` in Java is exactly what you would expect, and can represent all numbers from 0 to $2^{32}-1 = 4,294,967,295$.

This will not do, because sometimes we need to work with negative numbers. Several "quick fixes" come to mind: **biased representation**, where where you subtract an implied number `K` from any negative number, so an unsigned `0` becomes `-K`. This number `K` is usually half the maximum value, so that a number can be **equally biased**, with equal negative and positive numbers. 


#### Sign-magnitude 

**Sign-magnitude** uses the highest bit as a negative "flag"— `HIGH` (or `1`) and the number is negative, `LOW` (or `0`) and the number is positive.

<br>However, neither  biased representation nor sign-magnitude is used in most circumstances.

#### Two's complement

Instead, all modern computers use **two's complement** to represent signed numbers. If a number is `signed` rather than `unsigned` (by default, almost every number is `signed` in Java and C), the **most significant bit**---the binary digit in the largest place (the `1` in `1,000,000`)---represents a *negative version of itself*. For example, in traditional, unsigned binary:

$$ 110_2 = 1 \times 2^{2} + 1 \times 2^{1} + 0 \times 2^0 = 6 $$

In *two's complement*:

$$ 110_2 = 1 \times -2^{2} + 1 \times 2^{1} + 0 \times 2^0 = -2 $$

![========](images/line.gif)

## Text & ASCII

If you have actually gotten here, I suggest going to Chapter 8 of the class
[textbook]({{ "/files/cc_v0_06.pdf#page=75" | relative_url }}) for more.

{% include footer.html %}
