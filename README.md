# openscad-registry
A vcpkg-ce registry for OpenSCAD

This is currently collecting some test cases for using vcpkg-ce with
OpenSCAD.

## How to use
1. Install [vcpkg-ce](https://github.com/microsoft/vcpkg-ce). **Note**: vcpkg-ce is
   still in preview status itself, so if you find *any* issues, please report
   those in the [OpenSCAD Package Manager Discussion](https://github.com/openscad/openscad/issues/3479)
   not in the vcpkg-ce repository.
   
2. Create an `environment.yaml` file describing the dependencies of your project, e.g.
   using the smooth-primitives library to create a button.

```
info:
  name: smooth-button
  version: 0.0.1

registries:
  - kind: artifact
    location: https://github.com/openscad/openscad-registry/archive/refs/heads/main.zip
    name: openscad
    
requires:
  openscad:libraries/smooth-primitives: "*"
```

3. Create the scad file, smooth-button.scad

```
use <smooth-primitives/smooth_prim.scad>

$fa = 4; $fs = 0.4;

dia = 22;
height = 3;
holes = 2;

difference() {
    SmoothCylinder(dia, height, height / 2);
    for (y = [-1, 1], x = [-1, 1])
        translate(2 * holes * [x, y])
            cylinder(3 * height, r = holes, center = true);
}
```

4. Activate the environment, which will download the dependencies

```
$ ce activate
```

5. Run openscad (which the updated `OPENSCADPATH` environment variable set to find the library)
```
$ openscad smooth-button.scad
```
