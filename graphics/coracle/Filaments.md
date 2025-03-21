<iframe 				
src="https://orllewin.github.io/coracle/drawings/ports/filaments/"
width="800"
height="450"
scrolling="no"></iframe>

Ported to Coracle from [www.dwitter.net/d/26504](https://www.dwitter.net/d/26504)

```kotlin

import coracle.Color
import coracle.Colour
import coracle.Drawing
import coracle.randomInt
import kotlin.math.cos
import kotlin.math.sin

class Filaments: Drawing() {

    private val bg = Colour(0x1d1d1d)

    override fun setup() {
        size(800, 450)
        stroke(0xffffff)
    }

    override fun draw() {
        background(bg)

        var Z = -1.00
        var y = -1.00

        for (i in -1000..1000 step 1){
            val ii = i.toFloat()/1000.00
            val X= ii * 2.5 + sin(Z/52.0) / 5.0
            y = 5 * cos(ii * y - 4)
            Z = 8 + y * sin(3*ii+(frame/100.0)) * 40.0
            point((width/2) + (width/6)*X, (height/2) + Z+y)

            y = 4 * cos(ii * y - 4)
            Z = 8 + y * sin(3*ii+(frame/100.0)) * 40.0
            point((width/2) + (width/6)*X, (height/2) + Z+y)

            y = 5 * sin(ii * y - 4)
            Z = 8 + y * cos(3*ii+(frame/100.0)) * 40.0
            point((width/2) + (width/6)*X, (height/2) + Z+y)

            y = 4 * sin(ii * y - 4)
            Z = 8 + y * cos(3*ii+(frame/100.0)) * 40.0
            point((width/2) + (width/6)*X, (height/2) + Z+y)
        }
    }
}
    
```