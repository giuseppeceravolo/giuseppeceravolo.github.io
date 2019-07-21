---
title: "5 Simple Yet Tricky Python Interview Exercises"
date: 2019-07-20
tags: [python, learning, interview]
header:
  image: "/images/python-interview-questions.png"
  excerpt: "Here's some of my very first Python challenges. Can you solve these 5 python interview exercises in no more than 15 minutes?"
---

When did you start learning Python? Do you remember your first Python piece of code? I remember doing my first Python exercises: I was so excited about *learning one of the most used programming language for Data Science*, although I was tackling very basic exercises.

In this post I challenge you to solve **5 easy, yet tricky, Python exercises that you may be asked to solve during a job interview**. I reckon you should be able to solve flawlessly all of them in about 15-20 minutes.

Good luck and have fun! ;)


## *CHALLENGE 1: Print F made of x's*

Using nested loops, write a piece of code to draw an F made of multiple x's, just like in the image below.

![Python Challenge Draw F Shape Output]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-draw-f-shape-output.png)
{: .full}

HINT: make use of a list such as `numbers = [5, 2, 5, 2, 2]` the numbers in the list determine the number of x's to print on each line.

DO NOT CHEAT: you should not multiply each number in the list by a string 'x'... You have to use a nested loop to generate a string that contains x's.

### Solution of Challenge 1

```python
numbers = [5, 2, 5, 2, 2]

for number in numbers:
    output_line = ''
    for x in range(number):
        output_line += 'x'
    print(output_line)
```






So, did you manage to solve them within 15-20 minutes?

I would love to hear from you what you think about my solutions.
Also, what Python exercises would you suggest to prepare yourself for a job interview?

If you are new to Python and want to learn how to program in Python, I would suggest you the online course of []().
