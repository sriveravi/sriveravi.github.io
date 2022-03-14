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

## Problem

Why are some people so good at drawing followers and gaining influence from their social media posts? I'm notoriously terrible at it, with under 100 twitter followers after 10 years on the platform. In the days of myspace it wouldn't matter, but today a strong following means career opportunities and other potential revenue streams. Social media reach is powerful. Fortunately, I can apply data science tools to better understand what makes some people so good at building an audience. This post summarizes a project I did this week to mine the secret sauce from @dog_feelings. 

## Plan Overview
The overall approach to understanding how to make tweets go viral was straightforward. The key idea was to build a model that understands how to separate the spectacular tweets (dog tweets) from mediocre tweets (cat tweets). Then it was just a matter of model explanation to understand  what separated the best from the rest. 

More concretely, the steps were as follows:
1. Build a twitter bot to download the latest tweets from a few accounts. 
2. Use pre-trained language models to encode the tweets as numerical values.
3. Build a classifier with those features.
4. Use Shapley value analysis to determine what makes the tweets distinct.  

Let's jump into it.

### Twitter Bots

If we want to gain insight from some text data, then the first step is to obtain that data. Fortunately, you can use the tweepy library to access the twitter API with python. Once you make a developer account and register a bot, it will give you some authentication keys for the API v1.1 authentication. The newer v2 API has some additional project constraints that I didn't want to get into for something this simple. I pointed my bot to @dog_feelings as my main class of interest and @feline_feelings as the contrasting class. This superficial class split is sufficient for this proof of concept (POC), but would be tweaked if I was doing this type of analysis in the real world.

### Natural Language Processing (NLP)
After obtaining a few tweets (100 per class), I used pre-trained language models to convert them to numerical sequences. Specifically, I used a pretrained [BART](https://huggingface.co/facebook/bart-large) model from huggingface. 

I obtained the features passing a full tweet through the network, pulling out the final hidden layer outputs, then averaging the sequence. Averaging is necessary for making all tweet feature vectors the same size. This enabled me to pass them to a classifier model.

Some alternatives would be zero padding shorter sequences or cropping them down to the same size. Although averaging removes some of the sequence information, the BART model inherently preserves context and sequence information so I thought this was a perfectly reasonable place to start. If the classifier performed poorly, this is one of the places I would revisit to add a bit more care to preserve the original tweet content. 

#### Side Note on NLP Models
BART is transformer based natural language processing (NLP) model that generalizes well to many tasks such as language generation, translation, comprehension, and so on. It builds on the seminal [Transformers](https://arxiv.org/abs/1706.03762) architecture (but see the more recent [BERT](https://arxiv.org/abs/1810.04805)) that moved the NLP community away from recurrent neural networks (RNN) to massively parallelizable attention-based architectures. Basically, They got rid of recurrent network connections that scaled up poorly and replaced them with inner products across the sequence. There are some extra tricks like position encodings to preserve the sequence information and skip connection ala ResNet to train deeper layers. This allowed training on much larger datasets which transformed modern NLP. Now most if not all of the modern NLP architectures employ some transformer variant as of the time I'm writing this in 2022.

Another great thing about these architectures is that they use pre-training tasks that do not require labels. The tasks are usually something like filling in the missing words or predicting the next word in a sentence. Think "self-supervised learning". That means that they can just rip through digitized text all day without any complicated or expensive data labeling. After they are trained up, they they are usually fine-tuned on a small labeled dataset for a specific task. That means you get all the internal language model building from billions of sentence examples, but transferred to your specific task with just a few hundred training examples. Modern machine learning is great. 

 
- BART/BERT/GPT  (https://huggingface.co/facebook/bart-large-mnli)

## Classifier
- linear classifiers are KING

## Shapley analysis


## Limitations
- very small data
- weird RT's that have the dog label in it
- Ignoreing a variety of other metadata, frequency of tweets, time of day, etc.
- Proxy problem:
    - may get a whole different result if it was separating dog from other account
    - may want to model something like CTR directly




