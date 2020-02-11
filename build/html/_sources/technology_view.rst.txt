View
*************************************


RTI
=====
Hardware
---------
.. figure:: _static/images/dome7OxfordClassics.jpg
   :scale: 50 %
   :alt: Example of an RTI dome

   Example of an RTI dome as produced by `custom-imaging.co.uk <https://custom-imaging.co.uk/projects/dome-7/>`_ (their Dome 7).

There is a large variety of scanning equipment available, but in essence, both PLD and RTI require a set of images of an object that is illuminated from various known directions. 
RTI has 2 types of recording equipment: The dome setups and the highlight RTI setup.
The dome setup functions in the similar manner as the PLD domes.

Highlight RTI utilizes chrome/specular spheres to estimate the direction of a point light source (e.g. a flash). This technology is typically cheaper and more flexible in terms of object size to be scanned; for a manual to the procedure see [CHI2013]_.
The downside is that it generally is slower, requires more intervention and is less accurate, as light sources have to be calculated using the specular reflection in the reflective spheres. 

Algorithm
----------
Reflectance Transformation Imaging is a per pixel function fitting technique to interactively display objects under varying light conditions.
It was originally developed by Tom Malzbender in the form of Polynomial Texture Maps [Malzbender2001]_.

The observed intensity at a pixel is described a function of the light direction, in the case of PTM as a biquadratic polynomial, but other representations exist. A set of equations relating the observed intensity with the light direction for every image of the image set is then solved for the coefficients.

Besides PTM, another popular RTI method uses hemispherical harmonical functions. Because it can model the light space better, hard shadows and bright specularities are better preserved.

A newer approach uses Principle Component Analysis + Radial Basis Function interpolation [ponchio]_ 

The goal of RTI is to view objects interactively under varying incident light directions. As such, RTI doesn't use physical laws like PLD to extract surface material characteristics. Effects like self shadowing, interreflections and complicated light-matter interactions (e.g. subsurface scattering, complicated BRDFs) are not taken into account, though their contribution is captured in the fitting/interpolation function. As such, the relighting output appears to be photorealistic - especially for virtual light positions that are close to one of the input images.

Output
-------
PTM / HSH coefficients
++++++++++++++++++++++++
The output of the RTI fitting algorithm are coefficients, e.g. PTM for a PTM RTI and HSH for a HSH RTI. These coefficients are calculated per pixel and stored in a map. 

Normal map
+++++++++++
From certain types of RTI files, ie. from PTM and HSH RTI files, a normal map can be calculated. 

Generally, a normal map originating from an HSH RTI is of higher quality than one generated from PTM coefficients. 
Any spatial blurring that seems to exist is because the PTM or HSH coefficients don't capture the light space function perfectly. The resulting normal map can therefore also be more blurred than a result optained with a photometric stereo approach such as PLD.

.. check and elaborate

File formats
-------------
PTM
+++++

* RGB PTM: PTM coefficients are being calculated and saved per color channel (in 8 bit mode), resulting in 3*6 bytes per pixel
* LRGB PTM: PTM coefficients are being calculated for the luminance channel and stores non varying RGB values. As the chromaticity doesn't change too much under varying light directions, this method saves on storage, resulting in 6+3 bytes per pixel
* JPEG compression of RGB PTM
* JPEG compression of LRGB PTM
* JPEGLS compression of RGB PTM
* JPEGLS compression of LRGB PTM
* Look Up Table PTM
* YCrCb color space PTM

The upper 2 versions are the most popular PTM file types, as they are supported by CHI's processing and viewing software [CHI]_ 

RTI
++++

* PTM RTI: Polynomial Texture Mapping RTI [Malzbender2001]_
* HSH RTI: Hemispherical Harmonics RTI [Mudge2008]_
* SH RTI: Spherical Harmonics RTI
* EHSH RTI: Eigen Hemispherical Harmonics RTI [Lam2012]_
* Adaptive basis RTI

PLD
=====
Hardware
---------
There is a large variety of scanning equipment available, but in essence, both PLD and RTI require a set of images of an object that is illuminated from various known directions. PLD has a set of scanning domes, both with white light LEDs as well as narrow band near infrared, red, green, blue and near ultra violet light LEDs for multi spectral recordings. Can be listed:

White Light Mecano Minidome | Multispectral Minidome | White Light Microdome | Multispectral Microdome

