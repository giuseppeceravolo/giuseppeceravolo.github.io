---
title: "Identify strings made of matching symbols"
date: 2020-03-28
tags: [python, learning, interview]
header:
  image: "/images/identify-strings-made-of-matching-symbols.png"
  excerpt: "Write a function to check whether a string of symbols is balanced or not, meaning if it is made of pairs of opening and closing parentheses."
toc: true
toc_label: "Case"
toc_icon: "code"
---

Let's build a function that mimics what happens in the text editor when the **linter** is checking for whether or not you remembered to close all of your opening symbols. This function checks for the parentheses to be balanced,
each opening parenthesis should also have a closing parenthesis, and the parentheses must be properly nested.

# Question

Create a function that takes in a string of symbols as a parameter and return *True* if the symbol string
is balanced or *False* if it is not.
In particular, the string should only contain opening and closing parentheses.

## Example of input and final output

Here you can find and example of the input file, a list of words.

| input         | output    |
|---------------|-----------|
| ([]{})        | *True*    |
| ([]{}())      | *True*    |
| [[][][]]      | *True*    |
| (((())))      | *True*    |
| ]{))[]        | *False*   |
| []{(((]}      | *False*   |
| ([{)(}])      | *False*   |
| ({}][[)       | *False*   |
| ([a])         | *False*   |

Hint: make use of a stack in your solution!

# Lay out your structure


My favourite approach to solving this exercise is the following: **fail fast**.

In other words, I aim at returning *False* as soon as I hit a mismatch or an inconsistency, and if I manage to go through the entire input string then I return *True*.

Specifically, I am going to loop through each symbol of the input string and:
1. in case it is an opening symbol, I push it onto the stack
2. in case it is an ending symbol, I check if it matches the item at the top of the stack and:
  1. if they are a matching pair of symbols, then I remove the top symbol of the stack and move to the next symbol of the input string
  2. if they are not a matching pair of symbols, then I can immediately return *False*

The above process continues until all symbols of the input string are check and the stack is empty.

Also, as a quick consistency check, I am going to verify that the length of the input string is an even number - if that is not the case I will immediately return *False*.

As suggested by the hint, I am first going to implement a stack object.

# Step 1: Implement a stack

Let's write down some code to implement a **stack**.

~~~python
class Stack:

    def __init__(self):
        self.items = []

    def push(self, item):
        """Accepts an item as a parameter and appends it to the end of the list.
        Returns nothing.

        The runtime for this method is O(1), or constant time, because appending
        to the end of a list happens in constant time.
        """

        self.items.append(item)

    def pop(self):
        """Removes and returns the last item for the list, which is also the
        top item of the Stack.

        The runtime here is constant time, because all it does is index to the
        last item of the list.
        """

        if self.items:
            return self.items.pop()

        return None

    def peek(self):
        """This method returns the last item in the list, which is also the item
        at the top of the Stack.

        This method runs in constant time because indexing into a list is done
        in constant time."""

        if self.items:
            return self.items[-1]
        return None

    def size(self):
        """This method returns the length of the list that is representing the
        Stack.

        This method runs in constant time because finding the length of a list
        also happens in constant time.
        """

        return len(self.items)

    def is_empty(self):
        """This method returns a Boolean value describing whether or not the
        Stack is empty.

        Testing for equality happens in constant time.
        """

        return self.items == []
~~~

# Step 2: write the function

Now we are ready to write our function following the instructions discussed above.

~~~python
def check_balanced_symbols(string):
    """"
    This function verifies that the input string:
        - only contains opening and closing parentheses
        - is balanced, meaning each opening symbol has its corresponding closing symbol
        - is ordered, meaning each opening symbol precedes its corresponding closing symbol

    In case the above conditions are met, the functions returns True, otherwise it returns False.
    """

    matching_symbols_dict = {
        '(': ')',
        '[': ']',
        '{': '}'
    }
    allowed_characters = list(matching_symbols_dict.keys()) + list(matching_symbols_dict.values())

    if len(string) % 2 != 0:
        return False

    if all(c in allowed_characters for c in string):
        my_stack = Stack()
        for c in range(len(string)):
            if string[c] in matching_symbols_dict.keys():
                my_stack.push(string[c])
            else:
                if my_stack.is_empty():
                    return False
                elif matching_symbols_dict[my_stack.peek()] == string[c]:
                    my_stack.pop()
                else:
                    return False
        if my_stack.is_empty():
            return True
        else:
            return False
    else:
        return False
~~~

# Step 3: run the tests

Let's check if the function works as expected.

We can perform manual checks via prompt by letting the user define a string and test it. Here is the code for it.

~~~python
input_string = input("Type the string of symbols to check: ")
print("The string you typed is:", input_string)
print("Is the above string made of balanced symbols?", check_balanced_symbols(input_string))

~~~

And here an example of this check.

~~~
Type the string of symbols to check: [][](({})[])
The string you typed is: [][](({})[])
Is the above string made of balanced symbols? True
~~~

Alternatively, we can check all strings contained in a list and verify the expected result. Here is the code for it.

~~~python
strings_to_test = ['([]{})', '([]{}())', '[[][][]]', '(((())))', ']{))[]', '[]{(((]}', '([{)(}])', '({}][[)', '([a])']
strings_expected_result = [True, True, True, True, False, False, False, False, False]

for s, r in zip(strings_to_test, strings_expected_result):
    print("Input string to test:", s)
    print("Expected result for string:", r)
    print("Actual result for string:", check_balanced_symbols(s))
    print("-" * 50)

~~~

And here is the output for your reference.

~~~
Input string to test: ([]{})
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: ([]{}())
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: [[][][]]
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: (((())))
Expected result for string: True
Actual result for string: True
--------------------------------------------------
Input string to test: ]{))[]
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: []{(((]}
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: ([{)(}])
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: ({}][[)
Expected result for string: False
Actual result for string: False
--------------------------------------------------
Input string to test: ([a])
Expected result for string: False
Actual result for string: False
--------------------------------------------------

Process finished with exit code 0

~~~

# Conclusions

I hope you found this post useful - I would love to hear from you what you think about my python script.

Did you manage to find an alternative, perhaps more efficient, solution?

Also, please share with me one coding challenge you had to face recently! :)

~Giuseppe
