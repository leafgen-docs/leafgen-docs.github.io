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
