<iframe 				
src="https://orllewin.github.io/coracle/drawings/experiments/snowflakes/"
width="500"
height="500"
scrolling="no"></iframe>

```kotlin
import coracle.*
import coracle.Math.map
import kotlin.math.cos
import kotlin.math.max

class SnowClassDrawing: Drawing() {

    private val bg = Colour(0x1d1d1d)
    private val white = Colour(0xffffff)
    private val snowflakes = mutableListOf()
    override fun setup() {
        size(500, 500)
        noStroke()
        repeat(1500){
            snowflakes.add(Snowflake())
        }
    }

    override fun draw() {
        background(bg)

        snowflakes.forEach { snowflake ->
            snowflake.update().draw()
        }
    }

    inner class Snowflake{

        private var position = Vector(randomInt(width), randomInt(height))
        private val z = random(1, 50)
        private val alpha = map(z, 1f, 50f, 0.2f, 1f)
        private val speed: Vector = Vector(random(-z/150f, z/150f), random(0.03f, max(0.04f, z/20f)))
        private var r = map(speed.y, 0.1f, 2f, 1f, 4f)

        fun update(): Snowflake{
            position += speed
            position.x = position.x + cos(frame/1f) / z
            if(position.x < r || position.x > width + r || position.y > height + r){
                position = Vector(randomInt(-width, width), randomInt(-(height + r)))
            }
            return this
        }

        fun draw(){
            fill(white, alpha)
            circle(position.x, position.y, r)
        }
    }
}
    
```