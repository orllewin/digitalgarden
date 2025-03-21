<iframe 				
src="https://orllewin.github.io/coracle/drawings/experiments/circle_packing/"
width="450"
height="450"
scrolling="no"></iframe>

[Circle Packing](https://en.wikipedia.org/wiki/Circle_packing) drawing, includes methods for determining if two circles intersect and selecting a random point within a circle.

```kotlin
import coracle.*
import coracle.shapes.Point
import kotlin.math.cos
import kotlin.math.sin
import kotlin.math.sqrt

class CirclePackingDrawing: Drawing() {

    val worldColour = 0xf5f2f0
    val boundsColour = 0xE5E2E0
    val black = 0x000000

    val circles = mutableListOf<Circle>()
    
	override fun setup() {
        size(450, 450)
    }

    override fun draw() {
        background(worldColour)

        fill(boundsColour)
        noStroke()
        circle(width/2, height/2, 200)

        val point = coordWithinCircle()
        val rndCircle = Circle(point.x.toInt(), point.y.toInt(), 1)
        if(!collision(rndCircle)) circles.add(rndCircle)

        noFill()
        stroke(black)
        circles.forEach { circle -> circle.grow().draw() }

        circles.sortByDescending { c ->
            c.r
        }
    }

    private fun coordWithinCircle(): Point {
        val a = random(0f, 1f) * TWO_PI
        val r = 200f * sqrt(random(0f, 1f))
        val x = r * cos(a)
        val y = r * sin(a)
        return Point(width/2 + x.toFloat(), height/2 + y.toFloat())
    }

    fun collision(circle: Circle): Boolean{
        circles.forEach { other ->
            if(other != circle && circle intersects other) return true
        }

        return false
    }

    inner class Circle(x: Int, y: Int, var r: Int){

        var growing = true
        val location = Vector(x, y)

        fun grow(): Circle {
            if(!growing) return this
            r++
            if(r > 15) growing = false
            if(collision(this)) growing = false
            return this
        }

        infix fun intersects(other: Circle): Boolean = location.distance(other.location) <= r + other.r

        fun draw() = circle(location.x, location.y, r)
    }
}
```