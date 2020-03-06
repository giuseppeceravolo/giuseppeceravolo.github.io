---
title: "Select a random number from a stream with O(1) space"
date: 2020-03-04
tags: [python, learning, interview]
header:
  image: "/images/select-a-random-number-from-stream-with-o1-space.jpg"
  excerpt: "Let's write a piece of code to pick every element in a stream with 1/n probability, where n is the number of items seen so far."
toc: true
toc_label: "Case"
toc_icon: "code"
---

An algorithm is said to take *"constant space complexity"*, i.e. *O(1)*, if *the additional space complexity used by the algorithm*, meaning the extra space that it needs apart from the initial space occupied by the input data, *is constant regardless of the amount of input data*.

This concept is different than an algorithm is said to run in "constant time", which, instead, requires the algorithm to run in the same, and constant, amount of time regardless of the input size.

# Question

Write a piece of code that selects a random number from a stream of consequent positive integers with O(1) space in selection.

# Verify the objective

In other words, the code has to take an amount of space (memory) that is not proportional to the amount of the input data, yet is constant and equal to 1.

# Lay out the structure

In this exercise we want to define an algorithm that uses only O(1) space, so it can’t store more than 1 number - and not all previously seen numbers.

In particular, we need to prove that every element is retrieved from the stream with 1/n probability where n is the number of items seen so far.

# Code

~~~python
import random


# a function to randomly select an item from stream[1], stream[2], ..., stream[i]
def select_random_number_from_stream(up_to, i, previous):
    """
    This function works on a stream of increasing integers, which starts from 1.
    This function is called only once the second number in stream is reached, meaning when
    up_to=stream[1], i=1 and previous=stream[0]

    :param up_to: stream[i]
    :param i: i
    :param previous: the last picked random number
    :return: a random number from stream with probability 1/up_to
    """
    print("i={} up_to={} previous={}".format(i, up_to, previous))
    # generate a random number from 1 to up_to
    rnd = random.randrange(start=1, stop=up_to+1)
    print("rnd=random.randrange({})={}".format(up_to, rnd))
    # replace the previous random number with new random number with 1/up_to probability
    if rnd == up_to:
        res = up_to
        print("rnd == up_to --> up_to is chosen :", res)
    else:
        res = previous
        print("rnd != up_to --> previous is chosen :", res)
    return res


stream = [1, 2, 3, 4, 5, 6, 7, 8]
n = len(stream)

for i in range(n):
    if i == 0:
        x = stream[0]
        print("i={} up_to={} previous=nan".format(i, stream[0]))
        print("Random number from first", (i + 1), "number is", x)
    else:
        x = select_random_number_from_stream(up_to=stream[i], i=i, previous=x)
        print("Random number from first", (i + 1), "numbers is", x)
    print("-"*50)

~~~

# Example Result

~~~python
i=0 up_to=1 previous=nan
Random number from first 1 number is 1
--------------------------------------------------
i=1 up_to=2 previous=1
rnd=random.randrange(2)=2
rnd == up_to --> up_to is chosen : 2
Random number from first 2 numbers is 2
--------------------------------------------------
i=2 up_to=3 previous=2
rnd=random.randrange(3)=3
rnd == up_to --> up_to is chosen : 3
Random number from first 3 numbers is 3
--------------------------------------------------
i=3 up_to=4 previous=3
rnd=random.randrange(4)=4
rnd == up_to --> up_to is chosen : 4
Random number from first 4 numbers is 4
--------------------------------------------------
i=4 up_to=5 previous=4
rnd=random.randrange(5)=3
rnd != up_to --> previous is chosen : 4
Random number from first 5 numbers is 4
--------------------------------------------------
i=5 up_to=6 previous=4
rnd=random.randrange(6)=6
rnd == up_to --> up_to is chosen : 6
Random number from first 6 numbers is 6
--------------------------------------------------
i=6 up_to=7 previous=6
rnd=random.randrange(7)=1
rnd != up_to --> previous is chosen : 6
Random number from first 7 numbers is 6
--------------------------------------------------
i=7 up_to=8 previous=6
rnd=random.randrange(8)=3
rnd != up_to --> previous is chosen : 6
Random number from first 8 numbers is 6
--------------------------------------------------

Process finished with exit code 0

~~~

# Code Walkthrough

Here is how the code works.

For the very first item in the stream, stream[0], we just keep it and return it.

For every new stream item stream[i] with i>0, we pick a random number from 1 to stream[i]. In particular, stream[i] is referred to as 'up_to' within the function *select_random_number_from_stream*.
Then, within the aforementioned function, we generate a random number from 1, which is the very first item in the stream by constraint, up to stream[i] by means of the following formula rnd = random.randrange(start=1, stop=up_to+1).
Here we have two alternative scenarios:
 - if the picked random number is equal to stream[i], in other words in case *rnd = up_to*, then we replace the previous result, meaning the previously picked random number, with up_to
 - if the picked random number is not equal to stream[i], then we keep the previously picked random number

Now we need to **prove that this code is able to pick every element in the stream with 1/n probability, where n is the number of items seen so far**.

To simplify proof, **let us first consider when the **last element** is processed and its probability of being picked.
The last element, stream[n], replaces the previously-stored result with *1/n probability*: only when the random number generated within the function is equal to stream[n].

Next, **let us consider when the second-last element** is processed and its total probability of being picked.
When the second-last element, stream[n-1], is processed the first time, namely when the for loop runs for i = n-2,
the probability that it replaces the previous result is *1/(n-1)* for the reason discussed for last element in stream (see above).
When the second-last element, stream[n-1], is processed the second and last time, namely when the for loop runs for i = n-1,
the probability that it stays as result is *(n-1)/n*: only when the random number generated within the function is not equal to stream[n].
So the total probability that the second-last element is picked is *1/(n-1) x (n-1)/n]* which is equal to *1/n*.

Similarly, **we can prove for third-last element and all the others**.

# Conclusions

I hope you have fun with this little exercise about algorithm space complexity.

Honestly, I would like to know from you what you think about my solution.

Because, I am curious, can this solution of mine be improved? :)

~Giuseppe
