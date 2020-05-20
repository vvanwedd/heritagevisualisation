Manual
#######
Introduction
============
The pixel+ viewer is a WebGL based web viewer of :ref:`single-camera multi-light<technology:Technology>` files. The surface of objects that are scanned with this technology can be carefully examined by changing the virtual illumination or the :ref:`visual style<visualstyles:Visual Styles>`.

PLD
====

Default color
--------------
The *Default color* style implements how a diffuse material reflects light and is as such the inverse of the principle of Photometric Stereo (in which the albedo and surface normal get disentangled, see :ref:`PLDAlgorithm`). In computer graphics and computer vision, this model is sometimes referred to as Lambertian reflectance. The reflected intensity is proportional to the cosine of the angle between the light direction and the normal vector. 

A beam of light falling on an object has a certain amount of energy. If the surface of the object is perpendicular to the light direction, it receives the highest amount of energy per area. When the surface is tilted with respect to the light direction, the same amount of energy gets spread across a bigger area, resulting in a lower energy density received by the surface. 

The brightness of a Lambertian object does not depend on the viewing position, i.e. a Lambertian object looks equally bright from all positions. More physical background can be found `here <http://hyperphysics.phy-astr.gsu.edu/hbase/vision/photom.html>`_ and `here <https://en.wikipedia.org/wiki/Lambert%27s_cosine_law>`_ 

In terms of computer graphics, the light paths used in this style consist of a single bounce at the surface ( point light source -> surface and surface -> camera ) 
Interreflections (i.e. light paths with several bounces) and self shadowing (part of the object can cast a shadow on another part, especially noticeable at grazing light directions) are therefore not modeled. 

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* The color source: For white light recordings, this can be an RGB image or each red, green or blue channel separately. For multi spectral recordings, more combinations are possible. E.g. IRG will be displayed as a false color image with the infrared channel as red, the red channel as green and the green channel as blue.
* Reflectance source: Albedo (see :ref:`albedoMap`) vs. Ambient (see :ref:`ambientMap`)
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.

.. figure:: _static/images/visualstyle_pld_defColor.gif
   :scale: 50 %

   Alternating the virtual light position in PLD's *Default color* style.
   
Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`
* either an :ref:`albedoMap` or :ref:`ambientMap`

Shaded
-------

This visual style is the same as the *Default color* style, except that the Albedo is set to a uniform value. By removing the local color information from the image, a more careful study of the surface orientation (see :ref:`normalMap`) is possible by carefully changing the virtual light direction.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.

.. figure:: _static/images/visualstyle_pld_shaded.gif
   :scale: 50 %

   PLD's *Shaded* style allows for a more careful study of surface orientation w.r.t. the *Default color* style.
   
Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`

Shaded exaggerated
-------------------

This visual style is the same as the *Shaded* style, except that surface orientation is exaggerated w.r.t. the orientation parallel to the camera direction.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.

.. figure:: _static/images/visualstyle_pld_shadedExa.gif
   :scale: 50 %

   Alternating view of the *Shaded* and *Shaded Exaggerated* style.
   
Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`

Sharpen
--------

This visual style is the same as the *Color* style, except that the color map is sharpened. 2 parameters control the sharpening: Percentage and Size. See also `Unsharp masking <https://en.wikipedia.org/wiki/Unsharp_masking#Digital_unsharp_masking>`_.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* The color source: For white light recordings, this can be an RGB image or each red, green or blue channel separately. For multi spectral recordings, more combinations are possible. E.g. IRG will be displayed as a false color image with the infrared channel as red, the red channel as green and the green channel as blue.
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Reflectance source: Albedo (see :ref:`albedoMap`) vs. Ambient (see :ref:`ambientMap`)
* Percentage
* Size

.. figure:: _static/images/visualstyle_pld_sharpen.gif
   :scale: 50 %

   Influence of the Percentage parameter of the *Sharpen* style

