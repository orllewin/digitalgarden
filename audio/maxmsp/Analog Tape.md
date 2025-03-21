I want to treat a long audio file with a tape warbling/wow detune effect, but I don't want to use some off-the-shelf software because where's the fun in that? I've also been resisting buying a [Chase Bliss Generation Loss](https://www.chasebliss.com/generation-loss-mkii) because I realised I was getting Gear Acquisition Syndrome.

I found this [thread for Audacity](https://forum.audacityteam.org/t/accurate-tape-effect/96459/4)with a [Nyquist](https://en.wikipedia.org/wiki/Nyquist_(programming_language)) script which works pretty well. Load a file, go to `Tools` > `Nyquist prompt` which works well but I want more control to vary parameters over time, copying here because it's useful regardless:
```lisp
(setf depth 0.01)
(setf rpm 22)

; convert to Hz
(setf speed (/ rpm 60.0))

(defun gen-vardelay (hz depth)
  (setf max-variance (/ hz 2.0))  ; Max variation of speed in Hz
  (setf variance (sound-srate-abs hz (noise)))
  (setf variance (mult variance max-variance))
  (setf variance (force-srate *sound-srate* variance))
  (sum depth (mult depth (hzosc (sum hz variance)))))


(let ((vardelay (gen-vardelay speed depth)))
  (multichan-expand #'tapv *track* 0 vardelay (* 2 depth)))
```

For Max/MSP I found [this thread](https://cycling74.com/forums/tape-flutter) which has a fantastic simple patch from Andrew Benson but other than the pitch variation it doesn't sound like a detuning tape. I do have a Norns Shield with a beautiful [Tapedeck](https://llllllll.co/t/tapedeck/51919) patch, but that's SuperCollider.

Further searching led to [this video](https://www.youtube.com/watch?v=UIp1EVlH-LY)- which is perfect, adding to the [Infiniverb](buffer_loops/Infiniverb.md) patch and loading a Geophone recording of a wind turbine ([Flower Scar Hill wind turbine](../field_recordings/yorkshire/todmorden/Flower%20Scar%20Hill%20wind%20turbine.md)) gives:

![turbine_subloop_tape](audio/turbine_subloop_tape.mp3)

Next: to deconstruct that `tape` patch and figure out how it works, [download it here](https://orllewin.github.io/maxmsp/patches/tape.maxpat).
