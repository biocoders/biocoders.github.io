# Fourier Transformation

(by Emine Topcu)

There are many mathematical tools used in many aspects of science. Fourier Transformation (FT) is one of the most commonly used, and for a good reason. There are many online resources that explain what Fourier Transformation is. This post will only provide a brief summary of FT and a couple of examples of its applications. If you want a more detailed description, https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/ is a great source.
In short, Fourier Transformation allows transforming a function from a time domain to a frequency domain. What does that mean? Let’s say you have data with different values across time. The x-axis is time, and the y-axis is your variable. By FT, you can convert this function to a summation of different sinusoidal waves with different frequencies, amplitudes, and starting points. If you have a complex signal for 1000 ms, for example, with one data point for each ms, FT will convert this function summation to 1000 wave functions. There are many formulas for FT; here, I will show the simplest one:


$F_n=\sum_{t=1}^Tx_t*e^{-i2πnt/T} $


Here xt is the value of our signal at time t. Every data point will contribute to the frequency n as in the above formula. We got a value for each frequency. What is this value? If you notice, it includes an e to the power of an imaginary number. This notation is used to be able to wrap both the amplitude and the phase delay within the same expression. Because using Euler’s formula, it is possible to write an exponential expression as a summation of cos and sin, as shown in the below figure. So, the cos2 + sin2 will give the amplitude of the wave, and the angle will dictate when the wave starts (as not all of them will start at 0). The amplitude and starting points of every frequency will be calculated with this formula.

<img align="center" src="images\FT_Euler.png">

By Original: GuntherDerivative work: Wereon - This file was derived from Euler’s formula.png, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=821342

There are many neat properties of FT. The very first one is that every continuous signal can be expressed as a series of frequencies by the above formula. Furthermore, the result is unique, and it is possible to regenerate the time series back by using the FT.


$x_t=1/T \sum_{n=1}^T F_n*e^{-i2πnt/T}$


OK, so we converted our signal to a series of sinusoidal waves. We had 1000 data points before, and now we have 1000 sin waves. What is the point? FT by itself is not a simplification method. Nevertheless, it can be used for simplification.


Let’s say my signal is a very simple sinusoidal wave with noise, and I want to get rid of the noise. Below is a snippet of Python code to generate such a signal using the NumPy package.
```
import matplotlib.pyplot as plt
import numpy as np
import random as rnd
sinWave = np.fromiter((np.sin(i * np.pi/180) * (1 + rnd.random()/10) for i in range(720)), dtype = "float")
plt.plot(sinWave, lw=1)
```
<img align="center" src="images\FT_SinWithNoise.png">

I use the Fast Fourier Transform (FFT – an algorithm that uses FT) of this signal, and below are the plots of the real and imaginary parts of the results, which can be used to create the waves.

```
transformed = fft.fft(sinWave)
plt.plot(transformed.imag)
plt.show()
plt.plot(transformed.real)
plt.show()
```

<img align="center" src="images\FT_SinFFTIm.png">
<img align="center" src="images\FT_SinFFTReal.png">


I will not get into details of symmetry but briefly mention it. If you notice, both plots above show some symmetry: the imaginary component is symmetrical across the center point, and the real component is symmetrical across the central line. This is explained by the fact that our starting function is in the real domain; that it does not have any imaginary numbers, to begin with. Here is a more detailed description and the proof behind it if you would like to learn further: Symmetry Properties of Fourier Transformation
What can we do with this Fourier transformation? Let us try a very commonly used technique: low pass and high pass filter. When we take the low frequencies only and zero out the rest, and convert it back, we get the plot below. As you can see, it is very close to the sin wave without the noise.
<img align="center" src="images\FT_LowPass.png">

On the other hand, if we remove the low frequencies but allow only the high frequencies, we end up having the noise component only:

<img align="center" src="images\FT_HighPass.png">

This makes sense, as our original function had the sine wave with a low frequency, with the noise with high frequency on the top.

How is FT used in neuroscience? The first way that comes to mind is how FT can be used with EGG results to see what frequency of activity (like beta, gamma, theta) is dominant in the brain (Kaur and Singh 2015). In another study, FT was used to calculate the tail beat frequency of a zebrafish using recorded time series (Jha and Thirumalai 2020). In another study, the frequency of pacemaker action potentials is determined by finding the dominant frequency by FFT (Shifman et al. 2020).
Fourier transformation is widely used, but it isnot the best choise of algorithm in every case or data. We will talk about some other transformations in the future. Keep watching!

## References:
Jha U, Thirumalai V (2020) Neuromodulatory Selection of Motor Neuron Recruitment Patterns in a Visuomotor Behavior Increases Speed. Curr Biol 30:788-801.e3. https://doi.org/10.1016/j.cub.2019.12.064

Kaur C, Singh P (2015) EEG Derived Neuronal Dynamics during Meditation: Progress and Challenges. Adv Prev Med 2015:1–10. https://doi.org/10.1155/2015/614723

Shifman AR, Sun Y, Benoit CM, Lewis JE (2020) Dynamics of a neuronal pacemaker in the weakly electric fish Apteronotus. Sci Rep 10:1–9. https://doi.org/10.1038/s41598-020-73566-3

