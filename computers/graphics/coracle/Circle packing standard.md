<iframe 				
src="https://orllewin.github.io/coracle/drawings/circle_packing/standard/"
width="450"
height="450"
scrolling="no"></iframe>

The common circle packing implementation: start at max circle size, exhaust available space, reduce radius, repeat

```kotlin
import coracle.Drawing
import coracle.TWO_PI
import coracle.Vector
import coracle.random
import coracle.shapes.Point
import kotlin.math.cos
import kotlin.math.sin
import kotlin.math.sqrt

class StandardCirclePacking: Drawing() {

    val worldColour = 0xf5f2f0
    val boundsColour = 0xE5E2E0
    val black = 0x000000

    val circles = mutableListOf
```

