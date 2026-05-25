---
title: Additional information
layout: default
nav_order: 5
---

# Additional information

## Leaf base geometry

The leaf base geometry is composed of a collection of triangles that define the leaf surface. The assumptions for the base geometry are:

1. The start point of the leaf (where the petiole connects) is in the origin `[0 0 0]`.
2. The y-direction `[0 1 0]` is considered as the direction of the leaf (the petiole also points in this direction).
3. The z-direction `[0 0 1]` is considered as the direction of the leaf normal. If the leaf has a three-dimensional structure, the mean surface normal should be pointing in the z-direction.

The leaf base geometry does not need to have any specific surface area, as the base geometry is scaled automatically to follow the LSD.

## Petiole direction distribution

On QSM-based methods the directions of the petioles originating from a cylinder can be controlled with the petiole direction distribution. This is a distribution which defines the probability of petiole directions with respect to four attributes of a cylinder: length, radius, inclination angle, and azimuth angle. The petiole direction has values on the interval between 0 and $2\pi$, where the values 0 and $2\pi$ correspond to the upmost radial direction on the cylinder and the angle increases clock-wise when the cylinder is viewed along the direction of the cylinder axis (i.e. by the right hand rule with respect to cylinder axis). If the cylinder is vertical, north direction is set as the "upmost" reference direction. The petiole direction distribution is defined as a function handle with five variables, which are, in order: 1) petiole direction angle, 2) cylinder length, 3) cylinder radius, 4) cylinder inclination angle, and 5) cylinder azimuth angle. For example, the simple distribution

```matlab
    petioleDirDist = @(x,len,rad,inc,az) sin(x).^2;
```

promotes petiole directions on the sides of the cylinder, leaving the top and bottom less crowded. The cylinder attributes (length, radius, inclination, and azimuth) are not taken into account in this example distribution.


## Compound leaves

The base geometry of the leaves can be set to contain specific patterns for all of the three foliage generation methods. The leaf base geometry definition can contain any leaf pattern consisting of triangles in three-dimensional space.

When intersection prevention is used in the generation process, the number of triangles in the base geometry increases the computational cost exponentially. Therefore, complicated base geometry, like compound leaves consisting of multiple leaflets, can become infeasible to generate. One workaround for this is to define a simpler leaf base geometry that covers the outer dimensions of the detailed base geometry pattern, do the leaf generation and intersection prevention process using this base geometry, and afterwards changing the leaf base geometry to the original detailed base geometry. In this case, to achieve desired total leaf area, one needs to calculate the ratio of surface area between the simplified and detailed base geometry and scale the total leaf area to be generated accordingly.

## Phyllotaxis on QSM cylinders

A phyllotaxis pattern for the leaf positioning on QSM cylinders can be defined as an additional input, the `Phyllotaxis` struct. Since this requires cylinders as the base, this approach works only on the QSM direct method and the leaf cylinder library method. The inclusion of `Phyllotaxis` struct overrides the uniform sampling of petiole directions and start points on the cylinders, and instead, uses the user-supplied phyllotaxis parameters.

Branch phyllotaxis patterns are defined in terms of nodes, points on the branch where a petiole of a leaf or multiple leaves originate. Minimum required definitions for phyllotaxis are the number of leaves originating from a single node, the distance between successive nodes on the branch, and the angle of separation between petioles of the leaves originating from the node. Optionally, user can also define the radial direction of the first leaf petiole of the first node on the cylinder and the angle of rotation of petiole directions on successive nodes.

The contents of the `Phyllotaxis` struct are:

| Field name                     | Description                                               | Type   |
|--------------------------------|-----------------------------------------------------------|--------|
| `nNodeLeaves`                  | Number of leaves originating from a single node           | double |
| `nodeDistance`                 | Distance between successive nodes                         | double |
| `petioleSeparationAngle`       | Radial angle between leaf petioles of a single node       | double |
| `nodeRotation`                 | Radial angle between the orientations of successive nodes | double |
| `initialRadialAngle`           | Radial reference angle of the first node                  | double |

## Exporting

The parameters of the foliage generated by LeafGen are stored in a `LeafModelTriangle` class object. If we have such class variable called `Leaves`, the explicit 3D geometry can be exported into an obj file using the method `export_geometry` of the class:

```matlab
precision = 5;
Leaves.export_geometry('OBJ',true,'leaves_export.obj',precision);
```

This creates an obj file "leaves_export.obj" with all the vertices and faces of the triangles included in the generated foliage, with the precision of five decimals for the floating point numbers in the file (as defined with the `precision` parameter).

Instead of exporting the explicit 3D geometry, it is also possible to export only the transformation parameters of the leaf positions, scalings, and orientation. This is done with the `export_leaf_transforms` method of the class:

```matlab
precision = 5;
Leaves.export_leaf_transforms('leaf_transforms.txt', precision)
```

This creates a text file where each row contains the parameters of one leaf: the index of the leaf, the start position, the scaling along each axis, and the rotation matrix for the correct orientstion. These parameters can be used to transform the base leaf geometry to match the generated foliage. The contents of the file look something like this:

```
# Leaf transforms (position, scaling, rotation)
# id px py pz sx sy sz r11 r12 r13 r21 r22 r23 r31 r32 r33
1 79.86567 14.81574 1107.96790  1.28694  1.28694  1.28694  0.90490  0.34573  0.24824 -0.42541  0.71632  0.55309  0.01341 -0.60610  0.79528 
2 81.22222 14.71250 1103.61047  1.33267  1.33267  1.33267 -0.42003 -0.90497 -0.06781  0.79138 -0.40183  0.46071 -0.44418  0.13985  0.88496 
3 80.62014 14.28959 1105.12195  1.10094  1.10094  1.10094  0.12389  0.77510  0.61957 -0.59716 -0.44043  0.67039  0.79250 -0.45304  0.40829 
...
```
That is, one row contains the index, position vector, scaling vector, and the rotation matrix split into three row vectors. The trasformation of the leaf base geometry (which has the start point at [0 0 0] the leaf tip direction as [0 1 0] and the surfrace normal direction as [0 0 1]) to represent each leaf of the generated foliage can be achieved by scaling the base geometry according to the vector of scale factors, rotating the base geometry vertex points with the rotation matrix, and translating the leaf according to the position vector.

{: .note }
Exporting the transforms into txt file does not include the information of the leaf base geometry, only the transforms. For the use in downstream applications, the user can either save the geometry separately, or replace the triangle geomtery used in LeafGen with something more complicated, e.g., giving a more realistic 3D model of a leaf or needle shoot for radiative transfer modelling.