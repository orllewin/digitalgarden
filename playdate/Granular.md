## Granular is archived

There wont be any future development of Granular, it's still available on itch.io but you can now achieve the same output with more control in [ModularPlay](ModularPlay.md) (well, coming very soon), that's where all future Playdate audio dev efforts will be concentrated. The main screen of Granular was fantastic but the effects sections were always a little poor UX-wise., ModularPlay offers much more.

![granular_banner](images/granular_banner.png)

Itch.io page: [orllewin.itch.io/granular](https://orllewin.itch.io/granular)

Granular is an experimental 'grain sampler' for [Playdate](https://play.date) that plays subsamples from a recorded parent sample, it's inspired by the old [Reaktor](https://www.native-instruments.com/en/products/komplete/synths/reaktor-6/) 'Travelizer' instrument. Granular includes various modifiable parameters and effects to change the sound which produces beautiful glitchy ambience that can be used on its own or as a background for other instruments.  
  
**You really need headphones or to connect to a speaker** - this will not sound good through the Playdate's tiny built-in speaker.  
  
You don't have rigid control over the playback, most options not related to effects/filters increase or decrease the probability of something happening, either a change in playback rate, or how regular the samples are triggered, it's all fairly random.  
  
Generally 'A' selects and navigates, 'B' closes and pops the stack back to the previous location, everything should be intuitive with a little exploration. Granular operates on probabilities; when you select 'jump' or 'rev.' (reverse) or other parameters you're telling the underlying engine to allow the possibility of that happening each cycle.

## Sample output

Sample tracks, one with the built-in [Organelle M](https://www.critterandguitari.com/organelle) recording, and the other sampling a piano: [orllewin.bandcamp.com/album/playdate-granular-demos](https://orllewin.bandcamp.com/album/playdate-granular-demos)

## First Run/Home

After downloading the zip from Itch and [side-loading onto your Playdate](https://help.play.date/games/sideloading/) you're met with an introduction screen where you can either start playing with the built-in demo files or record your own sample. Once you've recorded a sample on subsequent runs Granular will jump straight to the live playback screen.

![pd_granular2_001](images/pd_granular2_001.png)

## Live Playback View

![pd_granular2_002](images/pd_granular2_002.png)

The five horizontal bars show the current subsample position and size within the parent sample/recording. There are five buttons across the bottom of the screen which launch the filter and effects screens for each channel/subsample. The top of the screen has four options:

- **Global**: controls some aspects of the current session (default octave, sample trigger probability, and a global delay effect).
- **Record**: launches a screen to record a new parent sample, this will be saved as recording.pda and will be loaded by default when Granular starts.
- **Open**: open a previously recorded (and saved) parent sample, including the demo files that come with Granular.
- **Save**: when you record the sample is saved as recording.pda, if you want to keep your recording save it here with a filename so you can load it again later. If you record a new sample the current recording.pda will be lost otherwise.

## Playback and Filters

![pd_granular2_effects_screen_1](images/pd_granular2_effects_screen_1.png)

The first effects screen controls some playback parameters of the selected subsample:

- **Position**: adjust the subsample position within the parent recording. Note. this control is redundant if you have Jump or Drift active.
- **Jump**: set the percentage chance of the subsample moving to a new random position each update. Lower values give more repetition, higher values are more hectic.
- **Reverse**: set the percentage chance of the subsample playing in reverse.
- **ADSR View**  (None interactive view, a visualisation of the Attack, Decay, Sustain, Release envelope options below.)

![pd_granular_adsr_view](images/pd_granular_adsr_view.png)

- **Attack**: sets the attack for the sample playback.
- **Decay**: sets the decay for the sample playback.
- **Sustain**: sets the sustain for the sample playback.
- **Release**: sets the release for the sample playback.
- **The sine octave views** : Four switches/toggles that control which octaves the subsample will play in. One will always be greyed-out, this is the global/default octave (see Global menu below) that the sample will _mostly_ play at. If any other of the switches are active the sample will occasionally (randomly) play back at that octave. The sine-wave graphic is just a space-saving way of getting across which of the four octaves they represent, bigger wavelength is a lower octave, higher frequency is a higher octave.

![pd_granular_sine_switches](images/pd_granular_sine_switches.png)

- **Size**: the size of subsample. Smaller is more grainy, bigger will include hooks and fragments from the parent.
- **Drift**: subsample drifts left or right through the parent, wrapping to the other side when it reaches the start or end. Not useful if you have a high Jump value set.
- **More**: onwards to the effects.

## Effects

![pd_granular2_effects_screen_2](images/pd_granular2_effects_screen_2.png)

The second subsample screen lets you customise the subsample sound, be careful with these, there's no clipping or limiter, I've not done any damage to anything yet but I'm not ruling out the possibility:

- **Low Pass Switch**: switches the low pass filter on or off.
- **Low Pass Frequency**: low pass frequency...
- **Low Pass Resonance**: low pass resonance...
- **High Pass Switch**: switches the high pass filter on or off.
- **High Pass Frequency**: high pass frequency...
- **High Pass Resonance**: high pass resonance...
- **Delay Length**: sets the delay length up to a max of 2.5 seconds
- **Delay feedback**: how often the delay repeats, higher values can create nice textures
- **Delay level**: the delay is always on, but its default mix level is 0, turning this dial fully on creates a 50/50 wet/dry mix
- **Overdrive**: the overdrive is always on, but its default mix level is 0, turning this dial fully on creates a 50/50 wet/dry mix
- **Overdrive Gain**: amplify the sample
- **Bitcrusher**: the bitcrusher is always on, but its default mix level is 0, turning this dial fully on creates a 50/50 wet/dry mix
- **Bitcrusher Amount**: how much to destroy the sample quality by. Higher values do not sound good, but with subtle use you can get a nice distortion with this effect
- **Bitcrusher Undersampling**: reduces the sample rate, very useful, can create a nice distortion

## Global

![pd_granular2_global_menu](images/pd_granular2_global_menu.png)

The 'global' menu allows you to set a base playback rate, the default is 0.5 which plays samples at 1/2 speed/pitch (the octaves are listed as 1 to 4 following the Midi convention: 4 being the original rate, 3 is half speed/pitch, 2 is 1/4, and 1 is 1/8 which is really too low for most recordings).

The Global menu also has a delay effect for the main channel and a 'tempo' setting, the lower the tempo the less chance one of the five subsamples has of triggering, when set to 0 the remaining operations will complete which can be used to gently finish playback.

---

Source (private): [github.com/orllewin/playdate_granular](https://github.com/orllewin/playdate_granular)