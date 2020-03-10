---
title: "Suggest the best real-time fraud detection model and the metrics to optimize it for"
date: 2020-03-06
tags: [case, learning, interview]
header:
  image: "/images/real-time-fraud-detection-model-and-metrics-case.png"
  excerpt: "What model would you suggest for real-time fraud detection in a Payment Service Provider company? And which metrics would you optimize the model for?"
toc: true
toc_label: "Case"
toc_icon: "code"
---

Let's pretend that you work in a Payment Service Provider company and you are required to build a model to detect frauds on its platform. How would you choose what model would work the best, and which metrics would you optimize this model for?

I enjoy solving this kind of case-based questions because they let your own flavor shine.

What's your strategy? Here's mine, have fun! :)

# Question

 ~*"Let's say you work for a Payment Service Provider company that wants to build a model to detect frauds on its platform. What kind of model would you build? And what metrics would you optimize the model for?"*

## 1. Summarize the question

I'm working in a Payment Service Provider company that aims at developing a model to detect fraudulent transactions on its platform.
My task is, first, to suggest what model to build and, second, to point out the metrics to optimize such model for.
Is that correct?

 ~*"Yes, it is correct."*

## 2. Verify the objective

Now, I would like to know more about what really constitutes success for the company.

Besides building the model and choosing the right metrics to optimize for, are there any other objectives I should be aware of?

 ~*"The company also wants to implement a text messaging service that, immediately after the response of the model and in case it detects a fraudulent transaction, will text customers with a verification code to authorize the transaction."*

That's good to know, thank you! Given this other objective, I can confirm that:
 - it makes good business sense to build a model that maximizes detection of actual frauds, since we will text customers a verification code in all cases of potential fraud - and this will build trust and reputation
 - our model must be fast in the testing phase, so we will also optimize for such model performance

## 3. Ask clarifying questions

I need to ask you some other clarifying questions.

So far, I know that we want to build a model to catch frauds on our platform because they hurt our reputation and our brand. Given that we can never build a model that is able to catch 100% of actual frauds and, at the same time, is 100% sure about the transactions that it classifies as fraudulent being actual frauds, I would like to define together the extent to which we will be satisfied with our model.

Let's define precision and recall as follows:
 1. Precision represents how good our model is in correctly classifying transactions as frauds; it is the percentage of actual fraudulent transactions out of all transactions predicted as fraudulent transactions by our model.
 <img src="https://render.githubusercontent.com/render/math?math=Precision = \frac{True%20Positive}{True%20Positive%2BFalse%20Positive}">
 2. Recall represents how good our model is in detecting frauds out of all fraudulent transactions; it is the percentage of actual fraudulent transactions caught by our model.
 <img src="https://render.githubusercontent.com/render/math?math=Recall = \frac{True Positive}{True Positive %2B False Negative}">

Given the fact that we will text our customers a verification code for every transaction that our model classifies as
fraudulent, I would suggest to maximize recall rather than precision. To clarify this idea, given the above definitions, respectively, please tell me:
 1. out of 100 transactions that our model classifies as fraudulent, how many of them must be actual frauds?
 2. out of 100 fraudulent transactions, how many of them our model must detect?

Please note that these two metrics usually behave as a trade-off: the more precise (selective) our mode is, the less cases of real fraud it will detect, and vice versa.
If you cannot really tell me a number, please just tell me which of the two is more important than the other.

 ~*"Since we are more interested in detecting and fighting all actual fraudulent transactions, we want to catch as many real frauds as possible, with little burden about how many transactions our model might mis-classify as frauds, provided that it does not flag every transactions as frauds!"*

Thank you. Therefore, I can confirm that we are more inclined to optimize for recall, meaning we are more concerned about stopping all actual frauds - as long as our model precision does not deteriorate too much, otherwise customers would receive a message to verify each and every transaction they make.

Also, I am going to assume that the data we work with to build this model has a label that distinguish the genuine transactions from the fraudulent ones, is that correct?

 ~*"Yes, it is correct: our data is labelled."*

Last question: I am afraid that the transactions labelled as frauds represent a very small percentage of the entire dataset. Am I wrong?

 ~*"No, the fraudulent transactions are about 1% of total transactions in the dataset."*

## 4. Lay out the structure

Now I would like to take some time to **come up with a structure of potential models to build and metrics to optimize for**. Can I take a few minutes to think about it?

 ~*"Please, take your time."*

### What model to build

First, we are in need of a **binary classifier**: a model that is able to output either 1 (fraud) or 0 (non-fraud).

Here is **my list of potential models** that we might build:
 - Naïve Bayes
 - Logistic Regression
 - K-Nearest Neighbours
 - Support Vector Machine
 - Random Forest
 - Neural Network

Please note that these models are sorted in order. My list starts with models that are the simplest, the fastest, the ones requiring the fewest number of parameters, the ones that are more interpretable and at the same time giving probabilistic predictions.

### What metrics to optimize for

Second, as per which *metrics to optimize for*, I would suggest to look at the Area Under the Precision-Recall Curve
(AUPRC) together with F1-score. Indeed, the traditional Accuracy, namely (True Positive + True Negative) / (Positive + Negative), is not meaningful for unbalanced classification: it will be usually high and misleading.
In particular, F1-score is the harmonic mean of Precision and Recall while the AUPRC shows the trade-off between
Precision and Recall for different thresholds. A high AUPRC represents both high recall and high precision, which means that the classifier is returning many results that are accurate (high precision), as well as returning a majority of all positive results (high recall).

Also, I would choose a model that is fast in delivering the results, meaning within a reasonably low amount of time, because we need to text the customer in case of fraudulent transactions. This is often called Prediction Speed.

**Maximizing both the Recall and the Prediction Speed involves** solving a multi-objective optimization problem. In this type of problems, in general, there is no single optimal solution - we need to investigate the so-called "Pareto frontier" of different optimal values (Recall vs Prediction Speed).

## 5. State your hypothesis (and solution)

Now, I would like to state my hypothesis in order to support my solution.

I am going to assume that Naïve Bayes together with Logistic Regression are used only to provide a good benchmark to be used as a reference for more advanced models. Indeed, I am confident that all other models can perform above them.
I would run all of them and pick the model that provides the highest Recall at the fastest Prediction Speed.

# Bonus Question

 ~*"How would you handle unbalanced data?".*

Simply put, there are three alternative ways in which you can handle unbalanced data:

1. undersampling the majority class, for example so that it matches the size of the minority class;
2. oversampling the minority class, for example by means of SMOTE (Synthetic Minority Oversampling Technique);
3. penalize the model when predicting the majority class, for example by weighting the classes inversely proportional to their frequency.

# Conclusions

I hope you found this post useful - I would love to hear from you what you think about my strategy to solve this case.

What else can be included in my answer? :)

~Giuseppe
