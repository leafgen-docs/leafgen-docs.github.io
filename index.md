---
title: Home
layout: home
nav_order: 1
---

# LeafGen Documentation

## Description

This is the documentation of the [LeafGen package](https://github.com/InverseTampere/leafgen). Overview of the methods is presented in the article

- Mönkkönen P, Van den Broeck W, Ali-Löytty S, Calders K, and Raumonen P (2025). *LeafGen*: Foliage Generation in 3D Tree Models. Methods in Ecology and Evolution. [https://doi.org/10.1111/2041-210X.70080](https://doi.org/10.1111/2041-210X.70080).

This reference can be also used to cite LeafGen.

## Contents

1. [Tutorials](/docs/tutorials)
2. [Basic functionality](/docs/basic-functionality)
3. [Leaf distribution options](/docs/leaf-distributions)
4. [Additional information](/docs/additional-information)

You can use the navigation panel on the left to browse the contents of the documentation.

## Setup

LeafGen is coded in MATLAB programming language and can be run using a local installation of [MATLAB](https://www.mathworks.com/products/matlab.html) or using [MATLAB online](https://www.mathworks.com/products/matlab-online.html). Once you have MATLAB set up, do the following:

1. Download the LeafGen repository from [GitHub](https://github.com/InverseTampere/leafgen) and extract the files.
2. Open the `src` folder of the extracted repository and open one of the example main files in MATLAB (preferably start with either `main_qsm_direct.m` or `main_canopy_hull.m`, as the `main_leaf_cylinder_library.m` requires a long computation.) This action opens the file's script to MATLAB's editor window.
3. Run the script using the "Run" button in the editor tab of MATLAB or by pressing F5 on your keyboard. If MATLAB prompts you that the chosen file is not found in the current folder, press the "Change Folder" button to go to the right folder.

This makes MATLAB run one of the main demos of LeafGen, and after a while of computing should provide you with plots of the generated foliage and the realized leaf distributions.

## Where to start

 If you are using LeafGen for the first time, check the basic tutorials in the [Tutorials](/docs/tutorials) section to get started.
