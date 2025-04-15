A new version of [Coracle](Coracle.md) but non-portable, targeting Android Compose only. Since Coracle was first written Compose has become the default user interface toolkit for Android, Coracle compose aims to keep the original simple syntax intact by hiding the Compose implementation details.

<small>An example Coracle compose drawing:</small>
```
class TestDrawing: Drawing() {  
  
    override fun draw(drawScope: DrawScope) {  
        super.draw(drawScope)  
        background(Color.Gray)  
        stroke(Color.Green)  
        line(0, random(height), width, random(height))  
    }  
}
```

<small>The above test drawing displayed in a Compose screen:</small>
```
Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->  
    Column(  
        modifier = Modifier.padding(innerPadding)  
    ){  
        CoracleCanvas(  
            modifier = Modifier.fillMaxSize(),  
            drawing = TestDrawing()  
        )  
    }  
}
```

## Status

Under active development.

## Demo drawing

<iframe width="800" height="467" src="https://www.youtube.com/embed/jKyWZl5kaIc" title="30 October 2024" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

```

package orllewin.coraclecompose.drawings  
  
import androidx.compose.ui.graphics.Color  
import androidx.compose.ui.graphics.drawscope.DrawScope  
import androidx.compose.ui.unit.dp  
import orllewin.coracle.Drawing  
import orllewin.coracle.Line  
import orllewin.coracle.Vector  
  
/**  
 * A port of https://orllewin.uk/graphics/coracle/Avoid+closest 
 */
 class AvoidClosest: Drawing() {  
  
    val worldColour = Color(0xff121212)  
    val nucleusColour = Color(0xFF222C2C)  
    val cellColour = Color(0x55c7e5f2)  
    val cellRadius = 80.dp  
    val nucleusRadius = 15.dp  
    val midpointRadius = 4.dp  
    private val cells = mutableListOf<Cell>()  
  
    override fun setup(width: Int, height: Int) {  
        super.setup(width, height)  
  
        repeat(100){  
            cells.add(Cell())  
        }  
    }  
  
    override fun draw(drawScope: DrawScope) {  
        super.draw(drawScope)  
        background(worldColour)  
  
        cells.forEach { boid ->  
            boid.update().draw()  
        }  
  
    }  
  
    inner class Cell{  
  
        private val maxSpeed = 1.5f  
        private var location = Vector.randomPosition(width, height)  
        private var velocity = Vector(0, 0)  
  
        fun update(): Cell {  
  
            var closestDistance = Float.MAX_VALUE  
            var closestIndex = -1  
  
            cells.forEachIndexed { index, other ->  
                if(other != this){  
                    val distance = location.distance(other.location)  
                    if(distance < closestDistance){  
                        closestIndex = index  
                        closestDistance = distance  
                    }  
                }  
            }  
  
            val closest = cells[closestIndex]  
  
            //While we've got the reference to closest cell, draw the relationship:  
            val line = Line(location.x, location.y, closest.location.x, closest.location.y)  
            val midpoint = line.midpoint()  
  
            stroke(Color.White)  
            line(line)  
  
            fill(Color.White)  
            circle(midpoint, midpointRadius.toPx())  
  
            var direction = location.direction(closest.location)  
            direction.normalize()  
            direction *= -0.2f  
  
            velocity += (direction)  
            velocity.limit(maxSpeed)  
            location += (velocity)  
  
            if (this.location.x > width + cellRadius.toPx()) this.location.x = -cellRadius.toPx()  
            if (this.location.x < -cellRadius.toPx()) this.location.x = width.toFloat() + cellRadius.toPx()  
            if (this.location.y > height + cellRadius.toPx()) this.location.y = -cellRadius.toPx()  
            if (this.location.y < -cellRadius.toPx()) this.location.y = height.toFloat() + cellRadius.toPx()  
  
            return this  
        }  
  
        fun draw(){  
            fill(cellColour)  
            circle(location.x, location.y, (cellRadius.toPx().toInt()/2))  
  
            fill(nucleusColour)  
            circle(location.x, location.y, nucleusRadius.toPx().toInt())  
        }  
    }  
}

```