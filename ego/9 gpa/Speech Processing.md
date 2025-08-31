
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
