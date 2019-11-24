---
layout: post
title:  "The Custom Specifiers in C# "
date:   2019-11-23 21:34:34 +0300
categories: csharp
author: Soner Akar
---
# The Custom Specifiers

I had difficulty in grasping the format specifiers while studying. I couldn't find educational explanatory web pages on which I can grasp perspicuously then I made a decision to write some notes. I assume your [culture setting](https://rextester.com/HVGS41537) is _en-US_.

## The "0" custom specifier

### For the integer part,

If number of digits of the given number are greater than number of "0" placeholders what presented is displayed.

If being less, leading $0$ digits are appended.

### For the decimal part,

If number of digits of the given number are greater than number of "0" placeholders, the decimal part is rounded as much as total digits of the "0" placeholders.

If being less, trailing zero(s) are appended.

```
// Number is rounded if includes more decimal places then the custom specifier.
Console.WriteLine($"{786.128:000.00}"); // 786.13
Console.WriteLine($"{786.1:0000.000}"); // 0786.100
```


## The "#"(octothorp) custom specifier

### For the integer part,

If number of digits of the given number are _greater than_ number of "#" placeholders, what presented is displayed.

If being less, what presented is displayed. That is to say, there is no any leading digits.

### For the decimal part,

If number of digits of the given number are _greater than_ number of "#" placeholders, the decimal part is rounded as much as total digits of the "#" placeholders. (same as the case of "0" placeholder)

If being less, nothing is appended, that is to say what presented is displayed.

```
Console.WriteLine($"{2.7:0##.#}"); // 002.7
// Number is rounded if includes more decimal places then the custom specifier.
Console.WriteLine($"{786.36:0######.#}"); // 0000786.4 **
Console.WriteLine($"{786.348:######0.##}"); // 786.35
```

> **Note 1:** Rounding logic in the placeholder usage is done by the following rule.
If a digit following immediately the digit being rounded is greater or equal to 5, the digit is increased by 1.
If being less, it preserves its value.

> **Note 2:** "#" placeholder doesn't yield results to insignificant digits in the result string unlike "0" placeholder.
```
// the four zeros are insignificant
int value = 0000529;
// the trailing zeros(6 pieces) are insignificant, but leading two.
double value = 2.00324000000 
// the whole number is insignificant
double value = 0.0;
```

Let's glance at some examples related to the notes.


```
double value = 25.710;
// both following yields same result, 26.
// Remember Note 1 saying immediate following digit's value makes a basis for rounding.
// left 5's following digit is 7(at the right-side).
Console.WriteLine("{0:#.}", value);
Console.WriteLine("{0:0.}", value);
```

```
double value = 0.01;
double value2 = 0.07;

// same as {0:#}
Console.WriteLine("{0:#.}", value); // empty print due to integer part's insignificance
// same as {0:0}
Console.WriteLine("{0:0.}", value);

// .1 is printed due to integer part's insignificance and decimal part's rounding
Console.WriteLine("{0:#.#}", value2);
Console.WriteLine("{0:0.0}", value2);
```

## The comma(,) specifier

It used both to separate the number into thousand places and divide by 1000.
I don't like some explanations of Microsoft Documents, however, [the bullet explanations](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-numeric-format-strings?redirectedfrom=MSDN#the--custom-specifier-2) are not bad.

Read the bullets more than twice to comprehend the following example.
```
double value = 911_271_222_876_919.12345;
// double size is exceeded, so 911271222876919.1 is the output.
Console.WriteLine(value);
// so, the acutal number is 911_271_222_876_919.1
// result is 911,271.2229
Console.WriteLine("{0:#,0,,,.0000}", value);
```

Here's my two cents that first examine the integer part then the decimal part.

The number is divided by $1000^3$ since there are 3 commas at first as well as one more comma among two placeholders to get seperated output. It yields 911,271. But, we wish to get 4 more digits as a decimal part.
In this case, the following digits of 911,271 make up the decimal part with rounding. So, the digits are 2, 2, 2, and 8. But since the actual numbers' digits are greater than the four placeholders, "0000", the decimal part is rounded to 2229.

I'm not going on explaining the other specifiers i.e. [%](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-numeric-format-strings?redirectedfrom=MSDN#the--custom-specifier-3) from now on since once you grasp the explained logic, the others are as easy as falling off a log.

Additional examples can be found on [this link](https://web.archive.org/web/20191123153500/https://www.dotnetperls.com/format) and [this one](https://web.archive.org/web/20191123153526/https://www.codebuns.com/csharp/numeric-formats/).

If you find amiss point, please let me know  but to err  is  human,  you  know.  To  forgive  is  divine.

_Soner The Improver_