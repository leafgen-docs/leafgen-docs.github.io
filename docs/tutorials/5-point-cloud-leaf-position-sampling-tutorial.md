---
title: Point cloud leaf position sampling tutorial
layout: default
parent: Tutorials
nav_order: 5
---

# Point cloud leaf position sampling tutorial

This tutorial demonstrates how to utilize the point cloud in leaf position sampling in the canopy hull method. 

By default the canopy hull method generates an alpha shape around the tree point cloud and then samples the LADD within this shape to get the leaf positions. However, if the point cloud data is accurate, it makes sense to generate the leaves close to the point cloud points as these are realistic places for the leaves to be.  

In addition, it might sometimes be useful to sample the leaf positions near the point cloud in the lower canopy, where the point cloud represents the branching sturcture more accurately, and use LADD in sampling for the upper part of the canopy, where the point cloud measurement is less accurate.

## Initialization

```matlab
% Clear all variables from the workspace and close all open figures
clear, close all

% Add the current directory and all the subdirectories to search path
addpath(genpath(pwd))
```

## Leaf base geometry

```matlab
% Vertices of the leaf base geometry
LeafProperties.vertices = [0.0    0.0    0.0;
                           -0.04  0.02   0.0;
                           0.0    0.10   0.0;
                           0.04   0.02   0.0];

% Triangles of the leaf base geometry
LeafProperties.triangles = [1 2 3;
                            1 3 4];
```

## Leaf distributions

```matlab
% LADD relative height
TargetDistributions.dTypeLADDh = 'beta';
TargetDistributions.pLADDh = [9 3.5];

% LADD relative distance from stem
TargetDistributions.dTypeLADDd = 'weibull';
TargetDistributions.pLADDd = [3.3 2.8];

% LADD compass direction
TargetDistributions.dTypeLADDc = 'vonmisesmixture';
TargetDistributions.pLADDc = [3/4*pi 2 7/4*pi 2 0.5];

% LOD inclination angle
TargetDistributions.dTypeLODinc = 'dewit';
TargetDistributions.fun_pLODinc = @(h,d,c) [-1 4];

% LOD azimuth angle
TargetDistributions.dTypeLODaz = 'uniform';
TargetDistributions.fun_pLODaz = @(h,d,c) [];

% LSD
TargetDistributions.dTypeLSD = 'normal';
TargetDistributions.fun_pLSD = @(h,d,c) [0.007 0.00025^2];
```

## Point cloud definition

```matlab
% Load point cloud from memory
addpath("<path_to_wytham_data>")
filename = "DATA_QSM_opt\wytham_winter_8399-0.2-0.22-3-0.045-0.0495-2-5-1-9.mat";
QSM = importdata(filename);
ptCloud = QSM.P;

% Center the point cloud to z-axis
stemBase = ptCloud((ptCloud(:,3) < min(ptCloud(:,3))+1),:);
baseMean = mean(stemBase,1);
ptCloud = ptCloud - [baseMean(1:2) 0];
```

## Target leaf area

```matlab
totalLeafArea = 50;
```

## Generating foliage without point cloud sampling

```matlab
[Leaves1,aShape] = generate_foliage_canopy_hull(ptCloud, ...
                                     TargetDistributions, ...
                                     LeafProperties, ...
                                     totalLeafArea);
```

## Generating foliage with progressive point cloud samplin

```matlab
% Define point cloud sampling weights
samplingWeights2 = [1 1 1 1 1 0.8 0.6 0.4 0.2 0];

% Generate foliage
[Leaves2,aShape] = generate_foliage_canopy_hull(ptCloud, ...
                                     TargetDistributions, ...
                                     LeafProperties, ...
                                     totalLeafArea, ...
                                     'PCSamplingWeights',samplingWeights2);
```

## Generating foliage with full point cloud sampling

```matlab
% Define point cloud sampling weights
samplingWeights3 = 1;

% Generate foliage
[Leaves3,aShape] = generate_foliage_canopy_hull(ptCloud, ...
                                     TargetDistributions, ...
                                     LeafProperties, ...
                                     totalLeafArea, ...
                                     'PCSamplingWeights',samplingWeights3);
```

## Comparing the results

```matlab
% Plot leaves
f3 = figure(3); clf, hold on
tiledlayout(2,2)
s1 = nexttile(1);
plot3(ptCloud(:,1),ptCloud(:,2),ptCloud(:,3),'k.','MarkerSize',1)
view(45,0)
axis equal, axis tight
title("Point cloud")
s2 = nexttile(2);
hLeaf1 = Leaves1.plot_leaves();
set(hLeaf1,'FaceColor',[0,150,0]./255,'EdgeColor','none');
axis equal, axis tight, view(45,0)
title("No point cloud sampling")
s3 = nexttile(3);
hLeaf2 = Leaves2.plot_leaves();
set(hLeaf2,'FaceColor',[0,150,0]./255,'EdgeColor','none');
axis equal, axis tight, view(45,0)
title("Progressive point cloud sampling")
s4 = nexttile(4);
hLeaf3 = Leaves3.plot_leaves();
set(hLeaf3,'FaceColor',[0,150,0]./255,'EdgeColor','none');
axis equal, axis tight, view(45,0)
title("Full point cloud sampling")
linkaxes([s1 s2 s3 s4])
```