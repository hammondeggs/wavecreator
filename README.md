# Wave Creator
Interactive Wave Creator / Sample Loader for KORG 'logue synthesizers.

#### PLEASE NOTE. This is EXPERIMENTAL SOFTWARE. It is to be used at your OWN risk! 

This application requires the .NET Framework 4.5 to be installed, which it likely already is. Windows 7 minimum.

### A quick word...
I've been having a ton of fun creating these plugins, and it's thirsty work. If you like stuff like this and my other work, by all means feel free to contribute whatever you can to the fund to help fund the beer supply! This project admittedly used up a lot!

This can be done here :  [Donate!](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=MSTCVLXMG7Z5J&source=url)


__*please be certain to review the current license as well!*__


Welcome to the world of 8 bit sample playback / synthesis for your KORG 'logue series synthesizers! Who said 32K was enough for anybody?

* *Note - Regarding the term "Samples" - while "technically" the term "Sampler" derives from the fact that an audio signal voltage is periodically "sampled" at a "Sample Rate" (e.g. 44,100 times a second), thus there would be 44,100 'samples' per second, common parlance uses the term "Sample" to represent the entire sound chunk - as this was typically part - or a 'sample' - of a larger piece / song. Unless otherwise specified, this documentation will use the latter phrasing.*

This application will allow you to prepare a file that is currently directly compatibly with your synthesizer librarian. You can have up to 127 samples, with 30567 bytes currently available to store the sample data / descriptor data. *Note! As items are fixed, or features are added to the embedded player, this size remaining may change!*

Samples, are not just simple 'wave' files with this application. A "Descriptor" is required, which has all of the requisite data required to actually play
the sound, such as sample rate at root note, loop points, loop types etc. Each Descriptor however does NOT require a unique wave! This means, a single 'wave'
can have multiple descriptors, allowing you to do some tricks, such as having 1 key that plays a sample looped whereas another key plays it unlooped. This is
especially useful if you have a drum loop and wish to break it up into parts. Samples may be played straight (no loop), with a forwards loop, and with a 
back-and-forth loop mode.

## Using the samples in your synthesizer

If you do NOT have shapescratch enabled on any descriptors, the shape knob will allow you to tune the sound +/- 1 octave. **Note!** By default, the shape value is at minimum on startup, so your sample may sound too low at first - simply turn the shape knob to the 'middle' 12 o'clock position to centre the tuning. If you DO have shapescratch enabled, then you will need to press SHIFT-SHAPE to perform the tuning.


Descriptors as well provide overall amplitude adjustment and ADSR type amplitude envelopes to the sound as well - this is run pre-synthesizer envelope, so the
synthesizer's envelope will still apply.


## General Usage

### File menu

**Load WAV** : Load a .wav file into the editor
 - Currently 8 bit and 16 bit uncompressed .wav file formats are supported. ADPCM, 24/32 bit .wavs are not supported

*Maximum file sample rate supported is 65535hz. It is suggested overal to work with files at a lower (32k etc) sample rate, however this is not an explicit requirement.*

**Load RAW** : Load raw 8 bit binary data into the editor. Below this is a dropdown to pick if this is signed or unsigned data.
If you are using this format, you will likely know the data source. However, if you do not, try "Unsigned". If the result
is massively distorted, it was likely signed - try to reload using "Signed".

