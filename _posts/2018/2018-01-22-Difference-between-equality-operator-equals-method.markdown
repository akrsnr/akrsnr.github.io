---
layout: post
title:  "Difference Between Equality Operator Equals Method"
date:   2018-01-22 21:34:34 +0300
categories: java
author: Soner Akar
---

You have used String class of Java hundred times, however, you feel yourself to know adequately about it. But, early to decide!

As you know, if not now time to learn, equality checking operator `==` is used to compare references, whereas `equals()` method is used to compare whether or not strings are filled up by the same contents.

Let's see your level, what is the output of the following?

```java
String str1 = "Soner";
String str2 = "Soner";
System.out.println(str1 == str2);
```

I seem to hear that it will result in totally false! Cray-cray adorbs of course true! Let's examine why. In the case of variables `str1` and `str2`, the objects are created and stored in a **pool of String objects**. Before creating a new object in the pool, Java searches for an object with similar contents. When the following line of code executes, no String object with the value
`"Soner"` is found in the pool of String objects:

```java
String str1 = "Soner";
```

As a result, Java creates a String object with the value `"Soner"` in the pool of String
objects referred to by the variable str1.

```java
/* Creates a new String object with value
    Morning in the String constant pool */
System.out.println("Morning");
```

These values are reused from the String constant pool if a matching value is found. If
a matching value isn’t found, the JVM creates a String object with the specified value
and places it in the String constant pool:

```java
String morning1 = "Morning";
System.out.println("Morning" == morning1);
```

Compare the preceding example with the following example, which creates a String
object using the operator new and (only) double quotes and then compares their
references:

```java
/* This String object is not placed
	in the String constant pool.
	So it will give false output */
String morning2 = new String("Morning");
System.out.println("Morning" == morning2);
```

<blockquote><p>
<span style="color:blue">Note:</span> The terms <b>String constant pool</b> and <b>String pool</b> are used interchangeably and refer to the same pool of String objects.
</p></blockquote>

Whether test your comprehension, take a glance at the following code to count total number of String objects being created.

```java
class OMGString {
	public static void main(String... args) {
		String soner = new String("Soner"); 	// 1
		String soner2 = "Soner";				// 2
		System.out.println("Soner");			// 3
		System.out.println("akar");				// 4
		System.out.println("akar" == "soner");	// 5
		String akar = new String("Soner");		// 6
		}
}
```

<details>
  <summary>
➡ How many String objects being created are there?
<i>Click here to see the answer.</i> ↴
 </summary>

* The code at ① creates a new String object with the value `"Soner"`. This object
is not placed in the String constant pool.
* The code at ② creates a new String object with the value `"Soner"` and places
it in the String constant pool.
* The code at ③ doesn’t need to create any new String object. It reuses the
String object with the value `"Soner"` that already existed in the String con-
stant pool.
* The code at ④ creates a new String object with the value `"akar"` and places
it in the String constant pool.
* The code at ⑤ reuses the String value `"akar"` from the String constant
pool. It creates a String object with the value "soner" in the String con-
stant pool (note the difference in the case of letters—Java is case-sensitive and
`"Soner"` is not the same as `"soner"` ).
* The code at ⑥ creates a new String object with the value `"Soner"`.


<blockquote><p>
<span style="color:blue">Answer:</span>
<i>The previous code creates a total of five String objects.</i>
</p></blockquote>

</details>


That's all. I hope that it wouldn't be waste of time.
