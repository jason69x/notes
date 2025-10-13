
**Energy** - refers to a measure of intensity or amplitude strength of an audio signal over a short time frame. its a key feature for distinguishing silence or background noise from active speech.

*Amplitude* - it represents the maximum displacement of a quantity from its equilibrium value.

*root mean square amplitude* - $A_{RMS} = \sqrt{\frac{1}{N}\sum_{i=1}^{N}x[i]^2}$ 

energy $\propto$ ${A_{RMS}}^2$

*zero crossing rate (zcr)* -  number of times a signal crosses the zero amplitude axis within a given time frame.
high zcr indicates presence of high frequency components, such as noise, fricatives(eg. 'f' or 's' sounds in speech) or unvoiced segments. in contrast, low zcr indicates low frequency, smoother signals like voiced speech (eg. vowels).

A *spectogram* is a picture of sound where : 
 - x-axis = time
 - y-axis = frequency
 - colors/brightness = intensity (energy at that frequency)

fricatives are messy because of turbulent airflow.

***Euclidean distance*** is a simple way to measure how close or far two feature vectors are in a multi-dimensional space.

$d(x,y) = \sqrt{\sum_{i=1}^{n}(x_i - y_i)^2}$ 

here , $x$ and $y$ are two feature vectors with same dimensions and $x_i,y_i$ are the elements of these vectors.

each number represents amplitude of the waveform at that sample/frame
energy is the measure of how "loud" a signal is.

$E = \sum_{i=1}^{n} {x_i}^2$

squaring makes sure negative amplitudes contribute positively
we take average to minimize the effect of length of the sample.

###### Articulation and Acoustics

air-from-lungs -> trachea -> larynx -> vocal folds contracts and vibrates -> articulators -> air out of mouth/nose

vocal folds are adjusted so that there is only a narrow passage between them, the airstream from the lungs will set them vibrating. sounds produced when the vocal folds are vibrating are said to be *voiced*, as opposed to those in which the vocal folds are apart, which are said to be *voiceless*.

parts of vocal tracts that can be used to form sounds are  called *articulators* eg. tounge , lips

airstream process -> phonation process -> oro-nasal process -> articulatory process

*pitch* - how high or low your voice sounds

sound consists of small variations in air pressure that occur very rapidly one after another.

air pushes vocal folds apart then they come together and again air pushes them apart this cycle happens super fast 100-300 times/second . each cycle chops  the airflow into little bursts of pressure, so that pulses of relatively high pressure alternate with moment of lower pressure.


---

*phonemes* -   are the smallest units of sound in a language that can distinguish meaning between                        words. eg pat and bat . here p and b are phonemes. if two sounds don't change                               meaning they are called allophones

when we speak we generate continous sound waves and not neatly separated letters. ASR systems need to figure out which phonemes are being spoken.

**acoustic model** learns the relationship between phonemes and sound wave features.
**lexicon** maps phoneme sequence to words.
**language model** figures out which word sequences makes sense.

eg. you say "i scream" , acoustic model detects ai skri: m. lexicon says could be "ice cream" or "i scream". language model + context help choose the right meaning.

```python
Speech Waveform (raw audio)
↓
Framing & Windowing (split into short segments)
↓
Feature Extraction (MFCCs, spectral features, etc.)
↓
Acoustic Model (HMM / DNN predicts phoneme probabilities)
↓
Phoneme Sequence
↓
Lexicon (maps phonemes → words)
↓
Language Model (chooses most likely word sequence)
↓
Recognized Words (final output)
```

*semi-vowels/glides* -  are sounds that are produced like vowels (open vocal tract, voiced, no turbulence) but they function like consonants. voiced $\to$ vocal tract vibrate.

*vowel* - open vocal tract, no major obstruction of airflow, voiced. you can't have a word without a vowel.

consonants - airflow is obstructed somewhere, can be voiced or voiceless. eg p,t,s,m,k.

*Auto Correlation and Lag* - 

autocorrelation measures how similar a signal is to a **shifted** (lagged) version of itself. if a signal matches well with its shifted copy $\to$ high autocorrelation else low or negative autocorrelation.
"if i slide this signal over itself, how well do the shapes line up".

for a signal $x[i]$ of length $N$ :

$R_{xx}[k] = \frac{1}{N}\sum_{i=0}^{N-1-k}x[i]\cdot x[i+k], k = 0,1,2,...$  

- $R_{xx}[0] \to$ energy of signal (max autocorrelation)
- $R_{xx}[k] \to$ similarity at lag $k$. (autocorrelation value at lag k)
- $N \to$ total number of samples
- $x[i] \to$ signal at sample index $i$ in the original signal.
- $x[i+k] \to$ signal at sample index $i+k$ in the shifted signal.

