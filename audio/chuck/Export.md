Can't remember where I found this, some ChucK forum post, but launch/spork this before any other shreds and it'll record output to your root directory. Audio export from the file menu in miniAudicle seems broken, at least on MacOS, but this is reliable:

```
dac => WvOut2 waveOut => blackhole;
"chuck-session" => waveOut.autoPrefix;
"special:auto" => waveOut.wavFilename;
<<<"writing to file: ", waveOut.filename()>>>;
.5 => waveOut.fileGain;
null @=> waveOut;
while( true ) 1::second => now;
```
