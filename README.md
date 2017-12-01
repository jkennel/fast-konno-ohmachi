# _Fast_ Konno-Ohmachi smoothing
#### This is a Python module that performs Konno-Ohmachi spectral smoothing very fast (reduces running time by ~60%).

## Description
Konno-Ohmachi is a smoothing algorithm proposed by Konno & Ohmachi (1998) [[Abstract](http://bssa.geoscienceworld.org/content/88/1/228.short), [PDF](http://www.eq.db.shibaura-it.ac.jp/papers/Konno&Ohmachi1998.pdf)], which achieves a "uniform-span" smoothing to frequency spectra in logarithmic scale.

For lower frequencies, the Konno-Ohmachi smoothing window is narrower (i.e., less smoothing), and for higher frequencies, the window is wider (i.e., more smoothing). "Conventional" smoothing filters use same smoothing window widths at all locations. This feature of the Konno-Ohmachi filter is preferred in engineering seismology, where the variations of amplitudes in lower frequencies (< 10 Hz) are more important than in higher frequencies.

The figure below shows the result of Konno-Ohmachi filter versus a "conventional" median value filter [[Wiki](https://en.wikipedia.org/wiki/Median_filter)]. The two filters yield similar results for frequency > 5 Hz, but for lower frequencies, the median filter over-smoothes the raw spectrum, which is undesirable.

![](demo.png)
###### (The raw signal used in this example is the Fourier amplitude spectrum of a ground acceleration waveform recorded during 2011/3/11 Magnitude-9.0 Tohoku-Oki Earthquake.)

## Computation speed
Konno-Ohmachi filter is time-consuming due to varying window widths. This module has pre-calculated smoothing window values built-in, which, compared to other ordinary Konno-Ohmachi smoothers, reduces the calculation time by ~60% (hence the "fast" in the module name).

## How to use this module
Put `konno_ohmachi.py` in your Python search path. Then,

```python
import konno_ohmachi
new_spectrum = konno_ohmachi.fast_konno_ohmachi(spectrum,freq,smooth_coeff=40,progress_bar=True)
```

`Demo_konno_ohmachi_smooth.py` is a Python script that demonstrates how to use the module, and plots the figure you see above.

## Dependencies
`konno_ohmachi.py` only dependes on Numpy 1.11.0+; works for both Python 2.7 and Python 3+.

In order to run `Demo_konno_ohmachi_smooth.py`, you also need Scipy 0.17.1+, and Matplotlib 1.5.1+.


## Limitations
`fast_konno_ohmachi` only supports even integers between [2,100] as eligible smoothing coefficient (i.e., "b"). Out-of-range values, odd integers, and/or decimal numbers will be "cupped"/"capped" within [2,100] and rounded to an even integer.

This is merely to reduce the file size of the source code (because smoothing windows are pre-calculated and hard-coded into the source). In practice, b values outside of [2,100] are very rarely used.

## License
Copyright (c) 2013-2017, Jian Shi. See LICENSE.txt file for details.
