---
title: "Efficient programming with Python"
date: 2020-05-05
tags: [python, learning]
header:
  image: "/images/efficient-programming-with-python.png"
  excerpt: "Why is Python considered one of the most efficient programming language? Here is one simple example where Python results in 92% reduction of lines of code while enhancing readability!"
toc: true
toc_label: "Comparison"
toc_icon: "code"
---

Why is Python considered one of the most efficient programming language? Here is one simple example where Python results in 92% reduction of lines of code while enhancing readability!

Let's find out **why Python is significantly more expressive than other widely used languages**, such as C.

By expressive, I mean that **Python makes it easy to write code that's synthetic and yet readable** - both these qualities lead to *efficient programming*.

Since Python is **synthetic**, you are able to program complex algorithms with few lines of code.

Since Python is **readable**, you will be able to share your code efficiently with your colleagues, or to understand it quickly when you go back to it after months or years.

# Comparison

To compare C and Python, here is a simple script that computes the value of π by means of the **Leibniz series**. The Leibniz series is not the best way to compute π though, because the terms in the series have alternating signs and decrease very slowly (the sum takes over 300 terms to obtain a precision of just 2 decimal places).

## C code

Here is the code in C.

~~~c
#include <stdio.h>
#include <math.h>

int main(int argc, char **argv)
{
    int k;
    double acc = 0.0;

    for(k=0;k<10000;k++) {
        acc = acc + pow(-1,k)/(2*k+1);
    }

    acc = 4 * acc;

    printf("pi: %.15f\n", acc);
}
~~~


## Python code

Now let's write the Python code.

First, we don't need any import statements: basic math is always available in Python.

Second, we can remove the boilerplate section: main, return, and their indentation.

Third, we can remove the variable declaration since Python is dynamically typed and so it doesn't need to know about the types of variables. However, we still need to initialize the sum to 0.

Fourth, we can replace the for loop with range.

Fifth, we can replace the print statement with a more friendly version.

Finally, we can get rid of all the semicolons.

~~~python
acc = 0.0

for k in range(10000):
    acc += pow(-1,k)/(2*k+1)

acc *= 4

print("pi:", acc)
~~~

## Better Python code

Although the Python code we just came up with is not too bad, as we went from 12 lines of code down to only 5 (58% reduction), we can do even better with Python.

Trust me: we can make it with only 1 line of code.

Here it is our **one-liner program in Python**.

~~~python
print("pi:", 4*sum(pow(-1,k)/(2*k+1) for k in range(10000)))
~~~

This is a one-line generator expression that uses Python built-in function sum to adapt a sequence of numbers, and we fit it directly with individual terms in the series.

# Conclusions

We replaced 12 lines of C with 1 line of Python: that is **92% reduction of lines of code**!

Isn't it awesome? :)

With Python we include only the minimum amount of code necessary to obtain the functionality we desire, and no more.

As a result, Python code is much more readable: **Python code reads like it runs**!

I hope you found this post useful and I would love to hear from you what you think about my Python script.

~Giuseppe
