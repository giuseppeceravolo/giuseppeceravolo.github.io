---
title: "Object-oriented implementation in Python of a printer and its printing queue"
date: 2020-03-30
tags: [python, learning, interview]
header:
  image: "/images/python-code-for-object-oriented-printer-queue.png"
  excerpt: "Write a Python code to implement an object-oriented Printer together with is Printing Queue."
toc: true
toc_label: "Case"
toc_icon: "code"
---

Let's build an **object-oriented implementation of a printer**. In particular, we want to mimic the behaviour of the printer by means of its printing queue given some print jobs as input: a printer takes print jobs as input and prints them out of a queue, which is well-know data structure.

To this purpose, we will need 3 classes, namely the **PrintJob**, the **PrintQueue**, and the **Printer**. You can refer to the image below to better understand the expected behaviour.

![This is an object-oriented schema of a printer](/images/object-oriented-schema-of-a-printer.png)

# Question

Write an object-oriented Python script to implement the 3 classes and mimic the behaviour of a printer.

## Requirements

In particular, here are the name of the 3 classes:
 - **PrintJob** that:
    - has a *name* attribute that we can use to refer to it
    - has a *pages* attribute (a random number from 1 to 10)
    - has a *printed* attribute
    - has a *check_printed()* method that will check that both all of its pages have been printed and the Printer has labelled it (i.e. changed its status) as 'printed'
 - **PrintQueue** that follows the queue data structure implementation and has the following methods:
    - *enqueue()* to add items in 0th index of the PrintQueue
    - *dequeue()* to remove the front-most item of the PrintQueue
    - *peek()* to return the front-most item of the queue (without removing it)
    - *size()* to return the length of the PrintQueue
    - *is_empty()* to check whether or not the PrintQueue is empty
    - *contains()* to check whether or not the PrintQueue contains a given item
 - **Printer** that initializes with an empty PrintQueue and has the following methods:
    - *get_job()* that makes use of PrintQueue's enqueue() method to add a PrintJob in the PrintQueue
    - *print_job()* that makes use of PrintQueue's dequeue() method to take the first PrintJob out of the PrintQueue and both print all its pages and changes its status as 'printed'
    - *show_next_job_in_queue()* that makes use of PrintQueue's peek() method to return the next PrintJob to be printed in the PrintQueue (without removing it from the PrintQueue)
    - *show_print_queue()* to show the length of the PrintQueue as well as the name, pages, and position of each PrintJob in it.

Please note that, before adding the PrintJob to the PrintQueue, the Printer's *get_job()* method should check, in order:
    1. if the argument provided is a PrintJob object
    2. if the PrintJob is already 'printed'
    3. if the PrintJob is already in the PrintQueue
and in case any of the above conditions is met it will need to raise a proper Exception.

Also, note that, before trying to show or print any PrintJob in the PrintQueue, all other Printer's methods should check if the PrintQueue is empty.
In case the PrintQueue is empty, the methods will need to raise a proper Exception.

Hint: make use of a **queue** in your solution!

# Lay out your structure

My approach to solving this exercise is the following:
  1. define PrintJob class
  2. define PrintQueue class
  3. define Printer class

In particular, I am going to let Printer initialize and work with a PrintQueue which is a queue made of PrintJobs.

# PrintJob class

I am going to define a class called PrintJob that initializes with two attributes, namely name and pages, and with 1 method called *check_printed()* that checks if the PrintJob has been printed.

~~~python
class PrintJob:

    def __init__(self, name):
        self.pages = random.randint(1, 10)
        self.name = name
        self.printed = False
        print("This PrintJob named {} has {} pages.".format(self.name, self.pages))

    def check_printed(self):
        """Returns a Boolean value expressing whether or not the number
        of pages of the Print Job is equal to 0 and the Printer has
        labelled it as 'printed' - meaning there are no more pages
        left to print and printing was successful.

        Runs in constant time, because it's only checking for equalities.
        """

        return self.pages == 0 and self.printed
~~~

# PrintQueue class

As per the PrintQueue class, I am going to implement each and every method detailed in the exercise requirements together with its required exceptions. Also, as suggested by the hint, I am going to implement a queue object by means of a Python list, which I initialize as an empty list.

~~~python
class PrintQueue:

    def __init__(self):
        self.items = []

    def enqueue(self, item):
        """Takes in an item and inserts that item into the 0th index of the list
        that is representing the Queue.

        The runtime is O(n), or linear time, because inserting into the 0th
        index of a list forces all the other items in the list to move one index
        to the right.
        """

        self.items.insert(0, item)

    def dequeue(self):
        """Returns and removes the front-most item of the Queue, which is
        represented by the last item in the list.

        The runtime is O(1), or constant time, because indexing to the end of
        a list happens in constant time.
        """

        if self.items:
            return self.items.pop()
        return None

    def peek(self):
        """Returns the last item in the list, which represents the front-most
        item in the Queue.

        The runtime is constant because we're just indexing to the last item of
        the list and returning the value found there.
        """

        if self.items:
            return self.items[-1]
        return None

    def size(self):
        """Returns the size of the Queue, which is represented by the length of
        the list.

        The runtime is O(1), or constant time, because all we're doing is finding
        the length of a list and returning that value.
        """

        return len(self.items)

    def is_empty(self):
        """Returns a Boolean value expressing whether or not the list
        representing the Queue is empty.

        Runs in constant time, because it's only checking for equality.
        """

        return self.items == []

    def contains(self, item):
        """Returns a Boolean value expressing whether or not an item
        provided as input is part of the list representing the Queue.

        Runs in linear time, O(n), because we need to check all items
        in the list representing the Queue.
        """
        return item in self.items
