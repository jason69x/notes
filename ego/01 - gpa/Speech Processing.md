
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

*k means* is an unsupervised learning algorithm used for *clustering data* into *k* distinct groups based on similarity.
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


---
**Discrete Markov Process** 

a discrete markov process is a type of stochastic (random) process where : 

1. the system moves between a finite set of states.
2. time is discrete
3. the markov property holds - the probability of moving to the next state depends only on the current state, not on the full history.

also known as first-order markov process.

raise the transition matrix to the power of t and multiply the initial probability distribution to get the probabilities after t steps.

$\pi^{(t)} = \pi^{(0)}\cdot P^t$

if today probabilities of sunny and rainy are $[\ \begin{matrix}1 & 0\end{matrix}\ ]$ , then tommorow they will be
$\begin{bmatrix}1 & 0\end{bmatrix}\ \cdot \ \begin{bmatrix}0.8&0.2\\0.5&0.5\end{bmatrix} = \begin{pmatrix}0.8&0.2\end{pmatrix}$


**Hidden Markov Models**

a markov process with hidden states and visible outputs.

---

*stochastic processes* - 

experiments in which outcomes of events depend on the previous outcomes.
stochastic processes involve random outcomes that can be described by probabilities.
such a process or experiment is called a *markov chain* or *markov process*.

*transition matrix*  - 

each row in the matrix represents an **initial state**.
each column represents a **terminal state**.
square matrix
each row adds to 1
transition matrix represents change over one transition period

$t_{i,j}$  is the probability of moving from state represented by row *i* to the state represented by row *j* in a single transition.

$t_{i,j}$   = $P\ ($ next state is column *j* $|$ current state is row *i* $)$ 

*state vector* -

way to represent the distribution among the states at a particular point in time.

if we want to determine the distribution after one transition, we will need to find a new state vector by multiplying previous state vector with the transition matrix.

for *nth day* $V_n = V_0\cdot T^n$ 

$T^n$ - probability of bike being at a particular station after *n* transitions, given its initial station.

*markov models with hidden states* -

these are the types of problems that describe the evolution of observable events, which themselves, are dependent on internal factors that can't be directly observed.

a hidden markov model is made of two distinct stochastic processes.
*invisible process* & *observable process*

the invisible process is a markov chain, like chaining together multiple hidden states that are traversed over time in order to reach an outcome.

hidden markov models describe the evolution of observable events.

*markov assumption* -

probability of reaching the next state is only dependent on the probability of the current state.

- hidden states
- transition matrix
- sequence of observations
- observation likelihood matrix
- initial probability distribution

hidden states, are those non-observable factors that influence the observation sequence.

transition matrix, probability of going from one state to another.

sequence of observations, each observation representing the result of traversing the markov chain.

observation likelihood matrix, probability of an observation being generated from a specific state.

initial probability distribution, this is the probability that the markov chain will start in each specific hidden state.

*evaluation problem* - 

calculate the probability of a given observation sequence.

in regular markov chain, to calculate the likelihood of observing the sequence of outcomes, *ok,fail,perfect* you'd traverse the chain by landing in each specific state that generates the desired outcome.
at each step you would take the conditional probability of observing the current outcome given that you observed previous outcome and multiply it with transition probability of going from one state to the other.

the big difference is that, in a regular markov chain, all states are well known and observable. not in a hidden markov model. in an hidden markov model you observe a sequence of outcomes, not knowing which specific sequence of hidden states had to be traversed in order to observe that.


--- 

*vector quantization* - 

you take a bunch of high-dimensional vectors . Instead of storing all of them, you cluster them and store only the *representative* vectors (centroids). 
then every original vector is replaced by the **index** of the nearest centroid.

*idea behind vq* - 

the concept of building a codebook of distinct analysis vectors (variation of each phoneme).
goal is to compress data.

*advantages of vq* - 
- less storage space
- reduced computation to determine similarity b/w coefficients
- discrete representation of speech signals.'

*disadvantages of vq* - 
- large codebooks affect performance
- spectral distortion is introduced. quantization error.


