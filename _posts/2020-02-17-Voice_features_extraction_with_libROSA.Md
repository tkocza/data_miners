---
title: Voice features extraction with libROSA
date: 2020-02-17 00:00:00 +0100
categories: [Tutorials, libROSA]
tags: [Python, Audio]
toc: false
seo:
  date_modified: 2020-02-26 22:00:50 +0100
---

LibROSA is a python package for music and audio analysis. It is  simple and powerful package which helps you extract audio features for further analysis.

Before I will show you how easy is using this package, I would like to do a short introduction. It will be helpful if you want to work with audio.

### How does sound work?
A sound is a molecule vibration transfered through transmission medium. This mechanical energy propagated thought molecules is called acoustic wave. When the source of sound is "active", pressure in the air is changed by acoustic wave.
Speakers, microphones or our ears have special membrane. Acoustic wave causes membrane oscillation which is received as sound.  Membrane could have different flexibility what is the reason why we are able to hear sounds with different frequency and amplitude. The sound intensity is measured in Decibels (dB).

### Audible frequency (AF)
AF is a speed of sound's vibration. It is a measure of wave cycles per minutes. The frequency unit is hertz (Hz).

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_audible_frequency.jfif)

### Fourier Transformation (FT)
The Fourier transform (FT) decomposes a function (often a function of the time or a signal) into its constituent frequencies. It transforms a signal from time domain into frequency domain.

### Fast Fourier Transformation (FFT)
Fast Fourier Transformation (FFT) is a mathematical algorithm that calculates Discrete Fourier Transform (DFT) of a given sequence. The only difference between FT (Fourier Transform) and FFT is that FT considers a continuous signal while FFT takes a discrete signal as input. DFT converts a sequence (discrete signal) into its frequency constituents just like FT does for a continuous signal.

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_fast_fourier_transformation.png)
 
### MEL scale
MEL is pitch scale measured by subjective perception of listened sounds by people in scale of measured objective frequency. In simple words - how we hear sound according to its frequency. 

---
# LibROSA
Now it a time for a little bit of code.

I prepared a simple sample, which is available in my GitHub:
# LINK
I want to start with some important parameters, which you will find in LibROSA:
- sampling rate - defines how many times per second a sound is sampled
- frame length - how many samples are in frame
- hop length - number of samples in a shift of frame for analysis

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_sample_frame_hop.png)

Let's code! First of all you need to import necessary packages for further features extraction:

```python
import librosa
from librosa import display
import matplotlib.pyplot as plt
import numpy as np
```

Next you can load audio data:

```python
sample, sampling_rate = librosa.load("Sample_record.wav", sr=None)
```
The default value of *sr* parameter equals 22050, so if you want to keep original sampling rate you cannot skip this parameter. The solution is setting this as *None*

For some analysis you will need data after short-term fourier transformation:

```python
S, phase = librosa.magphase(librosa.stft(sample))
```

Now you could see how loaded data looks on the graph:

```python
plt.figure(figsize=(16, 4))
librosa.display.waveplot(sample, sr=sampling_rate)
```

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_waveplot.png)

### Energy
Energy shows how loud is a sample

```python
S, phase = librosa.magphase(librosa.stft(sample))
rmse = librosa.feature.rms(S=S)

# plot
plt.figure()
plt.semilogy(rmse.T, label='RMS Energy')
plt.xticks([])
plt.xlim([0, rmse.shape[-1]])
plt.legend()
```

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_energy.png)

### Zero crossing rate
Zero crossing rate of any signal frame is the rate at which a signal changes its sign during the frame. It denotes the number of times the signal cross zero, from positive to negative and vice versa within single frame.

```python
librosa.feature.zero_crossing_rate(sample,frame_length=1048, hop_length=512)
```

### Mel spectrogram

```python
melspectrogram_vector = librosa.feature.melspectrogram(y=sample, sr=sampling_rate)
#plot
plt.figure(figsize=(10, 4))
melspectrogram_vector_dB = librosa.power_to_db(melspectrogram_vector, ref=np.max)
librosa.display.specshow(melspectrogram_vector_dB, x_axis='time',y_axis='mel', sr=sampling_rate)
plt.colorbar(format='%+2.0f dB')
plt.title('Mel-frequency spectrogram')
plt.tight_layout()
plt.show()
```

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_mel_spectrogram.png)

### Spectral centroid
It is calculation of the weighted mean of the frequencies present in the signal. It presents spectral shape and position of spectrum through energy around higher frequencies.

```python
spectral_centroid_vector = librosa.feature.spectral_centroid(y=sample, sr=sampling_rate)
# plot
plt.figure()
plt.semilogy(spectral_centroid_vector.T, label='Spectral centroid')
plt.ylabel('Hz')
plt.xticks([])
plt.xlim([0, spectral_centroid_vector.shape[-1]])
plt.legend()
plt.show()
```

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_spectral_centroid.png)

### Spectral roll-off
It is defined for each frame as the center frequency for a spectrogram bin such that at least roll_percent (0.85 by default) of the energy of the spectrum in this frame is contained in this bin and the bins below.

```python
spectral_rolloff_vector = librosa.feature.spectral_rolloff(y=sample, sr=sampling_rate)
# plot
plt.figure()
plt.semilogy(spectral_rolloff_vector.T, label='Roll-off frequency')
plt.ylabel('Hz')
plt.xticks([])
plt.xlim([0, spectral_rolloff_vector.shape[-1]])
plt.legend()
plt.show()
```

![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_spectral_rolloff.png)

## Hints
To show together waveplot with extracted features, you can draw those factors on one plot, but you have to normalize feature data. You will use sklearn package for that:

```python
import sklearn
frames = range(len(spectral_centroid_vector[0]))
t = librosa.frames_to_time(frames, sr=sampling_rate)
librosa.display.waveplot(sample, sr=sampling_rate, alpha=0.4)
plt.plot(t, sklearn.preprocessing.minmax_scale(spectral_centroid_vector[0], axis=0), color='r') # normalize for visualization purposes
```
![](../../assets/img/pictures/2020-02-17-Voice_features_extraction_with_libROSA_spectral_centroid_along_waveplot.png)

### More info
If you are more interested in sound theory I recommend you those articles:
https://medium.com/game-of-bits/audio-analysis-part-1-sound-waves-things-you-must-know-1e10851cc109
https://www.nti-audio.com/en/support/know-how/fast-fourier-transform-fft

### Thanks for reading! 