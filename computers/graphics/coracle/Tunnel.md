<iframe 				
src="https://orllewin.github.io/coracle/drawings/ports/tunnel/"
width="550"
height="550"
scrolling="no"></iframe>

Ported to Coracle from [www.dwitter.net/d/26555](https://www.dwitter.net/d/26555)

```kotlin

import coracle.Drawing
import coracle.Math.map
import kotlin.math.cos
import kotlin.math.sin
import kotlin.math.tan

class Tunnel: Drawing() {

    val bg = 0x1d1d1d
    override fun setup() {
        size(550, 550)
        noStroke()
        fill(0xffffff)
    }

    override fun draw() {
        background(bg)
        for(i in 800 downTo 0){
            val T = frame * 2
            val z = width * 39/(i - (T.toFloat() % 1))
            val a = i + T - (T.toFloat() % 1)
            val x = sin(a) * z + (sin(a/(width.toFloat()) * 1.1) + 2) * 200
            val y = cos(a.toDouble()) *z+(sin(a/(width.toFloat()) / 1.1 ) + 2) * 100
            circle(x - width/4, y, width/(i))
        }
    }
}
    
```