let $\vec{x}$ be a *k*-dimensional vector whose components are real valued random variables.
a vector $\vec{x}$ is mapped onto another *k*-dimensional vector $\vec{y}$ and is written as $\vec{y} = q(\vec{x})$ 

$Y = \{\vec{y_i}\} ; 1\leq i \leq k$ 

the set of vector $Y$ is called the codebook and each $\vec{y_i}$ is called a codeword.
size *k* of codebook is no. of levels.

k-dimension of $\vec{x}$ , k no. of entries in codebook.

$d(\vec{x},\vec{y})$ , quantization distortion

$D = \lim\limits_{m\to\infty} \frac{1}{m} \sum\limits_{n=1}^{m} d(x(n),y(n))$ ,  average distortion

$D$ tells us how good the vector quantizer is. 

to make a codebook, the k-dimensional space of $\vec{x}$ is partitioned into $K$ regions

*conditions necessary for optimality* - 

1. nearest neighbor selection rule,
   $q(\vec{x}) = \vec{y}$ *iff* $d(\vec{x},\vec{y_i})\leq d(\vec{x},\vec{y_j})\ \ \forall i\neq j\ \  ; i\leq j\leq k$ 
   choose that $\vec{y_i}$ for which distortion is minimum.

2. centroid condition,
   $\vec{y_i}$ should be chosen to minimize average distortion in the region $C_i$ 
   $\vec{y_i} = \frac{1}{m}\sum\limits_{j=1}^{m}\vec{x_j}\ \ \ ;\ \vec{x_j}\in C_i$

*generalized lloyd's / k-means* -

let *K* be the number of clusters. k regions  { $C_i$ } $i = 1,2,\ldots,k$ 

*step 1 , initialization*
set $m=0$ , where $m$ is the iterative index.
choose a set of initial code vectors.
there are *k* entries of *k*-dimensional vectors in the codebook.

*step 2 , classification*
we shall populate the clusters, put $\vec{x_i}$ in the appropriate bucket using the nearest neighbor selection rule.
bucket refers to codebook entries.

*step 3, code vector updation*
set $m=m+1$ 
update code vector of each cluster by computing the centroid of each cluster

*step 4, termination*
if $D(m)$ at iteration *m* relative to $D(m-1)$ is below a certain threshold stop. else go to step 2.

how we choose the initial choice of code vectors, affect the entire process. this is known as *empty cell problem*.
if the code vectors are too much distributed, it may happen that some buckets will remain empty. it means that average distortion will go up. we can't compute centroid since $m_i$ will be $0$ .

*to solve empty cell problem* -

- increase universe size, empty bucket probability low.
- decrease size of *k*
- ensure initial codebook is good.

*LBG algorithm / binary split algorithm* -

improves initial generation of codebook. binary split / modified k means.
initially codebook size is $1$ and that is mean of the universe.
we can gradually increase its size such that it remains optimal.

*step1*
design a one-vector codebook

*step2*
double the size of codebook by splitting each entry $\vec{y_n}$ 
$\vec{y_n}^+ = \vec{y_n}\cdot(1+\epsilon)$ 
$\vec{y_n}^- = \vec{y_n}\cdot(1-\epsilon)$ 

$0.01\leq\epsilon\leq 0.05$ , is the splitting parameter

*step3*
use k-means to get best set of centroids for the split codebook.

*step4*
iterate steps 2&3 until a codebook of size *m* is reached.

![[LBG.png]]

*extensions* - binary search tree, matrix quantization
LBG might give better results
lbg has more no. of iterations but less distortion , k-means has more distortion but takes less iteration.

we can make another codebook with variations of the same sounds. a tree where root is actual codebook with k childrens . each children represents a single sound. complexity will go up , but improvement will go up too.
use concept of annealing
heat quickly and cool slowly. molecular gap,movement decreases. tightly packed.
codebook moves out of local minima and converges to better minima by introducing random numbers to move code vectors. eg. genetic algorithm,swarm algo for improv.

---

after vector quantization, we get some observations. we can regenerate codebook from these observations and signal from codebook. entire process is reversible.

data modelling uses these observations and will compare the speech and give percentage how much it matches.

