---
title: "Tweet analysis with twitter bots, NLP and game theory"
layout: post
post-image: /assets/images/twitter-640.png
description: Explanatory twitter text analysis
tags:
   - machine learning
   - social media
   - nlp
   - python
---


# Twitter NLP and analysis
Go straight to the [code](https://github.com/sriveravi/tweet-nlp-interpreter).

## Problem

Why are some people so good at drawing followers and gaining influence from their social media posts? I'm notoriously terrible at it, with under 100 twitter followers after 10 years on the platform. In the days of myspace it wouldn't matter, but today a strong following means career opportunities and other potential revenue streams. Social media reach is powerful. Fortunately, I can apply data science tools to better understand what makes some people so good at building an audience. This post summarizes a project I did this week to mine the secret sauce from @dog_feelings. 

## Plan Overview
The overall approach to understanding how to make tweets go viral was straightforward. The key idea was to build a model that understood how to separate the spectacular tweets (dog tweets) from mediocre tweets (cat tweets). Then it was just a matter of model explanation to understand  what separated the best from the rest. 

More concretely, the steps were as follows:
1. Build a twitter bot to download the latest tweets from a few accounts. 
2. Use pre-trained language models to encode the tweets as numerical values.
3. Build a classifier with those features.
4. Use Shapley value analysis to determine what makes the tweets distinct.  

Let's jump into it.

### Twitter Bots

If we want to gain insight from some text data, then the first step is to obtain that data. Fortunately, you can use the [tweepy](https://github.com/tweepy/tweepy) library to access the twitter API with python. Once you make a developer account and register a bot, it will give you some authentication keys for the API v1.1 authentication. The newer v2 API has some additional project constraints that I didn't want to get into for something this simple. I pointed my bot to @dog_feelings as my main class of interest and @feline_feelings as the contrasting class. This superficial class split is sufficient for this proof of concept (POC), but would be tweaked if I was doing this type of analysis in the real world.

### Natural Language Processing (NLP)
After obtaining a few tweets (100 per class), I used pre-trained language models to convert them to numerical sequences. Specifically, I used a pre-trained [BART](https://huggingface.co/facebook/bart-large) model from huggingface. 

I obtained the features by passing a full tweet through the network, pulling out the final hidden layer outputs, then averaging the sequence. Averaging was necessary for making all tweet feature vectors the same size. This enabled me to pass them to a classifier model.

Some alternatives to averaging are zero padding shorter sequences or cropping them down to the same size. Although averaging removes some of the sequence information, the BART model inherently preserves context and sequence information so I thought this was a perfectly reasonable place to start. If the classifier performed poorly, this is one of the places I would revisit to add a bit more care to preserve the original tweet content. 

#### Side Note on NLP Models
BART is a transformer based natural language processing (NLP) model that generalizes well to many tasks such as language generation, translation, comprehension, and so on. It builds on the seminal [Transformers](https://arxiv.org/abs/1706.03762) architecture (but see the more recent [BERT](https://arxiv.org/abs/1810.04805)) that moved the NLP community away from recurrent neural networks (RNN) to massively parallelizable attention-based architectures. Basically, They got rid of recurrent network connections that scaled up poorly and replaced them with inner products across the sequence. There are some extra tricks like position encodings to preserve the sequence information and skip connection ala ResNet to train deeper layers. This allowed training on much larger datasets which transformed modern NLP. Now most if not all of the modern NLP architectures employ some transformer variant as of the time I'm writing this in 2022.

Another great thing about these architectures is that they use pre-training tasks that do not require labels. The tasks are usually something like filling in the missing words or predicting the next word in a sentence. Think "self-supervised learning." That means that they can just rip through digitized text all day without any complicated or expensive data labeling. After they are trained up, they are usually fine-tuned on a small labeled dataset for a specific task. That means you get all the internal language model building from billions of sentence examples, but transferred to your specific task with just a few hundred training examples. Modern machine learning is great. 

### Classifier
After converting my tweets to numerical feature form, the next step was to learn how to separate the awesome ones from the bad ones. While it's almost too easy to build a very complicated deep architecture with the libraries available today, I typically start with the simplest model possible. That's for two reasons:

1. Easier to debug: When you start with a big model, it's sometimes unclear if you aren't learning due to some bug in your architecture, issues with your data loading, or some other issue.
2. Rapid iterations: You can get a very fast and reproducible result with the convex objective function of linear regression.

Therefore, I started with a logistic regression model. Surprisingly, I got perfect accuracy on the first try. This meant that the model understood how to separate the good from bad tweets. 

The big assumption here is that there are not other confounding factors that would separate the tweets in this particular dataset. It could be something as silly as tweet length. These data may not be representative of good and bad tweets. This is where machine learning engineers need to be careful about claims they make. Data carry inherent biases and we must be critical of our models, data, and any insights we extrapolate from those models. That's enough philosophy for now. 

#### Side Note on First Try Nonsense
Clearly, I didn't get a perfect classification on the first try. I ran into this super tricky python bug where I was constantly getting 50% accuracy. Somehow, if you initialize a list of empty lists then try to append to one of them, it appends to both! Thanks to the step debugger for helping me with this one. Try it out, and please send me a tweet explaining why this is the case if you know. I get references, but shouldn't they be different empty lists in the initialization? 

~~~python
x = [[]]*2
x[0].append(1)
print(x)
# [[1], [1]]
~~~

### Shapley analysis
Finally, it was time to understand what made the dog tweets tick. Things got a bit complicated here because I used deep net architecture for the feature extraction. When using something like a simple linear model, you can tell the relative importance of features by looking directly at the magnitude of the feature coefficients. I used tricks like that when I was building [cognitive models](https://arxiv.org/abs/2010.15047). Although I did use a simple classifier model, the features used in the classifier could not be directly tied back to a single word of the input sequence after the transformer computations. This is a common drawback of deep networks. They can be difficult to explain and interpret.

Fortunately, techniques exist that allow you to relate every feature of your input to the impact it has on the classification outcome. Specifically, I used shapley value analysis to analyze my tweets. The [shap](https://github.com/slundberg/shap) package has nice interactive jupyter notebook tools to visualize and interact with the model inputs. Here's an example of the output:

![Dog tweet shap output](/assets/images/dogTweetShap.png)

You can in the picture that the words "route", "walk", and "exciting" were particularly influential.

## Conclusions and Limitations

This was meant is a POC and there are several important limitations including those I alluded to above. If I were to extend this work, I would address:
1. Small data size: 100 is a _tiny_ sample by today's standards.
2. Limited user sets: focusing on these users injects bias to our models. I mean bias in the social sense and not the estimation theory sense.
3. Data cleanup and feature engineering: I'm neglecting a great deal of valuable metadata like tweet length, frequency, user interactions and time, to name a few.

Thanks for making it this far! We went over quite a bit about building bots for pulling data, leveraging pre-trained NLP models, and finally interpreting the model results. While the technologies involved were quite sophisticated, execution was relatively simple because of the great tools and libraries that were developed by outstanding researchers, ML practitioners, and software engineers. My thanks to those doing great work for the open source community.

Check out the source [code](https://github.com/sriveravi/tweet-nlp-interpreter).