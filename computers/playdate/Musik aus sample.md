![mas_main_banner](images/mas_main_banner.jpg)

## About

MAS is a sample player/live drum machine for [Playdate](https://play.date). Works standalone or with an Android device running the partner app (Note. Android integration makes use of an [undocumented Playdate serial API](https://publish.obsidian.md/orllewin/computers/playdate/Serial+Comms) which Panic could disable at any time).

Itch.io page: [orllewin.itch.io/music-aus-sample](https://orllewin.itch.io/music-aus-sample)

![](https://www.youtube.com/watch?v=u_oSNGsQhjo)

## Main Controls

When you select a patch from the chooser MAS will be in edit mode, in this mode you use the direction pad to move around the grid, B changes the sample mode (hit/play/loop), A button triggers playback. If you move the crank to the live position all the Playdate buttons become sample triggers if they've been assigned (in the patch JSON) to a sample. It's a difficult feature to describe but intuitive when you're using MAS.

**When using a patch with buttons assigned to trigger samples you move between 'Live' and 'Edit/Manual' mode by moving the crank: 0 - 180 degrees are edit/manual mode, 180 - 360 are live where all 6 input buttons can trigger samples (depending on the patch set-up)**

## Android

Free companion app to trigger samples from a connected Android device:

Android .apk: [musik_aus_sample_0_0_1.apk](https://orllewin.github.io/playdate/musik_aus_sample/android/musik_aus_sample_0_0_1.apk)

# User Patches

Add custom patches to `/Disk/Data/orllewin.playdate.musicaussample/UserPatches`. Custom patches must be in their own directory and include a config.json file.

### Demo User Patches

- [MusicBox.zip](https://orllewin.github.io/playdate/musik_aus_sample/patches/MusicBox.zip)
- [MusicBoxAccompaniment.zip](https://orllewin.github.io/playdate/musik_aus_sample/patches/MusicBoxAccompaniment.zip)

### Make a patch

The patch JSON has a couple of config fields and an array of sample objects, the array size must equal columns x rows.

```

{
  "label": "Default Demo Patch",
  "columns": 4,
  "rows": 2,
  "samples": [
    ...
  ]
}
  
```

#### Sample Object

```

{
  "title": "Kick Drum",
  "path": "/Samples/Kick/Kick 12",
  "type": "sample",
  "mode": "hit",
  "image": "/Images/down",
  "key": "down",
  "volume": 0.75,
  "rate": 1
}
```

- `title`: String. Not truncated so keep it short
- `path`: String, path to the sample, either within the .pda directory or a user patch
- `type`: String: see below. files are streamed from disk. Use sample for anything but long audio files
- `mode`: String, the initial mode, can be one of hit, play, loop - see below for specs for each
- `image`: String optional - path to an image in the .pda directory
- `key`: String optional - one of left, right, up, down, a, b
- `volume`: Float optional - 0.0 to 1.0, default is 1.0
- `rate`: Float optional - playback rate in range 0.0 to 1.0, default is 1.0

#### Type

- `empty`: make the ui cell a spacer/placeholder, can still have text and/or an image
- `sample`: stored in memory so use for drum hits and short loops
- `file`: audio streamed from disk, use for longer files and background music
- `transition`: plays a file while loading in new player, see below

#### Mode

- `play`: plays once, a second tap while playing stops, good for longer samples
- `hit`: plays once, a second tap while playing restarts, good for drum hits
- `loop`: loops forever, a second tap should stop playback

#### Transition

A transition is a special type that allows you to build up a full set with different sample sets/patches. When triggered an optional sample can be played once while the ui for the new patch is loaded, any other playing samples are stopped immediately, mode is ignored for this type.

```

{
  "title": "GOTO SET2",
  "path": "/UserPatches/set1/interlude",
  "type": "transition",
  "image": "/UserPatch/set1/transition_image",
  "next": "/UserPatches/set2/"
}
```

#### Wav Conversion

Audio needs to be converted to Playdates own .pda format before you can use it in custom patches:

- First prepare your samples in [ADPCM .wav format](https://sdk.play.date/1.13.7/Inside%20Playdate.html#M-sound-prep)
- [Install the Playdate SDK](https://play.date/dev/), you'll need to use the pdc command-line Playdate compiler that comes with it
- Add the pdc compiler to your path
- Put your ADPCM .wav files in a directory together with an empty file named `main.lua`
- Compile the fake Playdate game: `pdc MySampleSet/`
- This will create `MySampleSet.pdx` which although a compiled Playdate game is just a directory. View the contents (MACOS: right click then: 'Show Package Contents') and move the newly converted .pda files to your patch directory

# USB

MAS can be controlled by an Android device connected via USB running [musik_aus_sample_0_0_1.apk](https://orllewin.github.io/playdate/musik_aus_sample/android/musik_aus_sample_0_0_1.apk). This is an experimental feature and could break if Panic turn off the USB serial `eval` method in a future Playdate OS release.

# Built-in samples

#### Image Attribution

Most of the images have been removed but some of the following remain, all from [thenounproject.com/coquet_adrien/](https://thenounproject.com/coquet_adrien/):

`jellyfish.png, frequency.png, sine.png, rain_cloud.png, snow_cloud.png, censored.png, heartbeat.png, bomb.png, recycle.png, octopus.png, snail.png, happy_jellyfish1.png, happy_jellyfish2.png, coronavirus.png, joking.png, tease.png, laugh.png, concentrate.png, pirate.png, grasshopper.png, creature.png`

#### Sample Attribution

The vast majority of audio samples come from MusicRadar: [musicradar.com/news/tech/free-music-samples-royalty-free-loops-hits-and-multis-to-download](https://www.musicradar.com/news/tech/free-music-samples-royalty-free-loops-hits-and-multis-to-download)

#### Full list of built-in samples

You can user these in your patches or convert your own wavs and add them to a patch directory.

```

Samples/Kick/Kick 05
Samples/Kick/Kick 11
Samples/Kick/Kick 10
Samples/Kick/Kick 04
Samples/Kick/Kick 12
Samples/Kick/Kick 06
Samples/Kick/Kick 07
Samples/Kick/Kick 13
Samples/Kick/Kick 17
Samples/Kick/Kick 03
Samples/Kick/Kick 02
Samples/Kick/Kick 16
Samples/Kick/Kick 14
Samples/Kick/Kick 15
Samples/Kick/Kick 01
Samples/Kick/Kick 18
Samples/Kick/Kick 24
Samples/Kick/Kick 25
Samples/Kick/Kick 19
Samples/Kick/Kick 22
Samples/Kick/Kick 23
Samples/Kick/Kick 21
Samples/Kick/Kick 09
Samples/Kick/Kick 08
Samples/Kick/Kick 20
Samples/Chords2/chords2_1
Samples/Chords2/chords2_2
Samples/Chords2/chords2_3
Samples/Chords2/chords2_4
Samples/Snare/Snare 02
Samples/Snare/Snare 16
Samples/Snare/Snare 17
Samples/Snare/Snare 03
Samples/Snare/Snare 01
Samples/Snare/Snare 14
Samples/Snare/Snare 10
Samples/Snare/Snare 04
Samples/Snare/Snare 05
Samples/Snare/Snare 11
Samples/Snare/Snare 07
Samples/Snare/Snare 13
Samples/Snare/Snare 12
Samples/Snare/Snare 06
Samples/Snare/Snare 23
Samples/Snare/Snare 22
Samples/Snare/Snare 20
Samples/Snare/Snare 08
Samples/Snare/Snare 09
Samples/Snare/Snare 21
Samples/Snare/Snare 19
Samples/Snare/Snare 25
Samples/Snare/Snare 24
Samples/Snare/Snare 18
Samples/Perc/Perc 11
Samples/Perc/Perc 05
Samples/Perc/Perc 04
Samples/Perc/Perc 10
Samples/Perc/Perc 06
Samples/Perc/Perc 12
Samples/Perc/Perc 13
Samples/Perc/Perc 07
Samples/Perc/Perc 03
Samples/Perc/Perc 17
Samples/Perc/Perc 16
Samples/Perc/Perc 02
Samples/Perc/Perc 14
Samples/Perc/Perc 01
Samples/Perc/Perc 15
Samples/Perc/Perc 18
Samples/Perc/Perc 19
Samples/Perc/Perc 21
Samples/Perc/Perc 09
Samples/Perc/Perc 08
Samples/Perc/Perc 20
Samples/Default/cb_beat_loop_07_120bpm
Samples/Default/snare_13
Samples/Default/broken_loop_03
Samples/Default/perc_04
Samples/Default/tape_loop_17
Samples/Default/tape_loop_03
Samples/Default/broken_loop_04
Samples/Default/tape_loop_08
Samples/Default/tape_loop_20
Samples/Default/kick_18
Samples/Default/perc_08
Samples/Default/kick_21
Samples/Default/snare_24
Samples/EffectLoops/tape_loop_1
Samples/EffectLoops/tape_loop_2
Samples/EffectLoops/tape_loop_3
Samples/EffectLoops/tape_loop_4
Samples/EffectLoops/tape_loop_5
Samples/EffectLoops/varispeed_loop_2
Samples/EffectLoops/varispeed_loop_3
Samples/EffectLoops/varispeed_loop_1
Samples/EffectLoops/cassete_loop_5
Samples/EffectLoops/cassete_loop_4
Samples/EffectLoops/cassete_loop_3
Samples/EffectLoops/cassete_loop_2
Samples/EffectLoops/cassete_loop_1
Samples/Clap/Clap 09
Samples/Clap/Clap 08
Samples/Clap/Clap 03
Samples/Clap/Clap 02
Samples/Clap/Clap 14
Samples/Clap/Clap 01
Samples/Clap/Clap 05
Samples/Clap/Clap 11
Samples/Clap/Clap 10
Samples/Clap/Clap 04
Samples/Clap/Clap 12
Samples/Clap/Clap 06
Samples/Clap/Clap 07
Samples/Clap/Clap 13
Samples/Transistion/001
Samples/Transistion/003
Samples/Transistion/002
Samples/Transistion/006
Samples/Transistion/007
Samples/Transistion/005
Samples/Transistion/004
Samples/Cymbal/Cymbal 22
Samples/Cymbal/Cymbal 23
Samples/Cymbal/Cymbal 21
Samples/Cymbal/Cymbal 09
Samples/Cymbal/Cymbal 08
Samples/Cymbal/Cymbal 20
Samples/Cymbal/Cymbal 18
Samples/Cymbal/Cymbal 19
Samples/Cymbal/Cymbal 03
Samples/Cymbal/Cymbal 17
Samples/Cymbal/Cymbal 16
Samples/Cymbal/Cymbal 02
Samples/Cymbal/Cymbal 14
Samples/Cymbal/Cymbal 01
Samples/Cymbal/Cymbal 15
Samples/Cymbal/Cymbal 11
Samples/Cymbal/Cymbal 05
Samples/Cymbal/Cymbal 04
Samples/Cymbal/Cymbal 10
Samples/Cymbal/Cymbal 06
Samples/Cymbal/Cymbal 12
Samples/Cymbal/Cymbal 13
Samples/Cymbal/Cymbal 07
Samples/Chords1/chords1_1
Samples/Chords1/chords1_3
Samples/Chords1/chords1_2
Samples/Chords1/chords1_4
Samples/Loops/128/tl_oxidebeat_128_01
Samples/Loops/128/tl_oxidebeat_128_03
Samples/Loops/128/tl_oxidebeat_128_02
Samples/Loops/128/tl_oxidebeat_128_06
Samples/Loops/128/tl_oxidebeat_128_05
Samples/Loops/128/tl_oxidebeat_128_04
Samples/Loops/85/TL_PolyAnaSyn_85_Am
Samples/Loops/85/TL_Drums_85_02
Samples/Loops/85/TL_ElecPiano_85_Am
Samples/Loops/85/BeatK01 85-04
Samples/Loops/85/BeatK01 85-01
Samples/Loops/85/BeatK01 85-03
Samples/Loops/85/BeatK01 85-02
Samples/Loops/85/TL_Mellotron_85_Em
Samples/Loops/85/TL_ElecPiano_85_Dm
Samples/Hat/Hat 16
Samples/Hat/Hat 02
Samples/Hat/Hat 03
Samples/Hat/Hat 17
Samples/Hat/Hat 01
Samples/Hat/Hat 15
Samples/Hat/Hat 14
Samples/Hat/Hat 04
Samples/Hat/Hat 10
Samples/Hat/Hat 11
Samples/Hat/Hat 05
Samples/Hat/Hat 13
Samples/Hat/Hat 07
Samples/Hat/Hat 06
Samples/Hat/Hat 12
Samples/Hat/Hat 23
Samples/Hat/Hat 22
Samples/Hat/Hat 08
Samples/Hat/Hat 20
Samples/Hat/Hat 21
Samples/Hat/Hat 09
Samples/Hat/Hat 25
Samples/Hat/Hat 19
Samples/Hat/Hat 18
Samples/Hat/Hat 24
Samples/Vinyl/Fluff
Samples/Vinyl/30s
Samples/Vinyl/OneNote
Samples/Vinyl/BlibbBlobb
Samples/Vinyl/UmErr
Samples/Vinyl/CrackleBeat2
Samples/Vinyl/ShotStuff
Samples/Vinyl/Bwardsbee
Samples/Effects/white_noise
Samples/Effects/noise_2
Samples/Effects/tape_stop_2
Samples/Effects/hum_2
Samples/Effects/tape_stop_1
Samples/Effects/hum_1
Samples/Effects/rewind
Samples/Effects/tape_start
Samples/Effects/noise
    
```