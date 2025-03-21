![povray_kotlinscript_sphere_shell006_2](../images/povray_kotlinscript_sphere_shell006_2.png)

All code snippets in Kotlin/.kts

Simple vector class:

```
data class Vector(val x: Float, val y: Float, val z: Float) {
    constructor(x: Double, y: Double, z: Double) : this(x.toFloat(), y.toFloat(), z.toFloat())

    override fun toString(): String {
        return "<$x, $y, $z>"
    }

    fun distanceTo(other: Vector): Float{
        return sqrt( (x - other.x).pow(2) + (y - other.y).pow(2) + (z - other.z).pow(2))
    }
}
```

Random vector in normalised space:

```
fun randomSpherePoint(m: Float): Vector {
    var x = Random.nextDouble(-1.0, 1.0)
    var y = Random.nextDouble(-1.0, 1.0)
    var z = Random.nextDouble(-1.0, 1.0)
    val mag = sqrt(x * x + y * y + z * z)
    x /= mag
    y /= mag
    z /= mag
    val c = cbrt(Math.random())
    return Vector(x * c * m, y * c * m, z * c * m)
}
```

Simple brute-force sphere packing:

```
val spheres = mutableListOf<Sphere>()

while(spheres.size < 300){
    val sphere = Sphere(randomSpherePoint(10f), Random.nextInt(5, 50)/10.0f, Texture(Pigment.white(), Finish.default()))

    var collision = false
    if(sphere.vector.distanceTo(oSphere.vector) < (sphere.radius + oSphere.radius) || sphere.vector.distanceTo(sunSphere.vector) < (sphere.radius + sunSphere.radius)){
        collision = true
    }else {
        spheres.forEach { other ->
            if (sphere.vector.distanceTo(other.vector) < (sphere.radius + other.radius)) {
                collision = true
            }
        }

        if(!collision) {
            spheres.add(sphere)
        }
    }
}
```