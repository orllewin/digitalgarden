![chuck_tibetan_bowl_ambience](audio/chuck_tibetan_bowl_ambience.mp3)

```
//Bowed ambience
BandedWG tibetanBowl => JCRev reverb => Gain gain => dac;

3 => tibetanBowl.preset;
gain => Gain feedback => DelayL delay => gain;

1.1::second => delay.max;
1.1::second => delay.delay;
0.6 => feedback.gain;
0.95 => delay.gain;
0.75 => reverb.mix;

12 => int rootNote;//C
[0, 4, 7, 9] @=> int scale[];

while(true){ 

    Math.random2f(0.5, 1.1)::second => delay.delay; 
      
    scale[Math.random2(0, scale.size()-1)] => int note;
    (Math.random2(1, 3) * 12) => int octave;
    rootNote + octave + note => int midiNote;
    
    //<<< "Midi note: ", midiNote >>>;
    midiNote => Std.mtof => tibetanBowl.freq;
    
    Math.random2f(0.0, 1.0) => tibetanBowl.bowRate;
    Math.random2f(0.0, 0.7) => tibetanBowl.bowPressure;
    Math.random2f(0.0, 1.0) => tibetanBowl.strikePosition;
    Math.random2f(0.1, 0.6) => tibetanBowl.startBowing;

    Math.random2(3250,8750)::ms => now;
    Math.random2f(0.1, 0.6) => tibetanBowl.stopBowing;
    Math.random2(4000, 9500)::ms => now;
}
```