.. figure:: _static/images/visualstyle_pld_sharpenBis.gif
   :scale: 50 %

   Influence of the Size parameter of the *Sharpen* style.
   
Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`
* either an :ref:`albedoMap` or :ref:`ambientMap`

Sketch 1
---------

This visual style is inspired by pencil drawings of objects with local relief like cuneiform tablets. Where the surface direction locally abruptly changes, a black pixel value is set.

Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Sensitivity
* Thickness

.. figure:: _static/images/visualstyle_pld_sketch1.gif
   :scale: 50 %

   Influence of the Thickness parameter of the *Sketch 1* style

.. figure:: _static/images/visualstyle_pld_sketch1Bis.gif
   :scale: 50 %

   Influence of the Sensitivity parameter of the *Sketch 1* style.
   
Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`

Sketch 2
---------


Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Sensitivity
* Thickness

Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`
* either an :ref:`albedoMap` or :ref:`ambientMap`

Curvature
-----------


Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Intensity
* Area

Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`

Normals
--------

See :ref:`pld:Normal map`.

.. figure:: _static/images/visualstyle_pld_normals.png
   :scale: 50 %

   *Normal* style showing the surface orientation. For more information see :ref:`pld:Normal map`.

Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.

Necessary input maps
++++++++++++++++++++

* :ref:`normalMap`

HSH RTI
=======
Default color
-------------

The *Default color* style renders the object in a photorealistic way. The level of light-varying detail that is captured depends on the amount of HSH basis functions that is used (which order is chosen during the creation of the HSH RTI file)

Parameters
+++++++++++

* The direction of the virtual light sources
* The brightness of the virtual light sources

Necessary input maps
++++++++++++++++++++

* HSH coefficients encoded either directly in an HSH RTI file or grouped per 3 as image data

Specular enhancement
---------------------

Sometimes, it is worthwhile to artificially boost the surface detail visibility. The *Specular enhancement style* achieves this by introducing a specular Phong term in the rendering equation. See also `Phong reflection model <https://en.wikipedia.org/wiki/Phong_reflection_model>`_. 

Parameters
+++++++++++

* The direction of the virtual light sources
* The brightness of the virtual light sources
* exp: Also sometimes known as alpha or the shininess constant. Increasing this value makes the appearance more mirrorlike - smaller specular highlights. 
* Ks: The ratio of specular reflection on the incoming light
* Kd: The ratio of diffuse reflection on the incoming light
* Normal source

Necessary input maps
++++++++++++++++++++

* HSH coefficients encoded either directly in an HSH RTI file or grouped per 3 as image data
* Normal map, either calculated from the HSH coefficients or possibly, when using glTF, using another method (e.g. PLD or PTM)

HSH sharpening
--------------
The *HSH sharpening* style is the same as the *Default color* style, but with the option to sharpen the HSH components. 2 parameters control the sharpening: Percentage and Size. See also `Unsharp masking <https://en.wikipedia.org/wiki/Unsharp_masking#Digital_unsharp_masking>`_.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* Percentage
* Size

Necessary input maps
++++++++++++++++++++

* HSH coefficients encoded either directly in an HSH RTI file or grouped per 3 as image data

PTM 
====

Default color
-------------
The *Default color* style renders the object in a photorealistic way. The level of light-varying detail that is captured depends on the order of the polynomial that is used during the creation of the PTM file.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources

Necessary input maps
++++++++++++++++++++

* PTM basis functions encoded either directly in an HSH RTI file or grouped per 3 as image data


Specular enhancement
---------------------

Sometimes, it's worthwhile to artificially boost the surface detail visibility. The *Specular enhancement* style achieves this by introducing a specular Phong term in the rendering equation. See also `Phong reflection model <https://en.wikipedia.org/wiki/Phong_reflection_model>`_. 

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* exp: Also sometimes known as alpha or the shininess constant. Increasing this value makes the appearance more mirrorlike - smaller specular highlights. 
* Ks: The ratio of specular reflection on the incoming light
* Kd: The ratio of diffuse reflection on the incoming light
* Normal source

Necessary input maps
++++++++++++++++++++

* PTM coefficients encoded either directly in a PTM file or grouped per 3 as image data
* Normal map, either calculated from the PTM coefficients or possibly, when using glTF, using another method (e.g. PLD or HSH RTI)

