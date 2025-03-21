![P_20231211_144627](images/P_20231211_144627.jpg)

Modernising Ever Ready/Exide bike lights from the 70s/80s. These lights were a feature of any Gen-X-ers childhood, they used two large 'D cell' R20 batteries and a very dim PR527 0.42 amp bulb which wasn't much good for actually being seen ([see nostalgia article on road.cc](https://road.cc/content/feature/did-you-use-ever-ready-bike-lights-cycling-268225)).

[Updated video with front light too](https://www.youtube.com/shorts/s2od27oFNTo)

![](https://www.youtube.com/watch?v=YEcX-L15yCM)

I kept the case, replaced the bulb and reflector with a modern LED ring and driver board, and instead of the old D Cell batteries there's now a LiPo battery with a charging board connected to a USB surface mount port, and a switch.

![P_20231211_144622](images/P_20231211_144622.jpg)

![P_20231211_144612](images/P_20231211_144612.jpg)

## Parts and Process

There was quite a bit of soldering involved, and some tactical destruction of the inside cavity to make room for everything.

- [Sparkfun LumiDrive](https://www.sparkfun.com/products/14779) - the brains/LED driver board. Can be programmed via the Arduino IDE but I ran into countless issues and settled for the Circuit Python/MicroPython environment instead. [LumiDrive Hookup Guide](https://learn.sparkfun.com/tutorials/lumidrive-hookup-guide)
- [SparkFun LuMini LED Ring - 2 Inch (40 LEDs)](https://www.sparkfun.com/products/14966)
- [LiPo Amigo](https://shop.pimoroni.com/products/lipo-amigo) - LiPo battery charger
- [LiPo Battery Pack – 1200mAh](https://shop.pimoroni.com/products/lipo-battery-pack?variant=20429082183)
- [JST PH 2-Pin Cable - Female Connector 150mm](https://thepihut.com/products/jst-ph-2-pin-cable-female-connector-150mm) - if you use these there's a couple less solder points to worry about...
- USB Panel Mount - from eBay, there's all kinds.
- Switch - again from eBay

The USB charge port was too big inside the light case but I'd already drilled the hole so kept it even though I found some smaller parts later. A little thought needed to be given to the wiring, I obviously wanted to be able to charge the battery without having the lights on: the USB port was connected internally to the LiPo charger, some soldered connections were then made to the LumiDrive with the switch added to the circuit. The LumiDrive actually supports LiPo charging but I didn't spend any time investigating if I could manage this project without the external charge board. The LuMini LED ring had solder pads and my soldering skills are poor but I managed it. To fit it all in the plastic casing I removed the silver plastic reflector and metal battery fittings, then used a soldering iron to melt away some innards that were getting in the way (no doubt producing some pretty toxic fumes - open a window!). The LED ring was attached inside the red light lens using a glue gun. Circular holes in the case were made using a 'Step Hole Drill' bit - that's the search term you need.

## Sourcecode - Rear Light

The source is just an edit of the [demo code](https://github.com/sparkfun/SparkFun_LumiDrive_Example_Code/blob/master/main.py):

```

import adafruit_dotstar
import digitalio
import board
import math
import time

led = digitalio.DigitalInOut(board.D13)
led.direction = digitalio.Direction.OUTPUT

num_pixels = 60
brightness = 0.32

pixels = adafruit_dotstar.DotStar(board.SCK, board.MOSI, 
    num_pixels, brightness=brightness, auto_write=False)

BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

def travel(color, wait):
    num_pixels = len(pixels)
    for pos in range(num_pixels):
        pixels[pos] = color 
        pixels.show() 
        time.sleep(wait)
        
def color_fill(color, wait):
    pixels.fill(color)
    pixels.show()
    time.sleep(wait)

while True: 
    travel(WHITE, 0)
    color_fill(BLACK,0) 
    
```