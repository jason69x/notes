
a *speech waveform* is a graphical representation of the sound waves produced by human speech. it visualizes how the amplitude (pressure variation) of the audio signal changes over time.
x-axis    ->  time
y-axis    -> amplitude, corresponds to loudness of the sound at each sample.

in its natural form, speech is a analog waveform (continous variations in air pressure captured by a microphone). Digitally, its sampled at regular intervals to create discrete representation.

envelope, overall shape of the wave

*sampling rate* , refers to the number of samples of an analog audio signal taken per second to convert it into a digital signal. its measured in *hz or samples per second.* 

the highest frequency that can be represented (*nyquist frequency*) is half the sampling rate.

a speech signal is typically divided into smaller, overlapping segments called *frames*. (20-40ms)
statistical properties remain relatively constant in very short durations, this allows for effective analysis. processing entire signal would be inefficient and inaccurate.

steady part - duration where acoustic properties of the signal remain relatively constant over time.

*DC shift* - refers to a constant voltage or amplitude offset in the speech signal that shifts entire waveform away from the zero baseline (i.e the mean of the signal is not zero)

in digital waveform, a speech signal should ideally oscillate symmetrically around zero. with DC shift, the entire waveform is shiften up or down by a constant value. (eg. all samples are offset by +0.1 volts).

to mitigate DC shift, a common preprocessing step applied is *Mean substraction* , 
subtract the mean value of the sample from each sample to center the waveform around zero.

$x_{corrected}[n] = x[n] - \frac{1}{N}\sum_{n=0}^{n=N-1}x[n]$

soundwaves are tiny variations in air pressure, these pressure fluctuations push on the diaphragm of the microphone, a microphone is a transducer it converts one form of energy into another.
thin diaphragm vibrates with the air pressure.
*dynamic mic* - diaphragm moves a coil near a magnet $\to$ chaning magnetic field induces current.
*condenser mic* - diaphragm is part of a capacitor $\to$ vibrations change capacitance, producing voltage variations.

*ADC - Analog-to-Digital converter*, takes continuously varying voltage and measures it at regular intervals.

*peak normalization* - divide each amplitude sample by absolute maximum amplitude value.