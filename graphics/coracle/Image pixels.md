<iframe 				
src="https://orllewin.github.io/coracle/drawings/images/image_pixels/"
width="450"
height="450"
scrolling="no"></iframe>

Move mouse around on the image to display the pixel colour. A histogram is displayed across the bottom.

```kotlin
import coracle.Colour
import coracle.Drawing
import coracle.Image
import coracle.Math.map

class ImagePixels: Drawing() {

    lateinit var testImage: Image

    var pixelsLoaded = false

    var histogram = IntArray(256){ 0 }

    var frame = 0

    override fun setup() {
        size(450, 450)
        testImage = loadImage("input_image")
        interactiveMode()
        stroke(0xffffff)
    }

    override fun draw() {
        image(testImage, 0, 0, width, height)
        frame++
        if(pixelsLoaded){
            //Draw histogram
            stroke(0xffffff)
            repeat(255) { index ->
                val x = map(index.toFloat(), 0f, 255f, 0f, width.toFloat()).toInt()
                val maxValue = histogram.maxOrNull() ?: 0
                val v = map(histogram[index].toFloat(), 0f, maxValue.toFloat(), 0f, height.toFloat()).toInt()
                line(x, height, x, height - v)
            }

            //Live pixel colour
            pixel(mouseX, mouseY)?.let{ pixel ->
                val pixelColour = Colour(pixel)
                fill(pixelColour)
                circle(mouseX, mouseY, 20)
            }
        }else if(frame == 50){
            //50 frame delay to ensure image has loaded
            loadPixels()
            repeat(width){ i ->
                repeat(height){ j ->
                    pixel(i, j)?.let{ pixel ->
                        val brightness = Colour(pixel).brightness()
                        histogram[brightness]++
                    }
                }
            }

            pixelsLoaded = true
        }
    }
}
  
```