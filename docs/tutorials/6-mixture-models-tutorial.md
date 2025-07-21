---
title: Mixture models tutorial
layout: default
parent: Tutorials
nav_order: 6
---

# Mixture model target distribution tutorial

This tutorial demonstrates how to define mixture models for the LADD marginal distributions. The mixture models are combinations of two distributions of the same type, allowing for more diverse and multimodal definitions for LADD. Compared to using only a single distribution and parameters for the definition of a marginal LADD, the mixture model takes in two sets of input parameters and a weighting factor between the two distributions. The mixture model distributions for LADD can be used in all of the the methods: QSM direct, leaf cylinder library, and canopy hull methods.

## Target LADD without mixture models

Let's start by first defining a comparison foliage without mixture models. We use the same target distribution definitions as in the `main_qsm_direct.m` script.

```matlab
% LADD relative height
TargetDistributions.dTypeLADDh = 'beta';
TargetDistributions.pLADDh = [22 3];

% LADD relative branch distance
TargetDistributions.dTypeLADDd = 'weibull';
TargetDistributions.pLADDd = [3.3 2.8];

% LADD compass direction
TargetDistributions.dTypeLADDc = 'vonmises';
TargetDistributions.pLADDc = [5/4*pi 0.1];

% LOD inclination angle
TargetDistributions.dTypeLODinc = 'dewit';
TargetDistributions.fun_pLODinc = @(h,d,c) [1 2];

% LOD azimuth angle
TargetDistributions.dTypeLODaz = 'uniform';
TargetDistributions.fun_pLODaz = @(h,d,c) [];

% LSD
TargetDistributions.dTypeLSD = 'normal';
TargetDistributions.fun_pLSD = @(h,d,c) [0.004 0.00025^2];

% Visualize the target distributions
visualize_target_distributions(TargetDistributions,[0 0 0]);
```

The visualization shows us the following distributions:

<img src="/assets/images/target-visualization.png" width="500" height="500" />

## Target LADD with mixture models

Now let us define mixture models for the LADD marginal distributions. These can be achieved by adding the word "mixture" to the end of distribution type definition and providing the parameters of the second distribution and the weighting factor as extension to the parameter vector.

```matlab
% LADD relative height
TargetDistributions.dTypeLADDh = 'betamixture';
TargetDistributions.pLADDh = [22 3 8 12 0.85];

% LADD relative branch distance
TargetDistributions.dTypeLADDd = 'weibullmixture';
TargetDistributions.pLADDd = [3.3 2.8 1.1 6 0.3];

% LADD compass direction
TargetDistributions.dTypeLADDc = 'vonmisesmixture';
TargetDistributions.pLADDc = [5/4*pi 1.1 1/4*pi 1.1 0.5];

% LOD inclination angle
TargetDistributions.dTypeLODinc = 'dewit';
TargetDistributions.fun_pLODinc = @(h,d,c) [1 2];

% LOD azimuth angle
TargetDistributions.dTypeLODaz = 'uniform';
TargetDistributions.fun_pLODaz = @(h,d,c) [];

% LSD
TargetDistributions.dTypeLSD = 'normal';
TargetDistributions.fun_pLSD = @(h,d,c) [0.004 0.00025^2];

% Visualize the target distributions
visualize_target_distributions(TargetDistributions,[0 0 0]);
```

In this definition for heightwise LADD we use a mixture of beta distribution with the first one having parameters $\alpha = 22$ and $\beta = 3$, the second one having parameters $\alpha = 8$ and $\beta = 12$, and the weighting factor of $0.85$ on the first distribution (and thus weight $0.15$ on the second). The distancewise LADD uses a mixture of two truncated Weibull distributions with the first one having parameters $\lambda = 3.3$ and $k = 2.8$, the second one having parameters $\lambda = 1.1$ and $k = 6$, and the weighting factor being $0.3$ on the first distribution (i.e. $0.7$ weight on the second). The compass direction LADD has a mixture of two von Mises distributions with the first one having parameters $\mu = \frac{5}{4} \pi$ and $\kappa = 1.1$, the second one having $\mu = \frac{1}{4} \pi$ and $\kappa = 1.1$, and equal weight $0.5$ on each of the distributions.

With these definitions, we get the following visualization:

<img src="/assets/images/target-visualization-mixture.png" width="500" height="500" />

Here we can see the more detailed marginal LADD distributions (in blue) compared to the previous visualization.

## Resulting foliage

Below are figures of the foliage generated with each of the above definitions.

Without mixutre models:

<img src="/assets/images/qsm-direct-3d-visualization.png" width="250" height="250" />

Using mixture models:

<img src="/assets/images/qsm-direct-3d-visualization-mixture.png" width="250" height="250" />

We can also plot the histograms of the LADD marginal distributions

Without mixture models:

<img src="/assets/images/result-ladd-visualization.png" width="700" height="500" />

With mixture models:

<img src="/assets/images/result-ladd-visualization-mixture.png" width="700" height="500" />