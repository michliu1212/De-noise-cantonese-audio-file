# Audio-Background-Noise-Suppression

Real applications have to recognize audio even when there are other irrelevant sounds happening in the environment. The audio files from the call center are full of background noise, to be able to convert call audio from call center to transcription accurately, it is important to remove or reduce the background noise from the audio.  

## 1. Preparation of Dataset
Main dataset are produced from Cantonese Youtube videos that are downloaded and converted to WAV files.  

###  1. Add random segment of environmental audio to the audio
A folder of background noise WAV file from Microsoft Scalable Noisy Speech Dataset (MS-SNSD) [GitHub](https://github.com/microsoft/MS-SNSD) with white noise and recordings of machinery and everyday household activities are used in this analysis to add noises on the cantonese audio. The loudness of the background noise are used in 5 levels, with 00 the largest and 40 the smallest. 
Since the sample rate are different from the main dataset (16 kHz instead of 44.1 kHz), it will required further process to combine the two audios. 

## 2. Noise Suppression MVP

## 3. Check the performance
  ### 1. Google Speech-to-text
  ### 2. Signal-to-noise ratio

## 4. Elaborate the use case with Google cloud NLP API


