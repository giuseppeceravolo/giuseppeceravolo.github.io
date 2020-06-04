---
title: "Using Pythonic code to analyze Olympic medallist data"
date: 2020-05-05
tags: [python, learning, interview]
header:
  image: "/images/using-pythonic-code-to-analyze-olympic-medallist-data.png"
  excerpt: "Python makes it easy to answer complex questions about a set of data by exploiting the power of comprehensions, iterators and collections"
toc: true
toc_label: "Case"
toc_icon: "code"
---

One of the main reasons why I love Python is its beauty and efficiency: Python makes it easy to answer complex questions about a set of data by exploiting the power of comprehensions, iterators and collections.

I recently did a programming challenge where I wrote Pythonic code to analyze the Olympic gold medallists.

# Question

The task is twofold:

1. Find the five athletes who won the most gold medals.
2. Find the five athletes who won gold medals in the largest number of different events.

Hint: be careful that slightly different names may correspond to the same event.

# Lay out your structure

Here is the structure I am going to follow to solve this challenge:

1. Load the data set of gold medallists
2. Find the five athletes who won the most gold medal
3. Find the five athletes who won gold medals in the most events

# Step 1: load data
```python

medals = [medal(*line.strip().split('\t')) for line in open(directory_path+data_file_name,'r')]
```

We are playing with data about all the gold medals that were won at the Olympics in athletics.

Before we can parse it we should understand its structure: we can open it in reading mode, read all the lines, and display only the first 10.

~~~python
print(open("goldmedals.txt","r").readlines()[:10])
~~~

~~~
['1896\tThomas Burke\tUSA\t100m men\n',
 '1896\tThomas Curtis\tUSA\t110m hurdles men\n',
 '1896\tEdwin Flack\tAUS\t1500m men\n',
 '1896\tThomas Burke\tUSA\t400m men\n',
 '1896\tEdwin Flack\tAUS\t800m men\n',
 '1896\tRobert Garrett\tUSA\tdiscus throw men\n',
 '1896\tEllery Clark\tUSA\thigh jump men\n',
 '1896\tEllery Clark\tUSA\tlong jump men\n',
 '1896\tSpyridon Louis\tGRE\tmarathon men\n',
 '1896\tWilliam Welles Hoyt\tUSA\tpole vault men\n']
~~~

The data looks like simple tabular data separated by tabs (i.e. by "\t"). Although the best way to handle tabular data is Pandas, let's stick with the core features of the standard library of Python.

What should we do? We could store each record in a Python dictionary (using the keys year, athlete, team, event), but dicts are relatively memory-heavy objects and, furthermore, they are mutable since keys can be added and values can be changed, which is not appropriate for medals - we definitely don't want to mess  with gold medals.

A more efficient, immutable object that can play the same role is **namedtuple** from the collections module. Roughly speaking, a namedtuple is to a dict what a tuple is to a list. There is a fixed set of keys and the values cannot be changed once the namedtuple is created.

So, we first define the namedtuple for gold medals.

~~~python
import collections


medal = collection.namedtuple("medal", ["year", "athlete", "team", "event"])
~~~

Then, we read the file by means of a list comprehension to go through the lines of the file. In particular, we split each line at the tabs, and we use the star notation to spread the data of each line across to the corresponding argument of the medal constructor. Also, we get rid of the new line character at the end of each line with the *strip()* method.

~~~python
medals = [medal(*line.strip().split("\t")) for line in open("goldmedals.txt","r")]
~~~

Let's have a look at the medals list, which is made of distinct medal namedtuples.

~~~python
print(medals[:10])
~~~
~~~
[medal(year='1896', athlete='Thomas Burke', team='USA', event='100m men'),
 medal(year='1896', athlete='Thomas Curtis', team='USA', event='110m hurdles men'),
 medal(year='1896', athlete='Edwin Flack', team='AUS', event='1500m men'),
 medal(year='1896', athlete='Thomas Burke', team='USA', event='400m men'),
 medal(year='1896', athlete='Edwin Flack', team='AUS', event='800m men'),
 medal(year='1896', athlete='Robert Garrett', team='USA', event='discus throw men'),
 medal(year='1896', athlete='Ellery Clark', team='USA', event='high jump men'),
 medal(year='1896', athlete='Ellery Clark', team='USA', event='long jump men'),
 medal(year='1896', athlete='Spyridon Louis', team='GRE', event='marathon men'),
 medal(year='1896', athlete='William Welles Hoyt', team='USA', event='pole vault men')]
~~~

Excellent, let's start counting medals now! :)

# Step 2: find the top 5 gold medallists

Finding the athletes with the most gold medals is a simple application of **collections.Counter**. What we are counting is the number of times that each athlete name appears.

~~~python
medals_by_athlete = collections.Counter(medal.athlete for medal in medals)
~~~

Then, we can call the *most_common()* method to get the five best.

~~~python
medals_by_athlete.most_common(5)
~~~

~~~
[('Paavo Nurmi', 9),
 ('Carl Lewis', 9),
 ('Usain Bolt', 9),
 ('Ray Ewry', 8),
 ('Allyson Felix', 6)]
~~~

