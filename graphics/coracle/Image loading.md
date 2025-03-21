<iframe 				
src="https://orllewin.github.io/coracle/drawings/basic/images/"
width="450"
height="550"
scrolling="no"></iframe>

```kotlin
import coracle.Drawing
import coracle.Image

class BasicImage: Drawing() {

    lateinit var testImage: Image

    var x = 0

    override fun setup() {
        size(450, 550)

        x = width

        testImage = loadImage("input_image")
    }

    override fun draw() {
        image(testImage, x, 0)
        x--

        image(testImage, 0, 275, width, height/2)

        if(x < 0) noLoop()
    }
}
  
```