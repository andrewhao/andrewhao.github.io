---
layout: post
title: "Can Neural Networks Make Me a Better Parent? A tale in three parts"
categories:
- Machine Learning
- TensorFlow
- CNN
- Convolutional Neural Network
- Deep Learning
- Baby Monitor
- Personas
---

<h2 class="intro">
Over a year ago, I wrote a <a href="{% post_url 2018-03-21-tensorflow-for-tears-part-1 %}">couple</a> of <a href="{% post_url 2018-08-27-tensorflow-for-tears-part-2 %}">blog posts</a>) about how I trained a TensorFlow model on sound recordings of my crying baby (now infant) so I could plot the cry data as a time-series graph to glean some "insights" (air quotes mine).
</h2>

<h2 class="intro">
I distilled this experience into a talk called "Can Neural Networks Make Me a Better Parent?" and it reflects some extra maturity in my machine learning knowledge, as well as some hard-won parenting wisdom and experience.
</h2>

<script async class="speakerdeck-embed" data-id="01b04271a7fc42a996799435411f660b" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

Nighttime at our house is pretty hectic and crazy, and we have sleep battles with our toddler all the time. The question was, just _how much_ were we locking horns? I decided to train a machine learning model to find out.

I discussed how I did this in [Part I]({% post_url 2018-03-21-tensorflow-for-tears-part-1 %}) and [Part II]({% post_url 2018-08-27-tensorflow-for-tears-part-2 %}) of my talk. In the slides, I go over the data pipeline I used to tag and label the data, which if you ask me is pretty interested. One interesting tool (I won't talk about it here) is [EchoML](https://github.com/ritazh/EchoML), which allows you to tag and label different parts of audio files for utterances.

The interesting stuff is what happened after I was able to get the system set up and doing real-time audio detection. Can this dad find some sleep training insights, or will he be forever doomed to tears?

## Well what happened?

![Screen Shot 2019-10-03 at 8.03.27 AM.png](/images/0e12d38b53aa40958f292de34d8071b2)

_Here's some real-time sleep data_

![Screen Shot 2019-10-03 at 8.08.31 AM.png](/images/6bce0facbf6e44ffa8726db7992c6aa5)

_Cry patterns, measured by minutes per day_

At the end of my time, I realized that quantifying sleep progress was actually kind of depressing, especially when you see those numbers continue to jump as the months go by.

I loaded up the data in a Jupyter Notebook (link) and tried to slice the data for some insights.

![Screen Shot 2019-10-03 at 11.43.05 AM.png](/images/cab024dab6e2433ea0d236c4fd355a72)

_Note how beyond actual "loudness", the only variables that correspond with cry patterns are month and day of week. We cannot truly depend on month, since I only had one year of data._

The only interesting finding was that there was a 1% correlation of crying with the day of the week. However, that's small enough to be within the margin of error for analysis. It could be interesting when subjected to further analysis.

![Screen Shot 2019-10-03 at 11.43.14 AM.png](/images/bfe15b1ae5874715a55ba697e35934d0)

_My attempt at a correlation heatmap. Nothing interesting here_

![Screen Shot 2019-10-03 at 11.44.25 AM.png](/images/2e4e92f1094e4843a28e6de5254ec4fc)

_Looking for correlations between temperature and humidity and crying - once again, nothing interesting._

At the end of my analysis, I realized that quantifying sleep progress was actually kind of depressing, especially when you see those numbers continue to jump up and down month after month.

So was this project useless? Was I back at square one?

![baby-monitor.gif](/images/ca4d805b811b4b6086e58466e0f41d14)

## The Illusion of Insight

One night Annie and I were staring at the baby monitor during another cryfest and we were up to our eyeballs in frustration. She looked over at me and said "I'm going in"

"What?" I replied, incredulously. "You know we can't do that. We'll just reward his crying and his sleep training will be ruined!"

But she went up anyways. It was silent for 20 minutes and all I could hear was the white noise machine.

She came back downstairs, sat down next to me and all she could say was "kids are weird".

No kidding. "What happened?" I asked.

"I sat down on the floor next to his bed and we just looked at each other. Then I grabbed a blanket and lay down on the floor and pretty soon he lay down too and then he fell asleep."

"He just wants to be with us, Andrew"

At that moment I had this hot flash of shame. Had I missed something vitally important as the fact that _my son needed us, but I had been so fixated on my way of doing sleep training that I had been holding back comfort and connection?_

Of course, it didn't always work. And yes, "going in" really does reinforce bad sleep habits. But sometimes, that's just what you do when you need to give your kid (and everyone else in the house) a break.

## So now what?

I'm thankful and glad I embarked on this project. It's taught me a lot about deep learning, and the process of understanding the internals of TensorFlow models. It's also shown me how to optimize model training, as well as the importance of labeling data thoroughly and accurately.

However, these days I don't watch the baby monitor nearly as closely as I used to. I don't really care for the minute-by-minute information because I understand its limitation.

## A parable for the real world - understanding the humans that work alongside our models

As I was preparing my talk, I realized that there was an important connection to real-world machine learning.

In the same way that I dismissed my son's human need for connection for the sake of building my model off of my idea of inputs and data, do we, as ML and engineering practitioners miss seeing the human needs of the people adjacent to our ML models?

_How do we go from here..._
![Screen Shot 2019-10-06 at 1.58.57 PM.png](/images/245e3a9c964244efbb9666bdff7bf741)

There are humans whose inputs are valuable to our models. If we base our models only off of only what we measure through quantifiable datasets and pipelines, we run the risk of building systems that fail to serve (or worse - harm) the people they are designed to help.

_...to here?_
![Screen Shot 2019-10-06 at 1.58.46 PM.png](/images/bc717ded994241478794ca962fc6fceb)

How can we build a consciousness of our customers and the humans that work within? I have a silly idea, but I think it might be a good one: **Personas**. Let me expand on that idea in a future post...

## Appendix

* [github.com/andrewhao/babblefish](https://www.github.com/andrewhao/babblefish) - the source code of the baby monitor
* [github.com/tensorflow/tensorflow](https://www.github.com/andrewhao/babblefish) - TensorFlow source code (newly released with 2.0)