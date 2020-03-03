---
title: "Find out reasons for decreasing average number of comments per user"
date: 2020-02-29
tags: [case, learning, interview]
header:
  image: "/images/decreasing-average-number-of-comments-per-user-case.png"
  excerpt: "How would you investigate potential reasons for a slow yet constant decrease in the average number of comments per user? What metrics would you look at?"
toc: true
toc_label: "Case"
toc_icon: "code"
---

Let's pretend that you are working at a Social Media Company and your boss askes you: "The average number of comments per user has decreased over the last three months. How would you point out some reasons for this and what metrics would you look into?"

I enjoy solving this kind of case-based questions because they let your own flavor shine.

What's your strategy? Here's mine, have fun! :)

# Question
 ~*"Let's say you work in a Social Media Company that has just done a launch in a new city.
   Looking at weekly metrics, you see a slow decrease in the average number of comments per user from January to March in the city.
   The company has been consistently growing new users in the city from January to March.
   What are some reasons on why the average number of comments per user would be decreasing?
   And what metrics would you look into?"*

# 1. Summarize the question
I'm working in a Social Media Company that has just done a launch in a new city where, from January to March, we have been experiencing a slow decrease in the average number of comments per user.
At the same time, the number of new users in the city has been consistently growing.
We want to point out some reasons for why the average number of comments per user is decreasing, as well as the metrics to look at.
Is that correct?

 ~*"Yes, it is correct."*

# 2. Verify the objective(s)
Now, I would like to know what constitutes success for the company: we want to discover some reasons for the slow decrease in the average number of comments per user, yet we might also want, for example, the average number of comments per user to increase? If yes, to what value?

Besides this objective, are there any other objectives I should be aware of?

 ~*"The average number of comments per user fell from 3 in January down to 2 in March, and it was 2.5 in February.  We aim at making it increase back to 3. Also, please be aware that we aim at expanding into the new city and saturate it as quickly as possible, capturing the greatest market share while keeping users engaged."*

# 3. Ask clarifying questions
So the average number of comments per user fall 33% from January to March, and I assume that we have done the launch in
the new city starting from January, is that correct?

 ~*"Yes, it is correct."*

I would like to know the total number of users from January to March.

 ~Here's a summary for you.

 | month | comments per user |
 |-------|-------------------|
 | Jan   | 10,000            |
 | Feb   | 20,000            |
 | Mar   | 30,000            |

So this means that the total number of comments was 30,000, 50,000, and 60,000 in January, February, and March, respectively.
Okay, now we can confirm that the decrease in the average number of comments per user is not an effect of a declining user base.

One more question: is this city close, or similar, to one or more cities where we have already done a launch in? I would like to compare the above metrics between the cities.

 ~*"No, the only data we have is about the users in the new city we have just done a launch in."*

# 4. Lay out the structure
I would like to take some time to **come up with a structure of possible reasons, which are mutually exclusive and collectively exhaustive** (MECE). Can I take a few minutes to think about it?

 ~*"Please, take your time."*

Here are my MECE alternative scenarios **according to two variables, namely users engagement and platform performance**:

* (A) **users are engaged and platform is performing**: the average number of comments per user is decreasing due to the great increase of total users - simply because the average number of comments per user in January is not representative of the average number of comments per user  of the entire market
* (B) **users are NOT engaged and platform is performing**: the average number of comments per user is decreasing because users are less and less engaged with the content they find in the platform - which is bad and requires immediate actions from Marketing
* (C) **users are engaged and platform is NOT performing**: the average number of comments per user is decreasing because of some technical issues or recent change within the platform, in particular when users try to post comments - which is bad and requires immediate actions from IT.

 ~*"Good, and what metrics would you look into to verify them?"*

I would **look into the following metrics**, by scenario:

* (A) business growth metrics: average number of comments per user
* (B) user engagement metrics: churn rate; average number of comments per active (retained) users; average time spent on platform per active (retained) user
* (C) IT-related metrics: recent change in the comments section; average number of crashes per users; average time for page load (after commenting)

 ~*"Okay. How would you move forward in your analysis?"*

# 5. State your hypothesis (and solution)
Now I would like to state my hypothesis in order to support...

## alternative A
 ... We have experiencing a slow decrease in the average number of comments per users as a natural consequence of the huge (and expected) increase of the total number of users.
 I am going to assume that all users are active and the platform is performing well.
 The reason for a decrease in the average number of comments per user is simply the fact that users who joined in January are not representative of the behavior of all potential users, and are different from users who joined later on (in February and in March). I assume that users who joined in January are different than users who joined in March, in terms of engagement, so I can check whether there has been a any changes in the average number of comments per user considering on one side the new users who joined in January (at the beginning of the period under analysis) and on the other side the new users who joined in March (at the end of the period under analysis).

 To this purpose, I would suggest two analytical solutions:
 1. the first one is simpler, quicker and less statistically significant, namely just **plotting and looking at the average number of comments per users *by week* of new users in January and of new users in March**, and visually verify if these two lines behave differently;
 2. the second one requires more time and more effort although it is more robust from a statistical viewpoint, namely **running a Hypothesis Test (\*) to compare the average number of comments of a group of new users who joined in March with respect to he average number of comments of a group of new users who joined in January**, and statistically verify if they are indeed different.

