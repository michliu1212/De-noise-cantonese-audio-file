# Audio-Background-Noise-Suppression

Noise is everywhere, no matter you are at your home or on the street. Espectially in the digital age, these noises get picked up by microphones and interfere our communication. The audio records from the call center are as well full of background noise. As to be able use the data effectively, it is important to be able to convert these audio records to text, where reducing background noise is the key. 

As in the initial stage of the experiment, we will focus on a simple algorithm to understand and explore the next steps.  

## Types of Noise

### a. Stationary noise
Stationary or static noise is typical when there is a low volume background sound present in an audio signal. This noise is typically consistent across the entirety of a piece of media because the source of the sound doesn’t change.

There are a number of things that could cause a stationary sound:

* the audio equipment itself, such as a microphone hiss or electric hum of a power-line frequency
* a computer fan running in the background
* a heating or air conditioning unit circulating air

Detecting this type of noise reduction can be done through an analysis of the sound characteristics such as variations in frequencies over time for any hums, buzzes, and other white noise that distracts from the high-value content.

### b. Non-Stationary noise
Non-Stationary or non-static noise comes from less common sounds that appear infrequently or at a cyclical time.

The examples of non-stationary sounds is considerably more varied:

* a dog barking
* human speaking
* keyboard clicking
* traffic
* shutting door 

These sounds are unwanted, but not easy to detect from the sound profile itself. Another way to think of it is the inverse of how to look at stationary sounds.

[Source](https://dolby.io/blog/enhance-audio-by-removing-stationary-background-noise)


## 0. Preparation of Dataset
Jupyter notebook: [00 Data Preparation.ipynb](https://github.com/michliu1212/Audio-Background-Noise-Suppression/blob/9509b04196b739ce753b9c91ae092727028034f6/00%20Data%20Preparation.ipynb)

Main dataset are produced from Cantonese Youtube videos that are downloaded and converted to WAV files.
Noise audio files are downloaded from [Microsoft Scalable Noisy Speech Dataset (MS-SNSD)](https://github.com/microsoft/MS-SNSD) where it consists of white noise, recordings of machinery and everyday household activities. 
* Noise audio are added into the cantonese audio. The level of background noise are added in 5 levels, with 0 the largest and 40 the smallest. 

## 1. Noise Suppression Algorithm & Evaluation
Jupyter notebook: [01 Noise Suppression MVP & Evaluation](https://github.com/michliu1212/Audio-Background-Noise-Suppression/blob/7cbfa3d7a83877cdd7ebb29be2383ff5ec33c873/01%20Noise%20Suppression%20MVP%20&%20Evaluation-for%20upload.ipynb)

[noisereduce library](https://timsainburg.com/noise-reduction-python.html) had been used to reduce the background noise. In the noisereduce library, it is required for both the noise signal and the signal audio clip. However, in the reality, we do not often have the noise signal, thus below we will used a technique called sound envelope to create our own noise signal clip.

### noisereduce algorithm
      1. An Short-time Fourier Transforamtion is calculated over the noise audio clip
      2. Statistics are calculated over FFT of the the noise (in frequency: mean, standard deviation, noise threshold)
      3. A threshold is calculated based upon the statistics of the noise (and the desired sensitivity of the algorithm). How many standard deviations louder than the mean dB of the noise (at each frequency level) to be considered signal.
      4. An STFT is calculated over the signal
      5. A mask is determined by comparing the signal FFT to the threshold
      6. The mask is smoothed with a filter over frequency and time
      7. Calculate the threshold for each frequency/time bin, if the signal is above threshold --> convolve the mask with a smoothing filter with scipy.signal.fftconvolve
      8. The mask is applied to the FFT of the signal, and is inverted with Inverse short time Fourier Transform (ISTFT)

### Two additional ways are being tested on top of the algorithm
* Increase the volume of the audio   
* Slowdown the audio

### Evaluation - Google Cloud Speech-to-text API  
   Google Cloud Speech-to-text API is used to evaluate the result of the algorithm. We will run the API for the clear audio files, the noise files and the de-noised files to compare the text being transcribed and the accuracy. 

## 2. Elaborate the use case with Google cloud NLP API
Jupyter notebook: [02 Explore Google AutoML NLP API.ipynb](https://github.com/michliu1212/Audio-Background-Noise-Suppression/blob/ec14f55ecfb34ec49c256daa531cd429eb8cf077/02%20Explore%20Google%20AutoML%20NLP%20API.ipynb)

Tracking and analysing call centre records and data can help to improve customer service and agent performance. It can provide insights of the areas of strength and weakness to improve the service and customer satisfaction. 

With entity analysis, we can first understand the context of the call, is the customer calling about the problem on internet banking, or they want to open a new credit card, or if they see weird transaction in their bank account. Inspecting the entities in the call can easily identify what is the need of the customers, where then can easily classify the calls into different products or services (i.e. credit card, savings account, investment account, etc), further analysis on the transcription and share the analysis/ transcription of the call to the responsible team to understand the needs of the customers and things to improve. 

With the combination of sentiment analysis, we can detect the emotion of the customers when talking about specific products and services. This can provide further information about the view of the customer, if it is positive or negative when talking about specific product and service. However, sentiment analysis are difficult to identify irony and sarcasm which might affect the result.

## 3. Learnings and Next Steps

* The Google speech-to-text API recognizer is designed to ignore background voices and noise without additional noise-canceling. However, excessive background noise and echoes may reduce accuracy, especially if a lossy codec is also used ([Source](https://cloud.google.com/speech-to-text/docs/best-practices)). So from the analysis, we can see that the API are able to transcribe a lot of speech even in the highest level of background noise and are performed particularly well with the stationary noise. The API performs the worst when the background noise are human speaking, such as when neighbour is talking and in a noisy restaurant. In next steps, we should focus on background noise with huaman speaking, to isolate the speech of the main speaker with the others. 

* The way human perceive speech and the way machine the works are very different. While humans can focus on the voice produced by a single speaker in a crowded and noisy environment, this is not the same case with machine. From the examples we see, with our ears, we can clearly identify that the background noise are being removed/ reduced and can easily understand the speech in the audio. However, for machine, the performance are very different from what we perceived. To be able to improve the accuracy, for next steps, more effort will be put to understand the Speech-to-text algorithm and can test with a more robust algorithm such as de-noising with a [deep learning model](https://sthalles.github.io/practical-deep-learning-audio-denoising/).

* The API are sensitive to the volume of audio and the speed of speaking. From the examples where we increased the volume and slowdown the audio, the performance of the transcription is slightly improved. So the volume and talking speed is something we should also consider when using the Google Cloud Speech-to-text API. 

### Thank you! 