~~~

# Printer class

Now I can code the Printer class by means of the PrintQueue class. Also, I am going to implement each and every method detailed in the exercise requirements as well as the required exceptions.

~~~python
class Printer:

    def __init__(self):
        self.print_queue = PrintQueue()

    def get_job(self, print_job):
        if not isinstance(print_job, PrintJob):
            raise Exception("The argument must be a PrintJob object.")

        if print_job.check_printed():
            raise Exception("The PrintJob provided has already been printed by Printer.")

        if self.print_queue.contains(print_job):
            raise Exception("The PrintJob provided is already in PrintQueue.")

        self.print_queue.enqueue(print_job)
        print("The PrintJob named {} has been added to the PrintQueue.".format(print_job.name))

    def print_job(self):
        if self.print_queue.is_empty():
            raise Exception("The PrintQueue is empty.")

        job_to_print = self.print_queue.dequeue()
        print("Printing the first PrintJob in PrintQueue, which is named {}.".format(job_to_print.name))
        while job_to_print.pages > 0:
            print(" --- Printing page number:", job_to_print.pages)
            job_to_print.pages -= 1
            job_to_print.printed = True
        if job_to_print.check_printed():
            print("Printing is complete.")

    def show_next_job_in_queue(self):
        if self.print_queue.is_empty():
            raise Exception("The PrintQueue is empty.")

        next_job_in_queue = self.print_queue.peek()
        print("The next PrintJob in PrintQueue is {}.".format(next_job_in_queue.name))

    def show_print_queue(self):
        if self.print_queue.is_empty():
            raise Exception("The PrintQueue is empty.")

        print("The current PrintQueue is made of {} PrintJobs, namely:".format(self.print_queue.size()))
        for i in reversed(range(len(self.print_queue.items))):
            print("name: {} pages: {} position: {}".format(self.print_queue.items[i].name,
                                                           self.print_queue.items[i].pages,
                                                           len(self.print_queue.items)-i))
~~~

# Run the code

Let's check if the code works as expected.

We can interactively run the code via the command line (the prompt) and test it all code functionalities. Here is some code I ran and its output.

~~~
C:\Users\...>python -i printer_queue.py
>>> printer = Printer() # we initialize a Printer object
>>> job1 = PrintJob("Job1")  # we initialize a PrintJob object called "Job1"
This PrintJob named Job1 has 3 pages.
>>> job2 = PrintJob("Job2") # we initialize a PrintJob object called "Job2"
This PrintJob named Job2 has 1 pages.
>>> job3 = PrintJob("Job3") # we initialize a PrintJob object called "Job3"
This PrintJob named Job3 has 9 pages.
>>> fake_job = 'This is not a JobPrint ojbect!' # this is NOT a PrintJob
>>> printer.get_job(fake_job) # does Printer detect the class inconsistency?
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "printer_queue.py", line 136, in get_job
    raise Exception("The argument must be a PrintJob object.")
Exception: The argument must be a PrintJob object.
>>> printer.get_job(job1) # we add "Job1" to the PrintQueue
The PrintJob named Job1 has been added to the PrintQueue.
>>> printer.get_job(job3) # we add "Job3" to the PrintQueue
The PrintJob named Job3 has been added to the PrintQueue.
>>> printer.get_job(job2) # we add "Job3" to the PrintQueue
The PrintJob named Job2 has been added to the PrintQueue.
>>> printer.show_next_job_in_queue() # which PrintJob is the first to be printed?
The next PrintJob in PrintQueue is Job1.
>>> printer.show_print_queue() # let's have a look at the entire PrintQueue
The current PrintQueue is made of 3 PrintJobs, namely:
name: Job1 pages: 3 position: 1
name: Job3 pages: 9 position: 2
name: Job2 pages: 1 position: 3
>>> printer.print_job() # let's print the first PritnJob, which is "Job1"
Printing the first PrintJob in PrintQueue, which is named Job1.
 --- Printing page number: 3
 --- Printing page number: 2
 --- Printing page number: 1
Printing is complete.
>>> printer.get_job(job1) # does Printer realize we have already printed "Job1"?
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "printer_queue.py", line 139, in get_job
    raise Exception("The PrintJob provided has already been printed by Printer.")
Exception: The PrintJob provided has already been printed by Printer.
>>> printer.show_print_queue() # let's have a look at the current PrintQueue
The current PrintQueue is made of 2 PrintJobs, namely:
name: Job3 pages: 9 position: 1
name: Job2 pages: 1 position: 2
>>> printer.print_job() # let's print the first PritnJob, which is "Job3"
Printing the first PrintJob in PrintQueue, which is named Job3.
 --- Printing page number: 9
 --- Printing page number: 8
 --- Printing page number: 7
 --- Printing page number: 6
 --- Printing page number: 5
 --- Printing page number: 4
 --- Printing page number: 3
 --- Printing page number: 2
 --- Printing page number: 1
Printing is complete.
>>> printer.print_job() # let's print the first PritnJob, which is "Job2"
Printing the first PrintJob in PrintQueue, which is named Job2.
 --- Printing page number: 1
Printing is complete.
>>> printer.print_job() # does Printer realize that PrintQueue is empty?
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "printer_queue.py", line 149, in print_job
    raise Exception("The PrintQueue is empty.")
Exception: The PrintQueue is empty.
~~~

Awesome! It looks like all functionalities work as expected: we have just coded an object-oriented implementation of a printer!

# Conclusions

I hope you found this post useful - I would love to hear from you what you think about my python script.

Did you manage to find an alternative, perhaps more efficient, solution?

Also, please share with me one coding challenge you had to face recently! :)

~Giuseppe
