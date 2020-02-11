Process
*************************************
Introduction
============

RTI
=====
Reflectance Transformation Imaging is a per pixel function fitting technique to interactively display objects under varying light conditions.
It was originally developed by Tom Malzbender in the form of Polynomial Texture Maps [Malzbender2001]_.

The observed intensity at a pixel is described a function of the light direction, in the case of PTM as a biquadratic polynomial, but other representations exist. A set of equations relating the observed intensity with the light direction for every image of the image set is then solved for the coefficients.

Besides PTM, another popular RTI method uses hemispherical harmonical functions. Because it can model the light space better, hard shadows and bright specularities are better preserved.

A newer approach uses Principle Component Analysis + Radial Basis Function interpolation [ponchio]_ 

The goal of RTI is to view objects interactively under varying incident light directions. As such, RTI doesn't use physical laws like PLD to extract surface material characteristics. Effects like self shadowing, interreflections and complicated light-matter interactions (e.g. subsurface scattering, complicated BRDFs) are not taken into account, though their contribution is captured in the fitting/interpolation function. As such, the relighting output appears to be photorealistic - especially for virtual light positions that are close to one of the input images.

PLD
=====
.. _PLDAlgorithm:

Algorithm
-----------
PLD uses certain light material interaction models to estimate material properties.

When light interacts with matter, it can be amongst other things absorbed, transmitted or reflected. In what follows, we make abstraction of effects like interference, diffraction, scattering and the photoelectric effect [1]_. They definitely are useful to study and model certain properties - e.g. Rayleigh scattering to explain why the sky is blue and Mie scattering why clouds are greyish white, but are generally not used in 3D scanning approaches. 

Certain materials are perfectly matte; They reflect light isotropically. The observed reflected intensity is proportional to the cosine of the angle between the direction of the incident light and the surface orientation. This is sometimes refered to as Lambert's cosine law or Lambertian reflectance.

Unfinished wood or blotted paper are examples of Lambertian reflective materials. The Moon however is not. If it were, the edges should be completely dark during a full Moon, as the cosine of the angle between the sun rays and the surface is 0. 

In 1980 Woodham published the first paper on Photometric Stereo [Woodham1980]_. Photometric stereo is a computer vision technique that exploits this Lambertian reflectance of certain materials to find the surface orientation.
An object is illuminated from several, known, light directions. Each time a photo is taken. 
The result is a set of apparent brightness values and the corresponding light directions. This can be solved for the surface orientation. Note that this algorithm is applied per pixel. 

The algorithm has been extended to cope with self shadowing, interreflections, image noise and non-ideal point light sources.