voiced speech is quasi-periodic
first big peak after $R[0] \to$ pitch period.

Fundamental Frequency $F_0 = \frac{samplig\ rate}{lag\ of\ peak}$ 

fundamental frequency is the lowest frequency of a periodic signal, base frequency of your voice. (pitch).


**diphthong** - is basically a gliding vowel , the tongue (and sometimes lips) move from one vowel position to another in the same syllable. eg "ride"-ai , "now"-ao, "boy"-oi.

semi-vowel, vowel like sound used as a consonant, eg. "yes" - y /i/

**nasal sounds** , are speech sounds produced when air passes through the nose instead of mouth.
this happens because *velum* (soft palate) is lowered, letting air escape through the nasal cavity.
eg. "man" - m, "no" - n

*spectrogram* - visual representation of sound showing **frequency content over time**.

- x-axis : time
- y-axis : frequency
- color/intensity : amplitude(loudness) at each frequency

gaps in nasal spectrogram $\to$ due to anti-resonances from oral cavity and lower energy.

**ZCR** - *zero crossing rate* , is the rate at which a signal crosses the zero amplitude line. high zcr $\to$ lots of high frequency components (noisy sounds, fricatives), low  zcr $\to$ low frequency or smooth sounds (vowels, voiced sounds).

**sample rate**  - how many times per second we measure the amplitude of a sound wave. *hz*
nyquist theorem, sample rate must be at least twice the highest frequency you want to capture.

speech is non-stationary, meaning the signal changes over time. to analyze it we look at small frames (20-40ms), where it is roughly stationary called a window.

*windowing* - multiplying a frame by a window function to reduce edge effects.
common windows : hamming,hanning,blackman $\to$ smooths the edges of each frame which reduces artifacts in fourier analysis.

**cepstral analysis** -

cepstrum  = spectrum of the **log magnitude spectrum** of a signal.

1. take a short window of speech
2. compute fourier transform $\to$ get magnitude spectrum
3. take logarithm $\to$ separates excitation(pitch) and vocal tract effects
4. take inverse fourier transform $\to$ gives cepstrum

helps separate source(vocal chords) from filter(vocal tract)

**MFCC** - *mel-frequency cepstral coefficients*


**short time linear prediction** - 

linear prediction models a speech sample as a linear combination of previous samples.

$x[n] \approx \sum_{k=1}^{p}a_k\cdot x[n-k]$

- $x[n]$ = current speech sample
- $a_k$ = LP coefficients (model vocal tract)
- $p$ = order of prediction

LP tells us vocal tract shape by modeling the filter.

frame rate in lp = how often we compute LP features.
1. choose frame length $L$ 
2. choose frame shift $S$ 
3. frame rate = $\frac{1}{S}$ 

**Discrete fourier transform** - 

$X[k] = \sum_{i=0}^{N-1}x[i]e^\frac{-j2\pi ki}{N}$  , $k = 0,1,2\ldots N-1$ 

time complexity of covariance method to calculate LP coefficients is $O(p^2N)$ (forming matrix) + $O(p^3)$ (solving matrix).


hamming window is required in LPC autocorrelation method because it reduces edge effects, suppresses spectral leakage and ensures the stability/accuracy of LPC coefficients.

**liftering** - filtering in the cepstral domain. apply a window to cepstral coefficient

lower order cepstral coefficients are sensitive to spectral slope and higher order are sensitive to noise. to reduce these sensitivities we perform liftering.

**toeplitz matrix** - each descending diagonal (top left to bottom right) is constant.
it is symmetric. it is defined by first row and first column, so $2*n-1$ elements req. to store

**k-means algorithm** -  is a clustering algorithm, it groups data points into k clusters based on similarity (usually euclidean distance).

- choose k initial cluster centroids (can be random or based on some heuristic)
- for each data point find the nearest cluster and assign it to that cluster
- recompute each cluster center as mean of all points assigned to it.
- go back to step 2 until cluster centers don't change much (convergence)

---

**DC Shift** -

dc shift is a non-zero mean in a recorded audio signal.

$\mu = \frac{1}{N}\sum_{i=0}^{N-1}x[i]$

when $\mu\neq0$ , the waveform is shifted up or down around zero instead of being centered at 0 - this is DC shift.

Energy,zcr,autocorrelation,LPC estimate assumes zero-mean signals, offset biases all of these.
a dc shift adds energy at 0hz. in DFT the dc bin gets a large value, biasing log-magnitude spectra and mel filter outputs.

