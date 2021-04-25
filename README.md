# Audio-Background-Noise-Suppression

Real applications have to recognize audio even when there are other irrelevant sounds happening in the environment.  The audio files from the call center are full of background noise, to be able to convert call audio from call center to transcription accurately, it is important to remove or reduce the background noise from the audio.  

As in the initial stage of the experiment, we will focus on a simple algorithm to understand and explore the next steps.  

## Types of Noise
Stationary or static noise is typical when there is a low volume background sound present in an audio signal. This noise is typically consistent across the entirety of a piece of media because the source of the sound doesn’t change.

There are a number of things that could cause a stationary sound:

* the audio equipment itself, such as a microphone hiss or electric hum of a power-line frequency
* a computer fan running in the background
* a heating or air conditioning unit circulating air

Detecting this type of noise reduction can be done through an analysis of the sound characteristics such as variations in frequencies over time for any hums, buzzes, and other white noise that distracts from the high-value content. There are different techniques for this type of digital signal processing to remove or subtract this type of noise.

Non-Stationary or non-static noise comes from less common sounds that appear infrequently or at a cyclical time.

The examples of non-stationary sounds is considerably more varied:

* a dog barking
* birds chirping
* keyboard clicking
* an ambulance driving by
* book falling off a desk

These sounds are unwanted, but not easy to detect from the sound profile itself. Another way to think of it is the inverse of how to look at stationary sounds. Instead of detecting noise, we use machine learning algorithms that know how to elevate the sounds desired through speech isolation of spoken words in certain types of media.

Source: https://dolby.io/blog/enhance-audio-by-removing-stationary-background-noise

The spectrum of an audio tells you what frequencies are in it. To remove noise, we need to know  "where it is", and "what kind of noise" it is. As a naive approach, I will try to remove high frequencies, ie replace the last values of the fft by zero then do a ifft, since it's often where there is more noise than data.



## 1. Preparation of Dataset
Main dataset are produced from Cantonese Youtube videos that are downloaded and converted to WAV files.  

###  1. Add random segment of environmental audio to the audio
A folder of background noise WAV file from [Microsoft Scalable Noisy Speech Dataset (MS-SNSD)](https://github.com/microsoft/MS-SNSD) with white noise and recordings of machinery and everyday household activities are used in this analysis to add noises on the cantonese audio. The loudness of the background noise are used in 4 levels, with 10 the largest and 40 the smallest. 
Since the sample rate are different from the main dataset (16 kHz instead of 44.1 kHz), the audio signal for noise are upsampled to 44.1 kHz in order to combine the two audios. 

## 2. Noise Suppression Algorithm
There are many ways to remove the noise from a given audio recording. All it requires is a small sample where there is only a background noise, and then automatically delete this noise from the rest of the sample.

Try to increase volume to check the accuracy.


## 3. Evaluation
  ### 1. Google Speech-to-text


## 4. Elaborate the use case with Google cloud NLP API

## 5. Next Steps


Slow down the speed of audio to check if it can be transcribed more accurately. 
Denoise with Deep Learning models (Source: https://sthalles.github.io/practical-deep-learning-audio-denoising/)

Sound separation --> Deep Clustering 

To simplified the code. 



## Learnings and next step:
At the Data preparation period, I had faced the problem of having a the audio of female to male and the audio is being slow down. I realised that the problem is because the kHz of the noise audio and the main audio are different. I solved it by resample the noise audio to 44100 kHz.

The design of human's ear and a speech-to-text algorithm are very different. While humans can focus on the voice produced by a single speaker in a crowded and noisy environment, this is not the same case with machine. From our human ears, even we can clearly identified that the background noise are removed/ reduced and can clearly understand what are saying in the audio, from a machine perspective, it might become more difficult to identify words. 

It is clear that louder and slower audio can improve the transcription power. For the next step, we can try to adjust the rate of volume increase and slowdown to improve the transcription. 

The Google speech-to-text API recognizer ([Source](https://cloud.google.com/speech-to-text/docs/best-practices)) is designed to ignore background voices and noise without additional noise-canceling. However, excessive background noise and echoes may reduce accuracy, especially if a lossy codec is also used.

Use a more robust algorithm such as de-noising with a [deep learning model](https://sthalles.github.io/practical-deep-learning-audio-denoising/)



