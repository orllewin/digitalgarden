<iframe 				
src="https://orllewin.github.io/coracle/drawings/experiments/3d_points/"
width="550"
height="550"
scrolling="no"></iframe>

```kotlin
import coracle.*
import coracle.Math.map
import kotlin.math.max

class Points3D: Drawing() {

    var projectionMatrix = Matrix.projectionMatrix(450f / 450f, 90.0f, 0.1f, 1000.0f)
    var matRotX = Matrix()
    var matRotY = Matrix()
    var matRotZ = Matrix()

    var pointCount = 1720
    var points = Array(pointCount){ Vector3D.random2()}

    var colour: Int = 0xffffff
    var bg: Int = 0x34393e
    override fun setup() {
        size(550, 550)
        noStroke()
        interactiveMode()
    }

    override fun draw() {
        background(bg)

        matRotX.rotateX(frame/60f)
        matRotY.rotateY(frame/120f)
        matRotZ.rotateZ(frame/240f)

        points.forEach{ point ->
            var rotatedPoint = point * matRotX
            rotatedPoint *= matRotY
            rotatedPoint *= matRotZ

            val z = (rotatedPoint.z + rotatedPoint.z)/2
            rotatedPoint.applyZOffset(max(1.5f, mouseX/100f))

            val point2D = rotatedPoint.to2D(projectionMatrix)
            point2D.scale(width/2f)
            point2D.translate(width/2f, height/2f)
            
            fill(colour, map(z, -1f, 1f, 0.6f, 0.01f))
            circle(point2D.x, point2D.y, map(z, -1f, 1f, 8f, 1f) * max(0.25f, map(mouseX.toFloat(), 0f, width.toFloat(), 1f, 0.01f)))
        }
    }
}
    
```