It looks like Paavo Nurmi, Carl Lewis, and Usain Bolt are tied at 9 gold medals.

# Step 3: find the gold medallists in the most events

In order to find the athletes with gold medals in the most events, we need to collect events won by each athlete.

We can utilize a defaultdict to this purpose, and it should be **a defaultdict of sets** to avoid repeating the same event. So we can go through the medals and build the dict, adding to sets that will initialize as empty.

~~~python
events_by_athlete_set = collections.defaultdict(set)

for medal in medals:
    events_by_athlete_set[medal.athlete].add(medal.event)
~~~

Let's have a look at the dict we just created (I'm going to omit part of the output for the sake of readability).

~~~python
print(events_by_athlete_set)
~~~

~~~
defaultdict(set,
            {'Abdon Pamich': {'50km walk men'},
             'Abebe Bikila': {'marathon men'},
             'Abel Kiviat': {'3000m team men'},
             'Adhemar Ferreira Da Silva': {'triple jump men'},
             'Adolfo Consolini': {'discus throw men'},
             'Aksana Miankova': {'hammer throw women'},
             ...
             'Yuri Tarmak': {'high jump men'},
             'Yuriy Bilonog': {'shot put men'},
             'Yuriy Borzakovskiy': {'800m men'},
             'Yvette Winefred Williams': {'long jump women'},
             'Zdzislaw Krzyszkowiak': {'3000m steeplechase men'},
             'Zhen Wang': {'20km walk'}})
~~~

By default, the dict is ordered by athlete name in alphabetical order, but we can turn the dict into a list that is sorted in descending order by the number of different events who by each athlete. We can do so by means of  *sorted()* with a key function that counts the elements in the set of events won by each athlete - we have to write an ad-hoc function for that purpose.

Recall that both list.sort() and sorted() have a key parameter to specify a function to be called on each list element prior to making comparisons. The value of the key parameter should be a function that takes a single argument and returns a key to use for sorting purposes. This technique is fast because the key function is called exactly once for each input record.

In this case, I am going to implement a lambda function that will evaluate the length of the sets containing the events won by each athlete.

~~~python
sorted_events_by_athlete = sorted(events_by_athlete_set.items(),
                                  key=lambda athlete: len(athlete[1]),
                                  reverse=True)
~~~

Let's print the five elements of this sorted list.

~~~python
print(sorted_events_by_athlete [:5])
~~~

~~~
[('Paavo Nurmi',
  {'10000m men',
   '1500m men',
   '3000m team men',
   '5000m men',
   'cross country individual men',
   'cross country team men'}),
 ('Usain Bolt',
  {'100m',
   '100m men',
   '200m',
   '200m men',
   '4x100m relay',
   '4x100m relay men'}),
 ('Ville Ritola',
  {'10000m men',
   '3000m steeplechase men',
   '3000m team men',
   '5000m men',
   'cross country team men'}),
 ('Allyson Felix',
  {'200m women',
   '4x100m relay',
   '4x100m relay women',
   '4x400m relay',
   '4x400m relay women'}),
 ('Alvin Kraenzlein',
  {'110m hurdles men',
   '200m hurdles men',
   '60m men',
   'long jump men'})]
~~~

This is almost what we want, but we notice that, for example, "100m" and "100m men" are being counted as different events, so we still need a function to clean the events name by removing both "men" and "women".

~~~python
def clean_men_and_women(string):
    return ' '.join(word for word in string.split() if word not in ("men", "women"))
~~~

Now we can repeat the previous steps while making sure to clean the events name.

~~~python
events_by_athlete_set = collections.defaultdict(set)

for medal in medals:
    events_by_athlete_set[medal.athlete].add(clean_men_and_women(medal.event))

sorted_events_by_athlete = sorted(events_by_athlete_set.items(),
                                  key=lambda athlete: len(athlete[1]),
                                  reverse=True)

print(sorted_events_by_athlete[:5])
~~~

~~~
[('Paavo Nurmi', {'10000m', '1500m', '3000m team', '5000m', 'cross country individual', 'cross country team'}),
 ('Ville Ritola', {'10000m', '3000m steeplechase', '3000m team', '5000m', 'cross country team'}),
 ('Alvin Kraenzlein', {'110m hurdles', '200m hurdles', '60m', 'long jump'}),
 ('Hannes Kolehmainen',  {'10000m', '5000m', 'cross country individual', 'marathon'}),
 ('Jesse Owens', {'100m', '200m', '4x100m relay', 'long jump'})]
~~~

There we go: this is our solution!

It looks like long-distance runners are the most versatile athletes followed by sprinters.

If we want just the name of the athletes who won gold medals in the largest number of different events, then we can print only their names like so:

~~~python
print([events_by_athlete[0] for events_by_athlete in sorted_events_by_athlete[:5]])
~~~

~~~
['Paavo Nurmi',
 'Ville Ritola',
 'Alvin Kraenzlein',
 'Hannes Kolehmainen',
 'Jesse Owens']
~~~

# Conclusions

We analyzed the Olympic gold medallists data with great **Pythonic code**!

I hope you found this post useful and I would love to hear from you what you think about this exercise.

Also, please share with me one coding challenge you had to face recently! :)

~Giuseppe
