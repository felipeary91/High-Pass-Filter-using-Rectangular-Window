# High-Pass-Filter-using-Rectangular-Window
A high-pass filter allows to pass signals with a frequency higher than that of a determined cutoff frequency, while attenuating signals with a frequency lower than this cutoff frequency. This high-pass filter has been designed using the "Rectangular Window" method, which uses a
set of N coefficients all with value of 1. Though being easy to implement, the Rectangular Window method is not really used for digital filter design, due to its low attenuation of the stopband and producing filters characterized by extreme values (e.g. sudden jump from zero at the initial sample).

Output samples of a FIR low-pass filter are produced based on:

```math
y(n) = \sum_{j=0}^{N} b_k \times x[n - j]
```

While its frequency response is given by:

```math
H(w) = \sum_{j=0}^{N} b_k \times e^{-jwn}
```

The parameters $b_k$ are calculated using the DFT (Discrete Fourier Transform) method.

In order to test the time response of the filter you need to create the mex function in MATLAB using the command `mex -R2018a FIR_HP_Rectangular.c`. When calling the function you need to do it passing the parameters shown below:

```hs
FIR_HP_Rectangular(input_signal, passband_frequency, stopband_frequency, sampling_frequency, stopband_ripple);
```

Overall, the following MATLAB code helps plotting the time response:

```hs
fs = 20000;
dt = 1/fs;
F = 5000; 
t = 0:dt:0.0025;
x = sin(2*pi*F*t);
plot(x, 'red')
[filt_data, freq_response] = FIR_HP_Rectangular(x, 3000, 2000, 20000, 0.01);
hold on
plot(filt_data, 'blue')
```

Here, a signal with a frequency of 5KHz is passed through the filter, depending on the cutoff frequency (which is half the sum of the passband and the stopband frequencies), the signal might be allowed or attenuated. In this case, the cutoff frequency is ~2.5KHz, so the signal will be allowed.

On the other hand, the frequency response (with a length of 512 points) is also quite easy to plot:

```hs
f = 0:10000/512:10000;
plot(f, 20*log10(abs(freq_response)))
```

**Note:** As the order of FIR filters increase, the delay of the output increases based on `filter_order/(2 * sampling_frequency)`. In this implementation the delay is not compensated.
