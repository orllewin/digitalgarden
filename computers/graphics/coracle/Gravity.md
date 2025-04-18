<iframe 				
src="https://orllewin.github.io/coracle/drawings/vector/gravity/"
width="450"
height="450"
scrolling="no"></iframe>

```kotlin
import coracle.*

class Gravity: Drawing() {

    private var maxSpeed = 2.2f
    private val bodies = mutableListOf()
    private lateinit var blackHole: Vector

    private var bodyColour = 0x000000
    private var backgroundColour = 0xf5f2f0

    private var frame = 0

    override fun setup() {
        size(450, 450)
        blackHole = Vector(width/2, height/2)
        repeat(random(3, 7).toInt()) { bodies.add(Boid()) }
    }

    override fun draw() {
        background(backgroundColour)
        noStroke()
        frame++

        bodies.forEach { body -> body.update().draw() }

        circle(width/2, height/2, 10)
    }

    inner class Boid{
        private var velocity = Vector(0f, 0f)
        private var location: Vector = Vector.randomPosition(width, height)

        var tailLength = randomInt(30, 60)
        var tailX = FloatArray(tailLength)
        var tailY = FloatArray(tailLength)

        fun update(): Boid {
            var blackholeDirection = blackHole - location
            blackholeDirection.normalize()
            blackholeDirection *= 0.06f

            velocity += blackholeDirection
            location += velocity

            val tailIndex = frame % tailLength
            tailX[tailIndex] = location.x
            tailY[tailIndex] = location.y

            bodies.forEach { body ->
                if(body != this){
                    var bodyDirection = body.location - location
                    bodyDirection.normalize()
                    bodyDirection *= 0.01f

                    velocity += bodyDirection

                    velocity.limit(maxSpeed)
                    location += velocity
                }
            }

            return this
        }

        fun draw(){
            repeat(tailLength){ tailIndex ->
                stroke(bodyColour, 0.4f)
                point(tailX[tailIndex], tailY[tailIndex])

                if(tailIndex == tailLength - 1){
                    noStroke()
                    fill(bodyColour)
                    circle(location.x, location.y, 4)
                }
            }
        }
    }
}
    
```