to remove dc shift, you can subtract mean from every sample.
you can also do frame level mean subtraction or subtract running avg. mean.

$m[i] = \alpha m[i-1] + (1-\alpha)x[i]$ ,  then subtract $m[i]$ from $x[i]$ .


**Normalization** -

transform signal/features to reduce variability caused by environment,recording devices,speaker differences etc.

*peak amplitude normalization* :

$x^{'}[n] = \frac{x[n]}{max(\lvert x[n]\rvert)}$  , divide all samples by absolute value of maximum sample value so that each sample lies between $[-1,+1]$ .

*clipping* , form of a distortion when audio signal exceeds the maximum level that a system can handle.

*rms energy normalization* :

$RMS(x) = \frac{1}{N}\sqrt{\sum_{i=0}^{N-1}x[i]^2}$

$x^{'}[n] = \frac{x[n]}{RMS(x)}\cdot targetRMS$

compared recordings independent of speaker loudness

*z-score normalization* :

$\mu = \frac{1}{N}\sum_{i=0}^{N-1}x[i]$

$\sigma = \sqrt{\frac{\sum_{i=0}^{N-1}(x[i]-\mu)^2}{N}}$

$x^{'}[n] = \frac{x[n]-\mu}{\sigma}$ 

now mean = 0, variance = 1
removes DC bias and loudness scaling simultaneously

---
*frequency domain approach* - 

analyzing speech signal **in terms of its frequency content** rather than directly working with raw time samples.

A **speech signal** $x[n]$ varies over time - time domain representation.
but speech is made up of different frequencies, analyzing those gives deeper insights.

in the frequency domain, we represent the same signal as a function of **frequency**.
usually via **Fourier Transform** .
$X(f) = \mathcal{F}(x[n])$ 

using DFT/FFT , converts  a time frame into its **spectral representation** - showing which frequencies are present and their amplitudes.

*formants* - resonant frequencies of the vocal tract.

***fourier transform*** - 

every complex signal can be thought of as a mix of simple sinusoids - sin and cosines, each with its own frequency, amplitude and phase.
fourier transform breaks down a time-domain signal into its frequency components. it shows how much of each frequency exists in that signal.

*discrete-time fourier transform* , 

$X(e^{j\omega}) = \sum_{n=-\infty}^{\infty}x[n]e^{-j\omega n}$ 

inverse, $x[n] = \frac{1}{2\pi}\int_{-\pi}^{\pi}X(e^{j\omega})e^{j\omega n} d\omega$ 

*discrete fourier transform* , finite samples

$X[k] = \sum_{n=0}^{N-1}x[n]e^{-j\frac{2\pi}{N}kn}$ 

inverse, $x[n] = \frac{1}{N}\sum_{k=0}{N-1}X[k]e^{j\frac{2\pi}{N}kn}$ 

condition for existence of fourier transform, $\int_{-\infty}^{\infty}\lvert x(t)\rvert dt < \infty$ 
total area under the magnitude of $x(t)$- a signal, must be finite.

---
**k-means**

*k means* is an unsupervised learning algorithm user for *clustering data* into *k* distinct groups based on similarity.
it tries to find *k* centers (called *centroids*) such that each data point belongs to the cluster with the nearest centroid.
centroids are called codewords and the set of all centroids in called the codebook.

1. initialize cluster centers
2. assign observations to closest cluster center
3. revise cluster centers as mean of assigned observation
4. repeat 2&3 until convergence

The k-means algorithm divides a set of N samples X into K disjoint clusters C , each described by the mean $u_j$ of the samples in the cluster (centroid).
the k-means algorithm aims to  choose centroids that minimise the **inertia**.

*inertia*, is a measure of how internally coherent the clusters are. basically, how close the points in each cluster are to their respective cluster centers.

$Inertia = \sum_{i=1}^{k}\sum_{x\in C_i}\lvert\lvert x - \mu_i\rvert\rvert^2$ 

total inertia is the sum of inertia of all clusters, inertia of a cluster is the sum of square of euclidean distance between each point and the centroid.

lower is better , zero is optimal. in high dimensional spaces, euclidean distance becomes inflated.
voronoi diagram

size of codebook is the number of representative vectors we are using (*k*) .

vector quantization, chooses a set of points to represent a larger set of points.

tokhura distance, weighted euclidean distance.

**LBG - Linde Buzo Gray**

is a algorithm for designing a *vector quantization* codebook from training vectors. it builds a small set of prototype vectors (codewords) that are used to represent many real vectors with only the nearest prototype vector, saving bits.

1. start with the universe, compute its centroid.
2. repeatedly split each codeword in the codebook and run *k-means* for refinement, till the codebook contains desired number of codewords.


