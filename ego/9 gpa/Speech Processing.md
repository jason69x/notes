
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

