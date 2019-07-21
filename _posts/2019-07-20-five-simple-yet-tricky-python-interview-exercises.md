---
title: "5 Simple Yet Tricky Python Interview Exercises"
date: 2019-07-20
tags: [python, learning, interview]
header:
  image: "/images/python-interview-questions.png"
  excerpt: "Here's some of my very first Python challenges. Can you solve these 5 python interview exercises in no more than 20 minutes?"
toc: true
toc_label: "Python Exercises"
toc_icon: "python"
---

When did you start learning Python? Do you remember your first Python piece of code? I remember doing my first Python exercises: I was so excited about *learning one of the most used programming language for Data Science*, although I was tackling very basic exercises.

In this post I challenge you to solve **5 easy, yet tricky, Python exercises that you may be asked to solve during a job interview**. I reckon you should be able to solve flawlessly all of them in no more than 20 minutes.

Good luck and have fun! ;)


## *Exercise 1: Print an F made of x's*

Using nested loops, write a piece of code to print an F made of multiple x's, just like in the image below.

![Python Challenge - Draw F Shape Output]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-draw-f-shape-output.PNG){: .align-center}

**HINT**: make use of a list such as `numbers = [5, 2, 5, 2, 2]` where the numbers in the list determine the number of x's to print on each line.

DO NOT CHEAT: you should not multiply each number in the list by a string 'x'... You have to use a nested loop to generate a string that contains x's.

**Can you print an F made of multiple x's?**

### Solution of Exercise 1

```python
numbers = [5, 2, 5, 2, 2]

for number in numbers:
    output_line = ''
    for x in range(number):
        output_line += 'x'
    print(output_line)
```



## *Exercise 2: Remove duplicates from a list of numbers*

Write a program to remove the duplicates in a list.

Let's say we have a list of numbers with a bunch of duplicates like this one: `numbers = [1, 1, 4, 6, 5, 3, 4, 8, 7]`.

**Can you print a list that contains only the unique numbers in the above list?**

### Solution of Exercise 2

```python
numbers = [1, 1, 4, 6, 5, 3, 4, 8, 7]
print(f'List of numbers: {numbers}')
uniques = []

for number in numbers:
    if number not in uniques:
        uniques.append(number)

print(f'List of unique numbers: {uniques}')
```



## *Exercise 3: Find and print the longest string in a list*

Write a program to find and print the longest string in a list.

Let's say we have a list of names like this one: `names = ["Chiara", "Gianni", "Alex", "Emi", "Giuseppe", "Davide"]`.

**Can you find and print the longest string in the above list?**

### Solution of Exercise 3

```python
names = ["Chiara", "Gianni", "Alex", "Emi", "Giuseppe", "Davide"]
print(f'Among this list of name: {names} ...')
longest_name = ""

for name in names:
    if int(len(name)) > int(len(longest_name)):
        longest_name = name

print(f'...the longest name is {longest_name}, which is {len(longest_name)}-character long')
```



## *Exercise 4: Build an Emoji Converter*

Write a program that makes use of a dictionary to convert words into emojis.

For example, if the user enters the message "I fell happy", the program will print "I fell 🙂", or if the user enters the message "I am sad", the program will print "I am 😫".

The first step is to create a dictionary that maps the strings of characters ":)" and ":(" into "🙂" and "😫", respectively.

**Can you convert special characters into emojis?**

### Solution of Exercise 4

```python
message = input(">")
words = message.split(' ')
emojis = {
    ":)": "😃",
    ":(": "😫"
}

output = ""
for word in words:
    output += emojis.get(word, word) + " "
print(output)
```



## *Exercise 5: Car Engine Simulator*

In this exercise you have to code a car engine simulator.

First, when the program is running it will be waiting for an input from the user like shown in the image below (note the "> ").

![Python Challenge - Waiting for Input]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-waiting-for-input.png){: .align-center}

This means that the program is waiting for the user to enter a command.

The program will accept one of the following four commands, either in lower or in upper case:
+ `help`: to get the list of commands that the program supports;
+ `start`: to start the car;
+ `stop`: to stop the car;
+ `quit`: to exit the game.

For example, here's the output of the help command.

![Python Challenge - Help Command]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-help-command.png){: .align-center}

Please note that any other commands that the user may enter will result in the program telling the user that *"Sorry, I don't understand that... Enter 'help' for command instructions."*.

![Python Challenge - Command Not Supported]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-command-not-supported.png){: .align-center}

Next, If you enter the command `start`, the program will reply with *"Car started... Ready to go!"*, but if the car had already been started then the program will reply with *"Car already started!"*.

![Python Challenge - Start Command]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-start-command.png){: .align-center}

Similarly, if you enter the command `stop` the program will reply with *"Car stopped."*, but if the car had already been stopped then the programm will reply with *"Car already stopped!"*.

![Python Challenge - Stop Command]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-stop-command.png){: .align-center}

Finally, if you enter the command `quit` the program will end.

![Python Challenge - Quit Command]({{ site.url }}{{ site.baseurl }}/images/exercises/python-challenge-quit-command.png){: .align-center}

**Can you simulate a "start and stop" car like this?**

### Solution of Exercise 5

```python
command = ""
is_started = False

while True:
    command = input("> ").lower()
    if command == "help":
        print("""
start - to start the car
stop - to stop the car
quit - to exit
        """)
    elif command == "start":
        if is_started == False:
            is_started = True
            print("Car started... Ready to go!")
        else:
            print("Car already started.")
    elif command == "stop":
        if is_started == True:
            is_started = False
            print("Car stopped.")
        else:
            print("Car already stopped.")
    elif command == "quit":
        break
    else:
        print("Sorry, I don't understand that... Enter 'help' for command instructions.")


print("Program ended.")
```



## Ending

So, did you manage to solve these 5 Python exercises within 20 minutes?

I would love to hear from you what you think about my solutions.

Also, please let me know what tricky Python exercises you would suggest to prepare yourself for a job interview!

~Giuseppe