### Conclusion & Recommendations
 In either cases, I expect to find significance difference between the average number of comments per user in January and in March. Therefore, there would be no action to take to make the average number of comments per user increase back to 3 - this slow decrease is just a normal (and expected) adjustment as the user base is increasing.

 Also, given the objective of keeping users engaged, I would recommend to focus on user engagement (i.e. looking at  metrics like average time spent on platform per user) to prevent it from decreasing.

## alternative B
 ... We have experiencing a slow decrease in the average number of comments per users due to a constant decrease in user engagement despite an increase of the total number of users.
 I am going to assume that all users are less and less engaged with the content they find on the platform, although the platform itself is performing well.
 I assume that churn rate is behaving something like 25%, 20%, 15% on month 1, month 2, month 3, respectively.
 Accordingly, now we can evaluate the number of comments per active (i.e. retained) user and see how it changed from January (at the beginning of the period under analysis) to March (at the end of the period under analysis).

 To this purpose, I would suggest to evaluate the average number of comments per active user, by month, as follows:

 <img src="https://render.githubusercontent.com/render/math?math=average.number.of.comments.per.active.user=\frac{total.number.of.comments}{total.number.of.active.users}">

 where the *total number of active users*, by month, is equal to:

 <img src="https://render.githubusercontent.com/render/math?math=total.number.of.active.users = total.number.of.users \times (1 - churn.rate)">

 Let's run the numbers, by month:
 - Jan: 10,000 active users for 30,000 comments --> 3 comments per active user
 - Feb: \[10,000 * (1 - 25%)\] + 10,000 = 17,500 active users for 50,000 comments --> 2.85 comments per active user
 - Mar: \[10,000 * (1 - 25%) * (1 - 20%)\] + \[10,000 * (1 - 25%)\] + 10,000 = 23,500 active users for 60,000 comments --> 2.55 comments per active user

### Conclusion & Recommendation
 Given the churned rates, as assumed above, we have verified that the average number of comments per active user is decreasing.
 Also, I would like to verify whether the average time spent on platform per **active** user has decreased too, during the same period of time. Overall, I would recommend to further analyze the reasons of churn, for example by collecting feedback from churned customers and act accordingly - hopefully, this will make the average number of comments per user grow back to 3.

## alternative C
 ... We have been experiencing a slow decrease in the average number of comments per users due to problems with the platform: for example, it is not responding well to the increase of the total number of users or there has been a recent change in the platform (like a new location for the comments section).
 I am going to assume that all users are engaged with the platform but they experience problems when attempting to post comments because of technical issues with the platform.

 To this purpose, I can look at the aggregated metrics of users activities, such as the weekly number of crashes or the average time for page load after commenting by week. Simply plotting these two aggregate metrics by week would give us a visual proof for this scenario.

 Also, I would investigate whether there has been a recent change in the comments section of the platform, for example it has been moved far away from its original position and users are now wondering where to find it.

### Conclusion & Recommendation
 Given the platform inability to promptly react to the increase in user base or its recent changes as per the comments section, I would recommend to take immediate actions to reduce crashes, for example making use of more reactive and performing servers, and/or to let user familiarize with the new location of the comments section.
 This counteraction would foster the company expansion into the new city and ensure user engagement, making the average number of comments per user grow back to 3.


## (\*) How would you run the hypothesis test?

Let's say that the average number of comments of a group of new users who joined in January is known and we believe it represents the population of all potential users from the city: 3 comment per user; whereas the average number of comments of a sample group of new users who joined in March is μ, which we believe to be lower than 3.

The goal of the test is to see if μ is lower than 3, so the test needs to be set as follows:
- the Null Hypothesis (H0), which is the hypothesis we want to reject, is μ = 3
- the Alternative Hypothesis (H1), which is our claim, is μ < 3

We will run this test with the t-student statistics because the population standard deviation is unknown:

<img src="https://render.githubusercontent.com/render/math?math=t=\frac{\big(\bar{X}-\mu\big)}{s/\sqrt{n}}">

where Xbar and s are the sample mean and sample standard deviation, respectively.

Recalling the conditions for using a t-model:
1. there is no reason to think the average number of comments per user is normally distributed (they might be, but this
isn’t something we could know for sure), so the sample has to be large (more than 30); and
2. the sample has to be random, so we pick new users who joined in March by randomly selecting users among different days, times, locations, devices, sex, etc.

Let's say that we have a sample size of 1,000 new users who joined in March and that their average number of comments per user is 2 with a sample standard deviation of 1.

The question is: **assuming that the average number of comments per user is indeed equal to 3 (January's metric), what is the probability that 1,000 users who joined in March will have an average number of comments of 2 or less?**

Let's run the test statistic:

<img src="https://render.githubusercontent.com/render/math?math=t=\frac{\big(2-3\big)}{1/\sqrt{1000}}=-31.6">

The sample mean for our random sample is approximately 31.6 standard errors below the overall mean of 3. We know from previous experience that a sample mean this far below µ is very unlikely. With a t-score this low, the P-value is very small (say much lower than 0.001). The P-value helps us determine if the difference we see between the data and the hypothesized value of µ is statistically significant or due to chance.

Finally, we will set a significance level (α), say 95%, and **we see that P-value < α**. The only possible conclusion is to reject the null hypothesis in favor of the alternative hypothesis: **there is statistically significant evidence to state that the average number of comments per user for new users who joined in March is lower than the average number of comments per users for new users in January**.

# Conclusions

I hope you found this post useful - I would love to hear from you what you think about my solution.

Did you manage to find alternative scenarios or different metrics to look at? :)

~Giuseppe
