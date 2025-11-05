---
title: Leaf distribution options
layout: default
nav_order: 4
---

# Leaf distribution options

## Leaf Area Density Distribution (LADD)

### Relative height

Options for the marginal distribution function of relative height `dTypeLADDh` are:

| Distribution                  | `dTypeLADDh`   | Function Definition                                                                            |
|-------------------------------|----------------|------------------------------------------------------------------------------------------------|
| uniform                       | `'uniform'`    | $f(x) = 1$                                                                                     |
| beta                          | `'beta'`       | $f(x,\alpha,\beta) = (x^{\alpha-1}(1-x)^{\beta-1})/\mathrm{B}(\alpha,\beta)$                   |
| truncated Weibull             | `'weibull'`    | $f(x,k,\lambda) = \frac{k}{\lambda} \left( \frac{x}{\lambda} \right)^{k-1} e^{-(x/\lambda)^k}$ |
| polynomial                    | `'polynomial'` | $f(x,p_0,\ldots,p_n) = p_n x^n + \ldots + p_1 x + a_0$                                         |
| QSM-based[*](#qsm-based-note) | `'qsm'`        | Nonparametric distribution                                                                     |

Here $\mathrm{B}(\alpha,\beta)  = \int_0^1 x^{\alpha-1} (1-x)^{\beta-1} dx$ is the beta function.

The possible parameter values `pLADDh` are:

| Distribution      | `pLADDh`      | Parameter Values                 |
|-------------------|---------------|----------------------------------|
| uniform           | -             | -                                |
| beta              | `[a b]`       | `a`, `b` $> 0$                   |
| truncated Weibull | `[k l]`       | `k`, `l` $> 0$                   |
| polynomial        | `[p0 ... pN]` | `p0`, ..., `pN` $\in \mathbb{R}$ |

### Relative branch distance / relative distance from stem

Options for the marginal distribution function of relative branch distance / relative distance from stem `dTypeLADDd` are:

| Distribution                  | `dTypeLADDd`   | Function Definition                                                                            |
|-------------------------------|----------------|------------------------------------------------------------------------------------------------|
| uniform                       | `'uniform'`    | $f(x) = 1$                                                                                     |
| beta                          | `'beta'`       | $f(x,\alpha,\beta) = (x^{\alpha-1}(1-x)^{\beta-1})/\mathrm{B}(\alpha,\beta)$                   |
| truncated Weibull             | `'weibull'`    | $f(x,k,\lambda) = \frac{k}{\lambda} \left( \frac{x}{\lambda} \right)^{k-1} e^{-(x/\lambda)^k}$ |
| polynomial                    | `'polynomial'` | $f(x,p_0,\ldots,p_n) = p_n x^n + \ldots + p_1 x + a_0$                                         |
| QSM-based[*](#qsm-based-note) | `'qsm'`        | Nonparametric distribution                                                                     |

Here $\mathrm{B}(\alpha,\beta)  = \int_0^1 x^{\alpha-1} (1-x)^{\beta-1} dx$ is the beta function.

The possible parameter values `pLADDd` are:

| Distribution      | `pLADDd`     | Parameter Values                  |
|-------------------|---------------|----------------------------------|
| uniform           | -             | -                                |
| beta              | `[a b]`       | `a`, `b` $> 0$                   |
| truncated Weibull | `[k l]`       | `k`, `l` $> 0$                   |
| polynomial        | `[p0 ... pN]` | `p0`, ..., `pN` $\in \mathbb{R}$ |

### Compass direction

Options for the marginal distribution function of compass direction `dTypeLADDc` are:

| Distribution                  | `dTypeLADDc` | Function Definition                                                     |
|-------------------------------|--------------|-------------------------------------------------------------------------|
| uniform                       | `'uniform'`  | $f(x) = 1/2\pi$                                                         |
| von Mises                     | `'vonmises'` | $f(x,\mu,\kappa) = e^{\kappa \cos(x-\mu)}/(2 \pi \mathrm{I}_0(\kappa))$ |
| QSM-based[*](#qsm-based-note) | `'qsm'`      | Nonparametric distribution                                              |

Here $I_0(\kappa) = \frac{1}{2\pi} \int_0^{2\pi} e^{\kappa \cos(x)} dx$ is the modified Bessel function of the first kind of order 0.

The possible parameter values `pLADDc` are:

| Distribution | `pLADDc`    | Parameter Values               |
|--------------|-------------|--------------------------------|
| uniform      | -           | -                              |
| von Mises    | `[m k]`     | `m` $\in [0,2\pi]$, `k` $> 0$  |

### Mixture models

For all LADD marginal distributions, it is also possible to define the distributions as mixture models of two distributions of the same type. This allows for a larger variety of marginal distribution shapes, like multimodal distributions. The definition of a mixture model requires setting the parameters of each distribution separately and defining a weighting factor among the distributions. The weighting factor `w` is given a value between 0 and 1, which determines the weights of the distributions to be `w` for the first distribution and 1-`w` for the second distribution. Mixture models can be defined for the following distributions:

| Distribution      | `pLADDh`/`pLADDd`/`pLADDc`   | Parameter Values                                                |
|-------------------|---------------------------------|--------------------------------------------------------------|
| beta              | `[a1 b1 a2 b2 w]`               | `a1`, `b1`, `a2`, `b2` $> 0$, `w` $\in [0,1]$                |
| truncated Weibull | `[k1 l1 k2 l2 w]`               | `k1`, `l1`, `k2`, `l2`$> 0$, `w` $\in [0,1]$                 |
| von Mises         | `[m1 k1 m2 k2 w]`               | `m1`, `m2` $\in [0,2\pi]$, `k1`, `k2` $> 0$, `w` $\in [0,1]$ |

### <a name="qsm-based-note"></a> \* The QSM-based LADD

{. :note}
The QSM-based approach distributes the leaves evenly throughout the branch cylinders, emphasizing the leaves towards the tips of the branches, and can be considered as some kind of "automatic" leaf positioning for the QSM. This is useful if the user wants to generate a somewhat realistic foliage and the specific shape of LADD is not important. When using QSM-based approach the `dTypeLADD` field of each of the structural variables have to be set to `'qsm'`. Also, the QSM-based approach is obviously available only when generating foliage on QSMs.

## Leaf Orientation Distribution (LOD)

### Inclination angle distribution

Options for the marginal distribution function of leaf inclination angle `dTypeLODinc` are:

| Distribution         | `dTypeLODinc` | Function Definition                                                                         |
|----------------------|---------------|---------------------------------------------------------------------------------------------|
| uniform              | `'uniform'`   | $f(\theta) = 2/\pi$                                                                         |
| spherical            | `'spherical'` | $f(\theta) = \sin(\theta)$                                                                  |
| generalized de Wit's | `'dewit'`     | $f(\theta;a,b) = (1 + a \cos(b\theta))/(\frac{\pi}{2} + \frac{a}{b} \sin(b\frac{\pi}{2}))$  |
| beta                 | `'beta'`      | $f(\theta,\alpha,\beta) = (\theta^{\alpha-1}(1-\theta)^{\beta-1})/\mathrm{B}(\alpha,\beta)$ |
| constant value       | `'constant'`  | -                                                                                           |

Here $\mathrm{B}(\alpha,\beta)  = \int_0^1 x^{\alpha-1} (1-x)^{\beta-1} dx$ is the beta function.

The possible parameter values for the function `fun_pLODinc` are:

| Distribution         | `fun_pLODinc`    | Parameter Values                  |
|----------------------|------------------|-----------------------------------|
| uniform              | -                | -                                 |
| spherical            | -                | -                                 |
| generalized de Wit's | `[a b]`          | `a` $\in [-1,1]$, `b` $\in [2,4]$ |
| beta                 | `[a b]`          | `a`, `b` $> 0$                    |
| constant value       | `c`              | `c` $\in [0,\frac{\pi}{2}]$       |

### Azimuth angle distribution

Options for the marginal distribution function of leaf azimuth angle `dTypeLODaz` are:

| Distribution   | `dTypeLODaz` | Function Definition                                                           |
|----------------|--------------|-------------------------------------------------------------------------------|
| uniform        | `'uniform'`  | $f(\phi) = 1/2\pi$                                                            |
| von Mises      | `'vonmises'` | $f(\phi,\mu,\kappa) = e^{\kappa \cos(\phi-\mu)}/(2 \pi \mathrm{I}_0(\kappa))$ |
| constant value | `'constant'` | -                                                                             |

Here $I_0(\kappa) = \frac{1}{2\pi} \int_0^{2\pi} e^{\kappa \cos(x)} dx$ is the modified Bessel function of the first kind of order 0.

The possible parameter values for the function `fun_pLODaz` are:

| Distribution   | `fun_pLODaz`    | Parameter Values              |
|----------------|-----------------|-------------------------------|
| uniform        | -               | -                             |
| von Mises      | `[m k]`         | `m` $\in [0,2\pi]$, `k` $> 0$ |
| constant value | `c`             | `c` $\in [0,2\pi]$            |

## Leaf Size Distribution (LSD)

Options for the distribution function of leaf size distribution `dTypeLSD` are:

| Distribution   | `dTypeLSD`   | Function Definition                                                            |
|----------------|--------------|--------------------------------------------------------------------------------|
| uniform        | `'uniform'`  | $f(x,a,b) = (x-a)/(b-a)$                                                       |
| normal         | `'normal'`   | $f(x,\mu,\sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-(x-\mu)^2/(2\sigma^2)}$ |
| constant value | `'constant'` | -                                                                              |


The possible parameter values for the function `fun_pLSD` are:

| Distribution   | `fun_pLSD`        | Parameter Values |
|----------------|-------------------|------------------|
| uniform        | `[a b]`           | `a`, `b` $> 0$   |
| normal         | `[m v]`           | `m`, `v` $> 0$   |
| constant value | `c`               | `c` $> 0$        |
