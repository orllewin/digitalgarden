<iframe 				
src="https://orllewin.github.io/coracle/drawings/basic/hello_coracle/"
width="450"
height="450"
scrolling="no"></iframe>

```kotlin
import coracle.Drawing

class HelloCoracle: Drawing() {

    val worldColour = 0xf5f2f0
    val foregroundColour = 0x000000
    var x = 0

    override fun setup() {
        size(450, 450)
        if(isAndroid()) strokeWeight(5)
    }

    override fun draw() {
        background(worldColour)
        stroke(foregroundColour, 0.75f)

        var y = 0
        var d = 3

        while(y < height){
            var x = 0

            while(x < width){
                line(x, y, x, y + d)
                x += d
            }

            y += d
            d++
        }

        noLoop()
    }
}
  
```

