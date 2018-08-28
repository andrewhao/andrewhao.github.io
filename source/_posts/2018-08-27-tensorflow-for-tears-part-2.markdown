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

   $ sox FILENAME FILENAME_OUTPUT trim 0 5 vol 45 dB rate 22050

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
