---
title: Canopy hull method
parent: Basic functionality
layout: default
nav_order: 3
---

# Canopy hull method

## Function call

Foliage is generated on a point cloud by calling the function `generate_foliage_canopy_hull`.  The format of the basic function call is
```matlab
[Leaves,aShape] = generate_foliage_canopy_hull(pCloud,TargetDistributions, ...
    LeafProperties,totalLeafArea);
```
where `pointCloud` is a matrix containing point cloud of a tree, `TargetDistributions` is a struct containing target distribution types and parameters or parameter functions for LADD, LOD, and LSD, `LeafProperties` contains leaf base geometry and twig length information, and `totalLeafArea` is the total leaf area to be generated. The function outputs a leaf object `Leaves`, which contains information of the generated leaves, and alpha shape object `aShape`, which contains the information on the point cloud and the created alpha shape.

## Inputs

### Point cloud

The input `pCloud` is a matrix of xyz-values describing the point cloud coordinates in 3D space.


### Target distributions

The required fields for the `TargetDistributions` struct are:

|Field name       |Description                                               |Type           |
|:----------------|:---------------------------------------------------------|:--------------|
|`dTypeLADDh`     |LADD relative height distribution type                    |string         |
|`dTypeLADDd`     |LADD relative distance along subbranch distribution type  |string         |
|`dTypeLADDc`     |LADD compass direction distribution type                  |string         |
|`pLADDh`        |LADD heightwise distribution parameters                   |double         |
|`pLADDd`        |LADD distancewise distribution parameters                 |double         |
|`pLADDc`        |LADD directionwise distribution parameters                |double         |
|`dTypeLODinc`    |LOD inclination angle distribution type                   |string         |
|`dTypeLODaz`     |LOD azimuth angle distribution type                       |string         |
|`fun_pLODinc` |Function for LOD inclination angle distribution parameters|function handle|
|`fun_pLODaz`  |Function for LOD azimuth angle distribution parameters    |function handle|
|`dTypeLSD`       |LSD distribution type                                     |string         |
|`fun_pLSD`|Function for LSD distribution parameters                  |function handle|


### Leaf properties

The required fields for the `LeafProperties` struct are:

|Field name           |Description|Type|
|:--------------------|:---------------------------------------------------|:-----|
|`vertices`           |Vertice coordinates for leaf base geometry triangles|double|
|`triangles`          |Indicators for the edges of each triangle           |double|

{: .note } 
In LeafGen, a single leaf is considered to have only one surface normal, which is in the direction of vertical axis (z-axis) of the leaf base geometry. Therefore, for 3D base geometries, the geometry should be defined such that the average normal has vertical direction.


### Total leaf area

`totalLeafArea` contains the target total leaf area for the foliage.

## Optional inputs

Optional inputs can be given after the required inputs as name-value pairs. Input called `OptInput` with value `optValue` is given as:
```matlab
[Leaves,aShape] = generate_foliage_canopy_hull(pCloud,TargetDistributions, ...
    LeafProperties,totalLeafArea,'OptInput',optValue);
```
The optional input options are:

| Name                     | Description                                                   | Type    |
|:-------------------------|:--------------------------------------------------------------|:--------|
| `Alpha`                  | Alpha radius for the alpha shape generation                   | double  |
| `StemCoordinates`        | Coordinates for tree stem                                     | double  |
| `PCSamplingWeights`      | Heightwise weights for using point cloud in position sampling | double  |
| `IntersectionPrevention` | Intersection prevention flag                                  | boolean |

### Alpha radius

A non-negative double precision value to define the alpha radius for the creation of alpha shape. The default is 1 m.

### Stem coordinates

A matrix containing coordinates for the tree stem location or the assumed center axis of the tree. This is used in calculating the relative distance from stem to alpha shape edge and in defining the compass direction. Rows of the matrix correspond to the stem coordinates in ascending order, such that the first row corresponds to tree trunk start position and the last row is the highest point of the assumed stem. Default stem location is the positive z-axis from origin to the highest z-value of the point cloud points.

### Point cloud sampling weights

A vector containing the heightwise weighing factors for using point cloud in leaf position sampling. The tree is automatically segmented into equidistant heightwise intervals corresponding to the number of elements in the vector. The first element defines the weight of the lowest height interval and the last element the highest. Default value is 0, i.e., point cloud is not used directly in position sampling.

### Intersection prevention

A boolean value for turning intersection prevention of leaves off. Intersection prevention is computationally the heaviest part of the leaf generation, but assures more realistic foliage. If this is not needed, intersection prevention can be turned off. By default intersection prevention is on.

## Outputs

### Leaves

The output `Leaves` is a `LeafModelTriangle`-class object, which contains information of the generated foliage and functions for visualization. The contents of the class are:

| Name                  | Description                            | Type   |
|:----------------------|:---------------------------------------|:-------|
| `base_vertices`       | Base geometry vertice points           | double |
| `base_dimensions`     | Base geometry outer dimensions         | double |
| `base_triangles`      | Base geometry triangle definitions     | double |
| `base_area`           | Base geometry surface area             | double |
| `triangle_count`      | Number of triangles in base geometry   | double |
| `leaf_count`          | Total number of leaves                 | double |
| `leaf_area`           | Surface areas of leaves                | double |
| `leaf_start_point`    | Start points of leaves                 | double |
| `leaf_scale`          | Scale factors of leaves                | double |
| `leaf_direction`      | Tip direction vectors of leaves        | double |
| `leaf_normal`         | Normal vectors of leaves               | double |

### Alpha shape

The output `aShape` is an `alphaShape`-class object, which contains information of the alpha shape and the point cloud. More infromation can be found from [alphaShape] documentation.

[alphaShape]: https://www.mathworks.com/help/matlab/ref/alphashape.html