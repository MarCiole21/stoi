# Short-Time Objective Intelligibility Measure

<blockquote>Python implementation of the Short-Time Objective Intelligibility (STOI) measure as described in C.H. Taal, R.C. Hendriks, R. Heusdens, J. Jensen <em>'A Short-Time Objective Intelligibility Measure for Time-Frequency Weighted Noisy Speech'</em>, ICASSP 2010, Texas, Dallas.</blockquote>

**MATLAB Reference**: [http://insy.ewi.tudelft.nl/content/short-time-objective-intelligibility-measure](http://insy.ewi.tudelft.nl/content/short-time-objective-intelligibility-measure)

<hr>

## Usage

`d = stoi(sigclean, sigproc, fs)` returns the output of the Short-Time Objective Intelligibility (STOI) measure described in Taal et. al. (2010) & (2011), where `sigclean` and `sigproc` denote the clean and processed speech, respectively, with sample rate `fs` measured in Hz. The output `d` is expected to have a monotonic relation with the subjective speech-intelligibility, where a higher `d` denotes better intelligible speech. See Taal et. al. (2010) & (2011) for more details.

The model consists of the following stages:

1. Removal of silent frames. DFT frames of the input signals that have an average energy of 40 dB less than the most energetic frame are removed.
2. Expansion of the signals into a Fourier filterbank with a Hanning window length of 25ms and 256 channels covering the the frequency range from 0 to 5 kHz. The energy of the bands are then summed into third-octaves.
3. The output `d` is computed by a correlation process. See the referenced papers for more details.

## Example

The following example shows a simple comparison between the intelligibility of a noisy speech signal and the same signal after noise reduction using a simple soft thresholding (spectral subtraction):

```matlab
% Get a clean and noisy test signal
[f,fs]=cocktailparty;
Ls=length(f);
f_noisy=f+0.05*pinknoise(Ls,1);

% Simple spectral subtraction to remove the noise
a=128; M=256; g=gabtight('hann',a,M);
c_noise   = dgtreal(f,g,a,M);
c_removed = thresh(c_noise,0.01);
f_removed = idgtreal(c_removed,g,a,M);
f_removed = f_removed(1:Ls);

% Compute the STOI of noisy vs. removed
d_noisy   = taal2011(f, f_noisy, fs)
d_removed = taal2011(f, f_removed, fs)
```

This code produces the following output:

d_noisy =

    0.9770


d_removed =

    0.9915

## Built With

* [Python 3](https://www.python.org)

## Developers

* **Aswin Sivaraman**