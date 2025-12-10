---
title: QSM direct method
parent: Basic functionality
layout: default
nav_order: 1
---

# QSM direct method


## Function call
Foliage is generated on a QSM by calling the function `generate_foliage_qsm_direct`.  The format of the basic function call is
```matlab
[Leaves,QSMbc] = generate_foliage_qsm_direct(QSM,TargetDistributions,...
    LeafProperties,totalLeafArea);
```
where `QSM` is a struct containing a quantitative structure model, `TargetDistributions` is a struct containing the target distribution types and parameters for the leaf distributions, `LeafProperties` is a struct containing leaf base geometry and twig length information, and `totalLeafArea` is a value for the total leaf area to be added on the tree model. The fuction provides a `LeafModelTriangle`-class object `Leaves` containing information of the generated leaves and a `QSMBCylindrical`-class object containing information on the QSM.


## Inputs

### QSM

LeafGen uses the QSM format produced by the [TreeQSM] method. Optionally, a QSM can be intialized by defining a struct containing the basic information of the cylinders, in which case the required fields for the `QSM` struct are:

| Field name       | Description              | Type    |
|:-----------------|:-------------------------|:--------|
| `cylinder.start` | Cylinder start points    | double  |
| `cylinder.axis`  | Cylinder axes            | double  |
| `cylinder.length`| Cylinder lengths         | double  |
| `cylinder.radius`| Cylinder radii           | double  |
| `cylinder.parent`| Cylinder parent cylinders| integer |
| `cylinder.branch`| Cylinder branch index    | integer |

{: .note }
In the`cylinder.branch` field, LeafGen assumes the stem cylinders of the QSM to have branch index 1, and the actual branches are indexed with sequential natural numbers starting from 2. It is crucial to correctly mark the stem cylinders with index 1, since during leaf generation, these stem cylinders are automatically left without leaves.

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
|`fun_pLDS`|Function for LSD distribution parameters                  |function handle|


### Leaf properties

The required fields for the `LeafProperties` struct are:

|Field name           |Description|Type|
|:--------------------|:---------------------------------------------------|:-----|
|`vertices`           |Vertice coordinates for leaf base geometry triangles|double|
|`triangles`          |Indicators for the edges of each triangle           |double|
|`petioleLengthLimits`|Leaf petiole length limits                          |double|

{: .note } 
In LeafGen, a single leaf is considered to have only one surface normal, which is in the direction of vertical axis (z-axis) of the leaf base geometry. Therefore, for 3D base geometries, the geometry should be defined such that the average normal has vertical direction.


### Total leaf area

`totalLeafArea` is a double-precision variable containing the target total leaf area for the foliage.

## Optional inputs

Optional inputs can be given after the required inputs as name-value pairs. Input called `OptInput` with value `optValue` is given as:
```matlab
Leaves = generate_foliage_qsm_direct(QSM,TargetDistributions,...
    LeafProperties,totalLeafArea,'OptInput',optValue);
```
The optional input options are:

|Name                           |Description                              |Type             |
|:------------------------------|:----------------------------------------|:----------------|
|`PetioleDirectionDistribution` | Petiole direction distribution function | function handle |
|`Phyllotaxis`                  | Leaf phyllotaxis pattern struct         | struct          |
|`IntersectionPrevention`       | Intersection prevention flag            | boolean         |
|`LeaflessBranchInds`           | Leafless branch indices                 | double          |

### Petiole direction distribution

A function handle for the petiole direction distribution function on cylinder level. Defined over the interval $[0,2\pi]$ such that $0$ and $2\pi$ correspond to the upmost radial direction, and the value increases in clockwise direction when the cylinder is viewed in the direction of the cylinder axis. The default distribution for petiole directions is uniform distribution.

### Leaf phyllotaxis pattern

A struct containing parameters for a phyllotaxis pattern of the leaves. Overrides the petiole direction distribution.  More information on phyllotaxis can be found in the Phyllotaxis section in Additional Details. By default there is no phyllotaxis pattern.

### Intersection prevention

A boolean value for turning intersection prevention of leaves off. Intersection prevention is computationally the heaviest part of the leaf generation, but assures more realistic foliage. If this is not needed, intersection prevention can be turned off. By default intersection prevention is on.

### Leafless branches

A vector specifying the branch indices where leaves should not be generated. This enables leaving certain parts of the QSM leafless (e.g., dead branches). The tree stem (branch index 1) is always excluded from leaf generation.

## Outputs

### Leaves

The output `Leaves` is a `LeafModelTriangle`-class object, which contains information of the generated foliage and functions for visualization. The contents of the class are:

|Name                  |Description                            |Type  |
|:---------------------|:--------------------------------------|:-----|
|`base_vertices`       |Base geometry vertice points           |double|
|`base_dimensions`     |Base geometry outer dimensions         |double|
|`base_triangles`      |Base geometry triangle definitions     |double|
|`base_area`           |Base geometry surface area             |double|
|`triangle_count`      |Number of triangles in base geometry   |double|
|`leaf_count`          |Total number of leaves                 |double|
|`leaf_area`           |Surface areas of leaves                |double|
|`leaf_start_point`    |Start points of leaves                 |double|
|`leaf_scale`          |Scale factors of leaves                |double|
|`leaf_direction`      |Tip direction vectors of leaves        |double|
|`leaf_normal`         |Normal vectors of leaves               |double|
|`leaf_parent`         |Parent cylinders of leaves             |double|
|`petiole_start_point` |Petiole start points of leaves         |double|

### QSMbc

The output `QSMbc` is a `QSMBCylindrical`-class object, which contains information of the QSM and functions for visualization. More infromation can be found from [QSM-Blocks] repository.

[TreeQSM]: https://github.com/InverseTampere/TreeQSM
[QSM-Blocks]: https://github.com/InverseTampere/qsm-blocks-matlab