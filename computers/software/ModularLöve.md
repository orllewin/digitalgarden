## Platform

LÖVE/Lua.

## About

An open source (but not FSF approved license) experiment porting [ModularPlay](../playdate/ModularPlay.md) to the desktop using [LÖVE](https://love2d.org/). Started as part of the 2023 December Adventure ModularLöve shaped up pretty quickly and is archived on Github: [github.com/orllewin/love2d_modular_love](https://github.com/orllewin/love2d_modular_love)

![](https://www.youtube.com/watch?v=CUIyjFUM8Vc)

LÖVE is a fantastic framework for building a ui but the built-in audio engine is a little limited, so I don't think I'll ever turn this into a comprehensive bit of software. The main blocker is the built-in effects are always applied to the audio source, you can't chain effects. So in the video below the grey delay pedal isn't delaying the blue chorus, and the orange distortion isn't distorting the delay or chorus, etc etc. See the [setEffect API notes](https://love2d.org/wiki/love.audio.setEffect):

> Audio produced by effects are added on top of the normal dry sound from Sources.

![](https://www.youtube.com/watch?v=9qmXLPM0WuU)

## Status

Recent. May get more development, may not.