.. figure:: _static/images/msmicrodome.jpg
   :scale: 50 %
   :alt:   Use of multispectral microdome (PLD system) to scan an Egyptian coffin (@ Art and History Museum, Brussels)
   :figwidth: image

   Use of multispectral microdome (PLD system) to scan an Egyptian coffin (@ Art and History Museum, Brussels)


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

Output
-------

.. _normalMap:

Normal map
+++++++++++
The surface orientation is described by a vector that is perpendicular to the surface and is being refered to as the normal vector or more simply the normal. This calculation is performed for every pixel, resulting in a normal map or normal texture. The x, y and z components of the normal vector :math:`\mathbf{n}` at a particular pixel are stored as red, green and blue values using the following convention: 

.. math::
  r = (\mathbf{n}_x + 1)*\frac{1}{2}*255\\
  g = (\mathbf{n}_y + 1)*\frac{1}{2}*255\\
  b = (\mathbf{n}_z + 1)*\frac{1}{2}*255
 
*  Note that normal :math:`\mathbf{n}` is normalized, meaning the x, y or z components can range from -1 to 1.
*  Note that 255 corresponds to storing the normal in an 8 bit image. Depending on the bit depth of the image, this should be changed. An r value of 0 thus corresponds to an x component of -1 and a b value of 255 to a z component of 1
*  Note also that this conversion to a 3 channel color bitmap introduces quantization artifacts, as the normal is typically calculated in floating point arithmetic, but has to be stored to the nearest integer color value according to the equation above.

*  If the visualization pipeline supports floating point textures, the normal map can be stored using the floating point x, y, z values directly. Typically newer GPU models support floating point textures. In WebGL, this can be checked with a flag [OES_floating_point]_

.. _albedoMap:

Albedo map
+++++++++++
Besides the normal, the albedo is another material property that is calculated. The albedo is the amount of diffuse reflected light. In radiometric terms, the albedo corresponds to the radiosity (ie. radiant flux leaving the surface per unit area) to the irradiance (ie. the radiant flux received by the surface per unit area).

.. _ambientMap:

Ambient map
++++++++++++
The ambient map consists of a per pixel averaged color of all images from the taken sequence.

.. _reflectionMap:

Reflection map
+++++++++++++++
Once the surface orientation is estimated, the reflected color for each led that was lit can be represented on a map. This sparsely sampled lower dimensional representation of a BRDF can be used to classify materials, based on their appearance (and explicitely disregarding shape). This map can also be used to fit an analytic BRDF model, which can then be used for photorealistic rendering.

.. _heightMap:

Height map and depth profile
+++++++++++++++++++++++++++++
The normal map encodes the surface direction, ie. gradient. Often times, the user is not only interested in the surface gradient, but also the surface itself. Surface integration algorithms calculate the surface starting from a normal map.

File formats
-------------
The recorded images are stored before they are debayered and before a gamma correction is applied. Thus, the corresponding pixel values correlate with the (spectrally filtered) observed intensities. The images are stored as [PGM]_

After processing, a CFD file contains the various texture maps and metadata.
Next to this file, 2 filetypes are used to easily exchange data: CUN (textures bzip2 compressed) and ZUN (textures gunzip compressed). Note that the normal textures are stored in a packed way, allowing a smaller file size.

.. rubric:: Footnotes

.. [Malzbender2001] https://www.hpl.hp.com/research/ptm/papers/ptm.pdf
.. [ponchio] https://pc-ponchio.isti.cnr.it/relight/compact-representation-relightable.pdf
.. [CHI] http://culturalheritageimaging.org/What_We_Offer/Downloads/
.. [CHI2013] http://culturalheritageimaging.org/What_We_Offer/Downloads/RTI_Hlt_Capture_Guide_v2_0.pdf
.. [Mudge2008] http://culturalheritageimaging.org/What_We_Do/Publications/eurographics2008/eurographics_2008_tutorial_notes.pdf
.. [Lam2012] https://ieeexplore.ieee.org/document/6141180
.. [1] https://www.pearson.com/us/higher-education/program/Hecht-Optics-5th-Edition/PGM45350.html
.. [Woodham1980] https://www.cs.ubc.ca/~woodham/papers/Woodham80c.pdf
.. [OES_floating_point] https://developer.mozilla.org/en-US/docs/Web/API/OES_texture_float
.. [PGM] https://en.wikipedia.org/wiki/Netpbm_format

