<iframe 				
src="https://orllewin.github.io/coracle/drawings/ports/attractor/"
width="800"
height="450"
scrolling="no"></iframe>

Ported to Coracle from [www.dwitter.net/d/12129](https://www.dwitter.net/d/12129)

```kotlin

import coracle.Colour
import coracle.Drawing
import kotlin.math.cos
import kotlin.math.sin

class Attractor: Drawing() {

    private val bg = Colour(0x1d1d1d)
    var X = 0.0
    var Y = 0.0

    sealed class Mode{
        object Realtime: Mode()
        object Ghost: Mode()
    }

    val mode: Mode = Mode.Realtime
    override fun setup() {
        size(900, 550)

        when(mode){
            Mode.Realtime -> stroke(0xffffff)
            Mode.Ghost -> stroke(0xffffff, 0.110)
        }

        background(0x000000)
    }
    override fun draw() {
        if(mode == Mode.Realtime) background(bg)

        val f = frame/100.0
        for (i in 0..15000 ){
            X = X * cos(4 * (f / 2 ) + Y * 2) + Y / 4
            Y = cos(X+Y+X+f/3) - sin(i+X+f) / 4
            point( (width/2) + (X * (width/2.5)), (height/2) + (Y * (height/3)))
        }
    }
}
    
```