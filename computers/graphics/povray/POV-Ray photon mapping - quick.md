![povray_kotlinscript_sphere_shell006 1](../images/povray_kotlinscript_sphere_shell006%201.png)

It turns out you can drastically increase the speed of a photon mapping render by turning object reflections off:

```
sphere {
  <-3.3828278, 1.3612305, 6.065379>, 0.6
  texture{
    pigment {color rgb <1.0, 1.0, 1.0>}
    finish {
	  ambient 0.075
	  diffuse 0.9
      reflection 0.9
	}
  }
  photons { target reflection off }
}
```

The square artefact in the scattered media in image above shows the downside of using a box for a media container, I'll use a sphere in future scenes:

![povray_kotlinscript_sphere_shell006 2](../images/povray_kotlinscript_sphere_shell006%202.png)