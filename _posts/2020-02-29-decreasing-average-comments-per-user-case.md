---
title: "How to find users without overlapping date ranges in SQL?"
date: 2020-02-21
tags: [sql, learning, interview]
header:
  image: "/images/sql-users-without-overlap-subscription-date-range.png"
  excerpt: "What if you need to find the users whose subscription date range does NOT overlap with any other user's subscription date range? Here's the query for you."
toc: true
toc_label: "SQL Question"
toc_icon: "code"
---

What if you wanted to highlight the users who have subscribed to your company's service in a period of time different than any other users?

Let's pretend that you are curious and wish to investigate further on those *unique* users who joined your company's service in a period different than any other users...

# Question

In other words, given a table of service subscriptions, say it is already filtered for a specific period of time you are particularly interested in, with a subscription start date and subscription end date for each user.

How would you **write a query that returns 1 (true) or 0 (false) whether or not each user has a subscription date range that does NOT overlap with any other user's subscription range**?

If you have no clue, then keep reading! ;)

Let's assume that we have a simple table like the following one.

**'subscriptions' table**:

| COLUMN_NAME   | DATA_TYPE |
|---------------|-----------|
| user_id       | int       |
| start_date    | date      |
| end_date      | date      |


## Example

Input:

| user_id | start_date | end_date   |
|---------|------------|------------|
| 1       | 2020-02-01 | 2020-02-29 |
| 2       | 2020-02-15 | 2020-02-17 |
| 3       | 2020-02-28 | 2020-03-04 |
| 4       | 2020-03-05 | 2019-03-10 |

Output:

| user_id | overlap |
|---------|---------|
| 1       | 0       |
| 2       | 0       |
| 3       | 0       |
| 4       | 1       |


# Answer's Structure Layout

Given two date ranges, A and B, what determines if they would overlap?

Let's assume that date range A is completely before date range B as below:

    |-- date range A --|
                         |-- date range B --|

We can see that the condition **A < B holds if end_A < start_B**.

On the contrary, let's assume that date range A is completely after date range B as below:

                         |-- date range A --|
    |-- date range B --|

We can see that the condition **A > B holds if start_A > end_B**.

As a result, we can state that **overlap exists if neither one of the two following conditions is true**:
- end_A < start_B
- start_A > end_B

In other terms, if one range is neither completely after the other, nor completely before the other, then they must overlap.

Let's represent such non-overlapping condition with logical operators:

**(end_a ≥ start_B) AND (start_A ≤ end_B)**

Given this logical condition, **are you able to apply it to an SQL query?**

Let me give a hint: We can use this condition in the WHERE clause of the LEFT JOIN between the same table in order to compare each user to a different user in the same table.


## LEFT JOIN

```sql
FROM subscriptions AS s1
LEFT JOIN subscriptions AS s2
    ON s1.user_id != s2.user_id
        AND s1.end_date >= s2.start_date
        AND s1.start_date <= s2.end_date
```

In this way we set s1 as our A and s2 as our B. Given this conditional join, a user_id from s2 should exist for each
user_id in s1 on the condition where there exists overlap between the dates.

Please refer to this [SQL Fiddle](http://sqlfiddle.com/#!9/d17c8/4) to run the below queries and check the solution.

## CREATE TABLE

```sql
CREATE TABLE `subscriptions` (
  `user_id` int(11) NOT NULL,
  `start_date` date NOT NULL,
  `end_date` date NOT NULL,
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
```

## INSERT (example data)

```sql
INSERT INTO `subscriptions`(`user_id`,`start_date`,`end_date`) VALUES (1,'2020-02-01','2020-02-29');
INSERT INTO `subscriptions`(`user_id`,`start_date`,`end_date`) VALUES (2,'2020-02-15','2020-02-17');
INSERT INTO `subscriptions`(`user_id`,`start_date`,`end_date`) VALUES (3,'2020-02-28','2020-03-04');
INSERT INTO `subscriptions`(`user_id`,`start_date`,`end_date`) VALUES (4,'2020-03-05','2020-03-10');
```

## Final Query

```sql
SELECT
    s1.user_id,
    MAX(CASE
        WHEN s2.user_id IS NOT NULL THEN 0
        ELSE 1
    END) AS overlap
FROM subscriptions AS s1
LEFT JOIN subscriptions AS s2
    ON s1.user_id != s2.user_id
        AND s1.start_date <= s2.end_date
        AND s1.end_date >= s2.start_date
GROUP BY 1
```

You can check the output in this [SQL Fiddle](http://sqlfiddle.com/#!9/d17c8/4).

# Conclusions

I hope you found this post useful - I would love to hear from you what you think about my query.

Did you manage to find an alternative, perhaps more efficient, solution?

Also, please share with me one SQL challenge you had to face recently during your work! :)

~Giuseppe
