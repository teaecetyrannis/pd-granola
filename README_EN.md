


# granola
[leer en español](https://github.com/teaecetyrannis/granola/blob/main/README.md)
<br><br>
stochastic granular synth developed in [pure data](https://github.com/pure-data/pure-data) and [camomile](https://github.com/pierreguillot/Camomile)


## installation
download granola.zip file from the [last release](https://github.com/teaecetyrannis/granola/releases/tag/v1.0) and extract on the folder where vst3 plugins are found (to run 
with pd just download the source code run the granola.pd patch)

## operation
the synth features:

1. grain
	- **durat.**: controls grain duration (*ms* or *samps*)
	- **rate**: controls frequency at which grains are triggered (*ms*, *samps* o **sync**)
	<br>if **sync** is activated, grains will trigger only when the DAW or host is playing and the frequency/rythm will be determined by the ration between the numers to the right 
    of *sync* in relation to the quarter note (example: 1/1 stands for a quarter note, 2/1 for a half note, 1/3 for an eighth note triplet) responding accurately to tempo 
    changes
    <br>
2. sample
	- **load**: used to find the audio file (in .wav format) that will be used as the main sample
	- **playhead position**
	- **play**: if turned on, moves the playhead through the sample at the indicated speed (in other words, it resumes or pauses sample playback)
	- **reset_on_noteon**: when **play** is turned on, determines if the playhead will be one for all voices (off) or if each new note will begin to play the sample from **playhead position** independently (on)
	<br>keep in mind that when turned on moving the **playhead position** will have no effect since it's just determining at which point of the sample will the next note begin, whereas if it's turned off all voices will share the same playhead which can be altered by changing the **playhead position**
	- **mono/stereo**: determines if the sample is mono or stereo
	if it's stereo there's the freedom to choose *mono*, which will simply duplicate the sample's left channel
	if it's mono, then *mono* option should be selected unlesss silence from the right channel is desired
	- **speed**: determines the speed at which the playhead/s will move (*%*)
	<br>the slider found above can be used too, which goes from -300 to 300, for a more intuitive control if using a mouse
	- **origin_note**: allows to indicate which is the "note" (*midi*) of the sample so that it matches the notes that go into the synth, but also allows for more creative use cases that this one

3. window
	- **shape**: determines the shape of the window that is applied to each grain (*straight, cos, tri, tridown, triup, custom*)
	- **load**: used to find the audio file (in .wav format) that will be used as the *custom* shape, the window is always 2048 samples long and if the loaded file is longer only the first 2048 samples will be kept
	- **smooth**: si se enciende, una segunda ventana es aplicada a cada grano con breves rampas desde y hacia cero al comienzo y al final, esto sirve para evitar los impulsos generados por las formas de ventana que no empiezan y terminan en un mismo valor

4. envelope
	- attack (*ms*), decay (*ms*), sustain (*%*) and release (*ms*)
	- controls for switching between monophonic and polyphonic, legato, voice-stealing and behaviour of pitch bend (global applies bending to all active voices of the polyphony and last applies it only to the newest voice)

5. offsets
  the synth has three panels which allow to change the values of three parameters: playhead position (*ms* or *samp*), pitch (*midi* or *cents*) and duration (*ms* or *samps*) from left to right
	- **offset**: how much is added or substracted from the current value of each parameter
	- **chance**: the chance (*0-100%*) that said operation gets executed for each individual grain
	- **precision**: defines the margin of possible outcomes (*0-100%*), the outcome could be any number (with equal probability) that lies between the current value + precision% of the **offset** value AND the current value + 100% of the **offset** value
	<br>example: if set to *100* the outcome will always be the current value + the **offset** value, if set to *50* the outcome could be any number between the current value + 50% of the offset value AND the current value + the **offset** value (all of them equally probable)

WARNING:
- keep in mind that when **load**ing audio files the plugin will remember the path for said files and it is there that it will look for it the next time that te project/preset gets loaded
- when in polyphonic mode and especially if using very small **rate** values cpu usage may increase quite a bit with each voice, technically it allows up to 16 voices but in the pc i develop with (i5-4440) the buffer overflows before using them all

## credits
- [pure data](https://github.com/pure-data/pure-data) by miller puckette y many others
- [camomile](https://github.com/pierreguillot/Camomile) by pierre guillot
