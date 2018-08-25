# EuroshieldGranular
Granular Sampler for Euroshield Module by 1010Music

## Function
The Sketch begins sampling when the Gate Input is HIGH and. After sampling the sample is played back continously until the Gate is LOW. The playback Speed/Ratio can be changed while playback and changes the Pitch of the sample. Try sampling some audio books and modulate the Ratio with a turing machine :)

When the granular Effect begins, it plays back the dry signal for the length of the grain. The Mixer behind the effect opens when the playback of the firtstmost dry sample stopped (Gate goes HIGH + grainLength) and closes when the Gate closes. So the playback starts after the time of the grain lenght has passed (It needs to 'record' the sample first)

The grain size will update when the Gate goes HIGH. 

To reflect different CV Input Voltages, the maximum Value of the CVIn is saved for mapping the playback ratio. To calibrate, you need to Input the maximum voltage one time after rebooting the module. To reset the value (for changing the CV Source) you need to reboot the module. 

*When the Sample starts or ends at a high amplitude a clicking sound can occur. To prevent this, an envelope is used with a 5 ms attack and 5m release, timed at the begin and end of the sample.* (Not really working at the moment, see Known Issues)

## In/Outs
* Input 1:	  Audio Input
* Input 2:	  CV Input for Playback Ratio
* Output 1: 	Output from Granual Effect
* Output 2:	  Dry Output

* MIDI In:	  Gate IN
* MIDI Out:	  n/a
* Upper Pot:	Grain Size (20-290ms)
* Lower Pot:	None
* LED 1:		  active when Gate is HIGH

*Note: The MIDI IN Port (Teensy Port 0) was used as a digital Input.*

## Modified Audio Library
The Sampler can sample up to ~1450 ms of Audio when using a modified Teensy Audio library. The variables in `effect_granular.h` and `effect_granular.cpp` needs to be changed from type int16_t to int32_t. The sketch then can use up to 64000 bytes sample memory. In my setup I was able to use ~ 59015 bytes (1337ms) with 94% Memory Usage. (Teensy 3.5). The variable for the granular memory needs to be int32_t too. Not Sure if it works with Teens 3.2 as I wasn't able to test. 

The modified files are in this repository, make a backup first and copy them to: 

`C:\Program Files (x86)\Arduino\hardware\teensy\avr\libraries\Audio`

You should be able to use the sketch by default. To use the modified library look out for the definitions at the beginning of the code

## TFT/Serial Out
My Euroshield uses a ILI9342 TFT to display the values, but the relevant parts of the code are commented out by default. Per Default is a Serial Out which prints out the values of both Voices every second. Look out for the Definitions at the beginning.

The TFT commands should be compatible with the Adafruit libraries.


## Known Issues

* There's still a clicking sound when the playback of a grain stops (I think). It's clearly audible when using waveforms (e.g. from a VCO) as an audio input. Haven't noticed it when using an audio book as the audio source. 
* I used a Teensy 3.5 for this sketch, at the moment I cannot test it with a Teensy 3.2

## ToDo

* Add Option to use a Trigger to start Sampling
* Adding SD-Card support
* Add Effects
* Option too choose between Freezing and Pitchshifting or use the two Ouputs for each of them
* Fork / Pull request for the modified Audio Library


