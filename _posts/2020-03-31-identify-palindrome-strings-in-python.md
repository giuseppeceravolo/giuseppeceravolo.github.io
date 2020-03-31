---
title: "Identify palindrome strings in Python"
date: 2020-03-31
tags: [python, learning, interview]
header:
  image: "/images/identify-palindrome-strings-in-python.png"
  excerpt: "Write a function to check whether a string is made a palindrome by means of a deque data structure."
toc: true
toc_label: "Case"
toc_icon: "code"
---

Let's build a function that mimics a **palindrome checker** in Python!

As a reminder, a palindrome is a word that is spelled the same both forwards and backwards.

# Question

The task is write a function that takes in an input string and verifies whether or not it is a palindrome.
Specifically, the function must return either *True* if the string is a palindrome or *False* if the string is not a palindrome.

## Examples

Please refer to the table below as per some examples of input strings and the corresponding function outputs.

| input         | output    |
|---------------|-----------|
| mom           | *True*    |
| level         | *True*    |
| kayak         | *True*    |
| dad           | *True*    |
| 02022020      | *True*    |
| president     | *False*   |
| data          | *False*   |
| work          | *False*   |
| home          | *False*   |

Hint: make use of a **deque** in your solution!

# Lay out your structure

My favourite approach to solving this exercise is the following: **fail fast**.

In other words, I aim at returning False as soon as I hit a mismatch and if I manage to go through the entire input string then I return True.

Specifically, I am first going to implement a deque object and then I am going to:

1. loop through the entire input string and add all its characters to the rear of the queue (\*)
2. as long as the deque's size is greater than 2:
    - remove the front-most item and the rear-most item and store each in its own variable, and
    - if they are not equal, then I return *False*; if they are equal, then I can proceed to the next pair of opposite items.

The above process continues until the size of the deque is less than 2, meaning it is either 1 or 0 - in such a case the string is indeed a palindrome and I return *True*.

(\*) Note: I chose to add all items at the rear of the deque because it occurs in constant time.

# Step 1: implement a deque

Let’s write down some code to implement a **deque**.

~~~python
class Deque:

    def __init__(self):
        self.items = []

    def add_front(self, item):
        """Takes an item as a parameter and inserts it into the 0th index
        of the list that is representing the Deque.

        The runtime is linear, or O(n), because every time you insert into the
        front of a list, all the other items in the list need to shift one
        position to the right.
        """

        self.items.insert(0, item)

    def add_rear(self, item):
        """Takes in an item as a parameter and appends that item to the end of
        the list that is representing the Deque.

        The runtime is constant, or O(1), because appending to the end of a list
        happens in constant time.
        """

        self.items.append(item)

    def remove_front(self):
        """Removes and returns the item in the 0th index of the list, which
        represents the front of the Deque.

        The runtime is linear, or O(n), because when we remove an item from the
        0th index, all the other items have to shift one index to the left.
        """

        if self.items:
            return self.items.pop(0)
        return None

    def remove_rear(self):
        """Removes and returns the last item of the list, which represents the
        rear of the Deque.

        The runtime is constant, or O(1), because all we're doing is indexing
        to the end of a list.
        """

        if self.items:
            return self.items.pop()
        return None

    def peek_front(self):
        """Returns the value found at the 0th index of the list, which represents
        the front of the Deque.

        The runtime is constant because all we're doing is indexing into a list.
        """

        if self.items:
            return self.items[0]
        return None

    def peek_rear(self):
        """Returns the value found at the -1st, or last, index.

        The runtime is constant because all we're doing is indexing into a list.
        """

        if self.items:
            return self.items[-1]
        return None

    def size(self):
        """Returns the length of the list representing the Deque.

        The runtime will be constant because all we're doing is finding
        the length of a list and returning that value.
        """

        return len(self.items)

    def is_empty(self):
        """Checks if the list representing our Deque is empty. Returns True
        if it is, or False if it is not.

        The runtime is constant because all we're doing is testing for equality.
        """

        return self.items == []
~~~

# Step 2: write the function

Now we are ready to write our function following the instructions discussed above.

~~~python
def check_palindome(string):
    """"
    This function verifies that the input string is a palindrome, meaning it is spelled
    the same both forwards and backwards.

    In case the above conditions are met, the functions returns True,
    otherwise it returns False.
    """

    my_deque = Deque()
    for char in string:
        my_deque.add_rear(char)

    while my_deque.size() >= 2:
        front_char = my_deque.remove_front()
        rear_char = my_deque.remove_rear()

        if front_char != rear_char:
            return False

    return True
~~~

# Step 3: run the tests

Let's check if the function works as expected.

We can perform manual checks via prompt by letting the user define a string and test it. Here is the code for it.

~~~python
input_string = input("Type the input string to check: ")
print("The string you typed is:", input_string)
print("Is the above string a palindrome?", check_palindome(input_string))
print("\n\n\n")

~~~

And here an example of this check.

~~~
Type the input string to check: !eppe!
The string you typed is: !eppe!
Is the above string a palindrome? True
~~~

Alternatively, we can check all strings contained in a list and verify the expected result. Here is the code for it.

~~~python
strings_to_test = ['mom', 'level', 'kayak', 'dad', '02022020', 'president', 'data', 'work', 'home']
strings_expected_result = [True, True, True, True, True, False, False, False, False]

for s, r in zip(strings_to_test, strings_expected_result):
    print("Input string to test:", s)
    print("Expected result for string:", r)
    print("Actual result for string:", check_palindome(s))
    print("-" * 50)

~~~

And here is the output for your reference.

~~~
Input string to test: mom
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: level
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: kayak
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: dad
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: 02022020
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: president
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: data
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: work
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: home
Expected result for string: False
Actual result for string: False
--------------------------------------------------
~~~

# Conclusions

I hope you found this post useful - I would love to hear from you what you think about my python script.

Did you manage to find an alternative, perhaps more efficient, solution?

Also, please share with me one coding challenge you had to face recently! :)

~Giuseppe
