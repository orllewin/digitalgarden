<iframe 				
src="https://orllewin.github.io/coracle/drawings/experiments/3d_triangles_and_projection/"
width="450"
height="450"
scrolling="no"></iframe>

A Coracle implementation of the [Code-It-Yourself! 3D Graphics Engine Part #1 tutorial](https://youtu.be/ih20l3pJoeU)

```kotlin
import coracle.*
import coracle.shapes.Mesh

/*
    Code-It-Yourself! 3D Graphics Engine Part #1 - Triangles & Projection
    Adapted from: https://github.com/OneLoneCoder/videos/blob/master/OneLoneCoder_olcEngine3D_Part1.cpp
    See: https://youtu.be/ih20l3pJoeU
    */
class TrianglesAndProjection: Drawing() {

    lateinit var cube: Mesh

    var projectionMatrix = Matrix.projectionMatrix(450f / 450f, 90.0f, 0.1f, 1000.0f)
    var matRotZ = Matrix()
    var matRotX = Matrix()

    var frame = 0

    override fun setup() {
        size(450, 450)

        cube = Mesh.cube()

        noFill()
        stroke(0xffffff)
    }

    override fun draw() {
        background(0x1d1d1d)
        frame++

        val f = frame/20.0f

        matRotZ.rotateZ(f)
        matRotX.rotateX(f * 0.5f)

        cube.triangles.forEach { triangle ->

            //Rotate
            val rotatedTriangle = triangle * matRotZ
            rotatedTriangle *= matRotX
            rotatedTriangle.applyZOffset(3f)

            //Project to 2D
            val tri2D = rotatedTriangle.to2D(projectionMatrix)
            tri2D.scale(width/2f)
            tri2D.translate(width/2f, height/2f)
            tri2D.draw()
        }
    }
}
    
```