Loading waveforms brings up a new window where you will see the properties of the waveform (size, # of channels etc), and allows you a few options:

- Left, Right or ALL Channels : If the file is NOT MONO e.g. (multichannel), you can select to use just the "Left" (first) channel, the "Right(second channel), or Mix All Channels. Mix All will mix all of the channels found in the wav file. This is typically 2, but if there were more they would be mixed together as well.
- Normalize Amplitude : Amplifies the audio (if neccesary) to utilize the maximum dynamic range, if required.
- Remove DC Bias - removes any wasteful DC bias from the source data, if required. 

This window will tell you how much the sample is reduced by to fit the remaining memory (if at all) - by default enough memory is saved for ONE sample descriptor (required to be able to play the sound), however you can specify to save more room for more descriptors if you like via the "Leave room for # of descriptors" entry. 

*In addition you can force-shrink the sample by 0-99% (e.g. if you know you need to save room for another sample)*

__*It is important to note, that the re-sample method used here to shrink the samples is currently relatively simple - any DAW or external editor would likely do a MUCH better job at this, so for best results it's suggested you pre-size your samples.*__

The overall "Shrinkage" amount is displayed, and you will be warned if the shrinkage amount is too high (resulting in a very low quality sound)

Pressing "Import Sample" will import the waveform with the specified parameters into the Wave Editor ONLY. As 'typically' you will require a basic Descriptor to be able to actually 'use' the sample, a button "Import sample AND create basic setup" is provided, which will not only load the waveform, but it will "Add" the wave to the wave list, AND generate a generic descriptor that would let you generate output that you could use right away.(*This is effectively the same as pressing Import, Add Wave and Generate Descriptor immediately after importing*)

**Save/Load project** : The overall state of the application (excluding anything in the Wave Editor) may be saved and loaded using these two items. Note - this is NOT the same as the 'resultant' prlgunit/mnlgxdunit file - the generated files for your synthesizer are 'distilled' versions of the data, that cannot be extracted into 'exactly' the same data that was used.

**New** : Clears all loaded data and starts over from scratch.


### Edit Menu


**Trim Wave start** - Middle-clicking on the waveform allows you to specify an offset to the start of the waveform. 
If you are using this feature to offset the waveform with data that you will NOT play from or loop to with any descriptors, you can use this item to actually 'remove' this unwanted data - saving memory as a result.


**Trim Wave End** - If no descriptors are referencing any data beyond the end point (as specified with a right click), select this item to actually 'remove' this unwanted data - also saving memory as a result.

Add Wave : Loading files only loads them into the waveform 'editor' - to commit them into memory, you must select this item. *This performs the same function as the "Add Wave" button on the editor page*.

**Update Wave** : If you wish to edit a wave (i.e. trim the start/end), after clicking on it and performing your edits, you may select this item to replace the selected wave with the one in the editor.

**Remove Wave** : remove the waveform from the list - *currently you will need to update descriptors pointing to this wave, and any other descriptors that were referencing other waves after this one*

### Audio I/O:
Starts and stops the audio interface - currently the audio interface is started automatically on application startup. In the future this will be updated to provide audio interface selection capability - currently only Windows WAveOut is supported. The audio interface is not required for the application to perform, it is merely to allow you to listen / audition to the waveforms.


# The Wave Editor

Loading a waveform with the application via the "Import Sample" button, will *only* load the sample into the "Wave Editor". It **must** be 'added' to the Wave List before you can actually use it. The Wave Editor allows you to trim any unncessary data from the beginning or the end. In addition, the Wave Editor allows you to visually play with the Sample Start Offset (middle-click and drag), End Point (right click) and Loop Point (left click). Note, you *CAN* set the loop point to 'before' the start by setting the Loop Point before dragging the Start Offset. At any time you may press the "Audition" button to hear the sample, and if you wish you can right-click the "Audition" button to hold the keep playing the sample. Feel free to adjust the loop / end points while this is playing, if the Loop Mode has a loop mode selected. If you change the loop mode, you will currently have to re-audition the sample (re-press the audition button) to hear the new loop type.


Once you've finished with the wave editor, if the wave you are working with is NOT in the wave list, you will have to press "Add Wave" to add this to the Wave List. 

*Note, you may right-click the wave-list to select a waveform to delete it. Any assigned descriptors will be set to NO_WAVE_NUM and any further descriptors will be adjusted to match the new wave numbers.*

Otherwise, press "Generate Descriptor" to generate a Descriptor that uses the parameters set in the Wave Editor.


# The Descriptor Editor

Descriptors, are what define the parameters for "What to play, How, and Where". That is, what Wave Number to use, Where in that wave to play it and for how long, 
How to play it (loop modes, envelopes, shape scratch), and Where - the keyboard assignments.

Descriptors are best edited using the Wave Editor, where you can press "Generate Descriptor" to generate one that uses the sample type / loop properties / root note rate in the Wave Editor, however there is no reason you cannot edit the values yourself.

*Note you may right click the descriptor list to delete a descriptor*

### Descriptor Properties:

**Wave Num** : Which wave # (starting at 0) to use for this sample.

**Sample Rate At Root Note** : The playback rate to use when the Root Note is played. This will define the overall tuning of the sample. if you enable the "Play Reference Tone" and select an appropriate note to play, this can help you fine-tune the sample playback rate. To fine-tune this value, click and hold in this box, and move the mouse up and down. Useful with the Reference Note enabled.

**Start Offset** : Data offset where to start playing the wave from.

**Sample Length** : # of data points (samples) to play from the Start Offset

**Loop Type** : The type of looping to use (No Loop, Forwards, Back+Forward)


**Loop Free Run** : If looping is enabled, this instructs the player to never 'restart' a looped sample - playback will always continue 'where it left off'. Note - currently user oscillators 'freeze' once the synthesizer's amplitude EG reaches zero, however this still can be helpful to prevent the "machine-gun" type sound that can be associated with repeated sample playback. Useful for looped drum cymbal/hat sounds.

**Loop Sub Amount**: The # of data points to *SUBTRACT* from the current data position once the End Point has been reached. This is how you're allowed to have a sample 'start' in the middle but loop the entire sample. E.g. if your sample contained a total of 8000 points, and your Start Offset was 4000, Sample Length was 4000, but your Loop Sub was 8000, you would effectively have an 8000 point sample, that
is started from the middle. Useful for breaking up drum patterns. *Ideally this is set automatically via the wave editor*

**Scale Amount:** *Currently not fully supported by the player*. This will eventually allow you to specify the pitch-tracking level from 0 to 2 (in decimal), however currently, what it CAN do, is a value of "0" will force the synthesizer to play the sample at a FIXED pitch (cannot even be detuned by the shape knob), and a value of "1" is normal operation where both pitch tracking and tuning are permitted.

#### Amplitude Envelopes:
Before the synthesizer's amplitude envelope, you can have a separate amplitude envelope on the sample alone. Sample volume/envelopes are processed internally at 32 bits despite using 8 bit data sources. Adjust the Attack, Decay, Sustain Level and Release rates by clicking on the relevant 'knob',and while holding the mouse move it up or down to adjust. 

**Volume** : Sets the overall sample volume. This too is calculated internally as 32 bits despite the sample currently being an 8 bit data source.

**Shape Scratch**: This one, if selected, will cause **NO SOUND** to be generated **UNLESS** you hold a key within the specified range - **and** move the shape knob. The sample is mapped 'around' the shape knob, which 
will allow you to generate 'scratching' type sounds by turning the Shape Knob on your synthesizer. **Note! the Shape Knob was *NOT* designed with this in mind. Use of this will likely contribute to premature
wear of the Shape Knob! Do so at your own risk!**

**Playable Range** : This specifies what range on the keyboard this descriptor applies to. Left click to set the MINIMUM value, right click to set the MAXIMUM value. You will hear the sample play at the selected note when you do this. *Overlapping descriptors are  NOT supported!* If there is an overlap, the keys affected will be shown in RED. If this occurs, the "first" matching descriptor will be played. You cannot 'mix' samples this way. It would a: likely use up too much CPU time on the synthesizer, and b: *any* additions to the player code reduce the amount of memory left for samples!

**Root Note** :  This specifies at which key the sample will be played back at the "Sample Rate At Root Note" rate. You will hear the sample being played back at the Root Note rate when this is clicked. If you want your sample to be played back at a 'lower' octave, select a Root Note in a 'high' octave.




# Output Generator:

This page allows you to actually generate the *.mnlgxdunit or *.prlgunit file for your minilogue XD or prologue synthesizers. It is here where you can sepcify the User Oscillator Name (up to 6 characters that will be displayed on your synthesizer). Select the appropriate format, and press the Generate Result button. You should be greeted with a dialog indicating "Success..." - or any errors that are encountered (e.g. no sample data loaded / no descriptors etc)







*All product names, trademarks and registered trademarks are property of their respective owners.*
