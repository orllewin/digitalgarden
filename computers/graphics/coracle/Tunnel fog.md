<iframe 				
src="https://orllewin.github.io/coracle/drawings/experiments/tunnel/"
width="800"
height="750"
scrolling="no"></iframe>

```kotlin
import coracle.Color
import coracle.Drawing
import kotlin.math.cos
import kotlin.math.sin

class TunnelDrawing: Drawing() {

  var z = 0f; var a = 0f; var x = 0f; var y = 0f

  override fun setup() {
    size(800, 750)
    noStroke()
  }

  override fun draw() {

    for(i in 510 downTo 0){
      z = width * 35f/i
      a = i * 2f + frame * 2f
      x = sin(a) * z + (sin(a/width * 1.2f) + 2) * 200
      y = cos(a) * z + (sin(a/width / 1.2f) + 2) * 200
      val c = Color.grey(i/3)
      fill(c)
      if(i == 510) background(c)
      circle(x, y, z * 0.32)
    }
  }
}
  
```
