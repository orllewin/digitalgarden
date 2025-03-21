<iframe 				
src="https://orllewin.github.io/coracle/drawings/algorithms/spatial_hash/"
width="600"
height="600"
scrolling="no"></iframe>

Simple (simpler than a [Quadtree](https://orllewin.github.io/coracle/drawings/algorithms/quadtree/)) method to reduce collisions checks. Only circles in the same cell are checked for collisions (or neighbouring cells too if the circle straddles more than one, there's occasional glitches in that scenario which I may or may not find time to investigate).  
  
This [Circle Packing](https://en.wikipedia.org/wiki/Circle_packing) problem is a little different to the usual implementation; starting with large circles, exhausting available space, then working down to a minimum radius. Here they instead start with a radius of 1 and grow until they collide with a neighbour or reach their `maxRad` size.  
  
```kotlin
import coracle.*
import coracle.shapes.Circle
import coracle.shapes.Rect
import kotlin.math.cos
import kotlin.math.sin
import kotlin.math.sqrt

class SpatialHashCirclePacking: Drawing() {

    private lateinit var spatialHash: SpatialHash
    private var maxRad = randomInt(8, 20)

    private val backgroundColour = 0xF2EDF2
    private val gridColour = 0xC3BDD4
    private val defaultColour = 0xA87CAA
    private val boundaryColour = 0xC57596

    override fun setup() {
        size(600, 600)

        val columns = randomInt(3, 8)
        val rows = randomInt(3, 8)
        spatialHash = SpatialHash(columns, rows)
        print("Columns: $columns Rows: $rows Maximum radius: $maxRad")
    }

    override fun draw() {
        background(backgroundColour)

        spatialHash
            .addMultiple(20)
            .grow()
            .checkCells()
            .draw()
    }

    private fun coordWithinCircle(): Coord {
        val a = random(0f, 1f) * TWO_PI
        val r = ((width/2) * 0.8) * sqrt(random(0f, 1f))
        val x = r * cos(a)
        val y = r * sin(a)
        return Coord(width/2 + x.toFloat(), height/2 + y.toFloat())
    }

    inner class GrowingCircle(x: Float, y: Float, radius: Int): Circle(x, y, radius){
        var growing = true
        var colour = defaultColour

        fun draw(){
            noStroke()
            fill(colour, 1f)
            circle(x, y, r)
        }
    }

    inner class SpatialHash(private val columns: Int, private val rows: Int){

        private val cellPopulations = HashMap>()
        private val cellNeighbours =  HashMap>()
        private val cellsRects =      HashMap()
        private val pendingUpdates = mutableListOf>()
        private val cellWidth: Int
        private val cellHeight: Int

        init {
            repeat(columns * rows){ index ->
                cellPopulations[index] = mutableListOf()
                cellNeighbours[index] = mutableListOf()
            }

            cellWidth = width/columns
            cellHeight = height/rows

            var index = -1
            repeat(rows){ row ->
                repeat(columns){ column ->
                    index++
                    val rect = Rect(column * cellWidth, row * cellHeight, cellWidth, cellHeight)
                    cellPopulations[index] = mutableListOf()
                    cellsRects[index] = rect
                }
            }
        }

        fun addMultiple(count: Int): SpatialHash{
            repeat(count){
                val randCoord = coordWithinCircle()
                add(GrowingCircle(randCoord.x, randCoord.y, 1))
            }
            return this
        }

        fun add(c: GrowingCircle): SpatialHash{
            val index = getIndexHash(c.x.toInt(), c.y.toInt())
            var collision = false

            val population = cellPopulations[index]
            population?.forEach { e ->
                if(collision(c, e)) collision = true
            }

            val neighbours = cellNeighbours[index]
            neighbours?.forEach { e ->
                if(collision(c, e)) collision = true
            }

            if(!collision) population?.add(c)

            return this
        }

        fun grow(): SpatialHash {
            cellPopulations.forEach { cellCollection ->
                val circles = cellCollection.value
                val neighbours = cellNeighbours[cellCollection.key]
                circles.forEach { c ->
                    if(c.growing && c.r < maxRad){
                        c.r++
                        if(collision(c, circles) || collision(c, neighbours ?: emptyList())){
                            c.r--
                            c.growing = false
                        }
                    }else{
                        c.growing = false
                    }
                }
            }

            return this
        }

        private fun collision(c: Circle, circles: List
```