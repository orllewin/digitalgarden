<iframe 				
src="https://orllewin.github.io/coracle/drawings/algorithms/parametric_circle/"
width="450"
height="450"
scrolling="no"></iframe>

```kotlin
import coracle.Drawing
import kotlin.math.cos
import kotlin.math.sin

class CircleDrawing: Drawing() {

    var segmentCount = 80
    var step = 0
    var plotRadius = 0f

    override fun setup() {
        size(450, 450)
        plotRadius  = (width * .75f)/2
        noStroke()
    }

    override fun draw() {
        translate(width/2, height/2)

        val angle = (step * TWO_PI / segmentCount).toFloat()

        fill(0xdcaa88)
        circle(cos(angle) * plotRadius, sin(angle) * plotRadius, 7)

        if (step == segmentCount) step = -1
        step++

        translate(0, 0)
        fill(0xdeefff, 0.10f)
        rect(0, 0, width, height)
    }
}
    
```