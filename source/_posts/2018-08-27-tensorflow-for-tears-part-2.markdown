---
layout: post
title: "Tensorflow for Tears: Part 2"
date: 2018-08-27 20:44:34 -0700
comments: true
categories:
  - TensorFlow
  - Machine Learning
  - Parenthood
---

The [last time we met]({% post_url 2018-03-21-tensorflow-for-tears-part-1 %}), we had a lively discussion about the ins and outs and joys and terrors of parenting. I talked about how I started building a Raspberry Pi project with a USB mic and wrote a simple parsing script that measured the mean amplitude of recordings of the current state of the nursery. And when that guy wailed, he really WAILED.

Well that naive approach got us so far, but the system would still trip up due to random loud noises in the house. Music, or doors closing or opening, or loud conversation would all cause the system to think the kid was crying but no - it was just ambient noise.

### Getting started with TensorFlow

Let's start our dive into machine learning!

I had already gotten pretty far with [Udacity's Machine Learning](https://www.udacity.com/course/intro-to-machine-learning--ud120) course and had vague recollections of AI theory floating around from the cobwebs of undergrad CS courses past. So I had _some_ background in AI and machine learning. But training neural networks was a completely new thing to me.

Luckily, I came across the [Simple Audio Recognition Tutorial](https://www.tensorflow.org/tutorials/sequences/audio_recognition) example right there on the TF homepage. This was exactly what I was looking for. The objective was: given a training set of audio clips of crying babies and "empty rooms", classify an audio clip as one or the other.

### First: getting the dataset

I had oversimplified in my mind what made a great training data set. After all, I figured that I just had to record my kid crying a bunch, then get a few minutes of quiet room sounds, and then we were good, right?

Wrong.

Training data must be exactly matched. Sample rates must be consistent, and audio samples must either be highly randomized or at least normalized to the same rates. To wit, here's how I gathered my sample data:

1. I set up the Raspberry Pi to continuously record 30-second samples at 22.05kHz through the onboard microphone.
2. When my son started a crying episode, I'd make note of the start time and end times, and then go back and gather those clips of the cries and dump them in the `crying` folder.
3. Once I had collected enough of those (maybe 30 minutes total), I then gathered a bunch of the "other" clips of quiet sleep in his nursery and threw these in the `silence` folder.
4. Once I had moved these samples off the Raspberry Pi and onto my laptop, I then sliced these recordings into 5-second clips using `sox`. I also applied a few amplification filters to overcome the weak pickup on the mic.

`$ sox FILENAME FILENAME_OUTPUT trim 0 5 vol 45 dB rate 22050`

I then placed each of these trained samples in the folders corresponding to their label: `crying` and `silence`.

### Second: training the network

I modified the `train.py` script nearly verbatim from TF docs. We'll dissect it here, beginning with the command to begin training:

```bash
    $ python app/train.py --data_url= --data_dir=./data --wanted_words=silence,crying --sample_rate=22050 --clip_duration_ms=5000 --how_many_training_steps=1000,200 --learning_rate=0.001,0.0001 --train_dir=./training
```

- `--wanted_words=silence,crying`: This specifies which labels should be considered for training purposes.
- `--sample_rate`: The sample rate of the audio files provided
- `--clip_duration_ms`: Duration of each training clip in milliseconds
- `--how_many_training_steps`: This is a comma-separated list of n numbers that specify the number of steps per phase.
- `--learning_rate`: This is the rate at which the system can adjust its current learnings to match its new inputs. A higher learning rate means the faster the system can change to learn new inputs. A lower number ensures the stability of a system's learning. We specify a higher training rate for the first phase and lower the training rate in the latter phase as our precision increases.

Let's run the script!

```bash
$ python app/train.py --data_url= --data_dir=./data --wanted_words=silence,crying --sample_rate=22050 --clip_duration_ms=5000 --how_many_training_steps=1000,200 --train_dir=./training
2018-08-27 22:15:33.195894: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
Tensor("Placeholder:0", shape=(), dtype=string)
INFO:tensorflow:Training from step: 1
INFO:tensorflow:Step #1: rate 0.001000, accuracy 26.0%, cross entropy 2.674081
INFO:tensorflow:Step #2: rate 0.001000, accuracy 23.0%, cross entropy 1.593786
INFO:tensorflow:Step #3: rate 0.001000, accuracy 64.0%, cross entropy 1.067298
INFO:tensorflow:Step #4: rate 0.001000, accuracy 73.0%, cross entropy 0.843605
...
```

### Third: parsing the results

The output in each step reveals the current state of the neural network as trained. _Accuracy_ reflects the correctness of the model as the validation set is tested against the network (during each step of training, samples in the validation set are run against the model and checked if they line up with their specified labels).

[_Cross entropy_](https://www.tensorflow.org/api_docs/python/tf/losses/softmax_cross_entropy) is, as I understand it, the [squared error factor](https://deepnotes.io/softmax-crossentropy) in the result network from the actual results (lower is better).

This would proceed for several hours for 1200 total steps. On a 2013-era Macbook Pro, this took approximately 6 hours. I had 350 clips of crying and 500 clips of silence. (Too much or not enough? This tired parent says "too much".)

One more thing - every few hundred steps during training, we would get this sort of output:

```
INFO:tensorflow:Confusion Matrix:
 [[ 9  0  0  0]
 [ 0  0  0  0]
 [ 0  0 55  0]
 [ 0  0  1 30]]
```

What's a [confusion matrix](https://www.tensorflow.org/api_docs/python/tf/confusion_matrix)? It's, according to [this helpful article](https://www.dataschool.io/simple-guide-to-confusion-matrix-terminology/), another way to visualize the accuracy of a machine learning model.

Here's how to read this confusion matrix. Given the following labels:

```
# conv_labels.txt
_silence_
_unknown_
silence
crying
```

_(Where did these come from? More on that later...)_

Imagine these labels go left-to-right, and top-to-bottom. The x-axis represents the labels that have been predicted (i.e. that have been verified) to be a certain label. So the first column represents the percentages of samples that have been predicted to be `_silence_`.

The labels that go top-to-bottom are the actual results from the trained model. So in this case, if we take the top-left number `9`, that means that in 9 runs of the model where the prediction was `_silence_`, the actual result was `_silence_`. Moving one cell down, that represents the # of samples where the predicted result was `_silence_`, but the actual result was `_unknown_`. Fortunately for our model, there are `0` results in this cell. So on and so forth. So the ideal confusion matrix is a matrix that has a "diagonal line" running from top-left to bottom-right, and `0`s everywhere else, because all predictions would equal actuals.

tl;dr: Confusion matrices are a way to visualize and report the accuracy of a machine learning model. You want a clear and convincing diagonal line in the matrix.

Oh, and here are the results from the training data. I'm using `tensorboard` to visualize the training steps:

![Accuracy data](/images/tensorflow-for-tears/tensorboard-accuracy.png)
_Accuracy modeling. Note how quickly the model jumps to be fairly accurate._

![Cross entropy data](/images/tensorflow-for-tears/tensorboard-cross-entropy.png)
_Note how quickly cross entropy dives._

We can use these graphs to tune our models if we really cared. In this case, I say it's good enough (accuracy is up to 99% by the end).

### Fourth: Running the model

OK, but enough already. We have a trained model and, like [Chekhov's Gun](https://en.wikipedia.org/wiki/Chekhov%27s_gun), that means we've gotta use it!

Where's that model? Oh, it's waiting for us in ``.