Visual Styles
*************
Introduction
=================================
PLD and RTI have developed a plethora of styles to visualize their higher dimensional data. Each one of them accentuates certain aspects of the surface. In order to interpret the results, a summary of each visual style is provided.

PLD
====
Color
------
The 'Color' style implements how a diffuse material reflects light and is as such the inverse of the principle of Photometric Stereo (in which the albedo and surface normal get disentangled, see :ref:`PLDAlgorithm`). In computer graphics and computer vision, this model is sometimes referred to as Lambertian reflectance. The reflected intensity is proportional to the cosine of the angle between the light direction and the normal vector. 

A beam of light falling on an object has a certain amount of energy. If the surface of the object is perpendicular to the light direction, it receives the highest amount of energy per area. When the surface is tilted with respect to the light direction, the same amount of energy gets spread across a bigger area, resulting in a lower energy density received by the surface. 

The brightness of a Lambertian object does not depend on the viewing position, i.e. a Lambertian object looks equally bright from all positions. More physical background can be found `here <http://hyperphysics.phy-astr.gsu.edu/hbase/vision/photom.html>`_ and `here <https://en.wikipedia.org/wiki/Lambert%27s_cosine_law>`_ 

In terms of computer graphics, the light paths used in this style consist of a single bounce at the surface ( point light source -> surface and surface->camera ) 
Interreflections (i.e. light paths with several bounces) and self shadowing (part of the object can cast a shadow on another part, especially noticable at grazing light directions) are not modeled. 

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* The color source: For white light recordings, this can be an RGB image or each red, green or blue channel separately. For Multi Spectral recordings, more combinations are possible. E.g. IRG will be displayed as a false color image with the infrared channel as red, the red channel as green and the green channel as blue.
* Reflectance source: Albedo (see :ref:`albedoMap`) vs. Ambient (see :ref:`ambientMap`)
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.


Shaded
-------

This visual style is the same as the Color style, except that the Albedo is set to a uniform value. By removing the local color information from the image, a more careful study of the surface orientation (see :ref:`normalMap`) is possible by carefully changing the virtual light direction.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.


Shaded exaggerated
-------------------

This visual style is the same as the Shaded style, except that surface orientation is exaggerated w.r.t. the orientation parallell to the camera direction.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.



Sharpen
--------

This visual style is the same as the Color style, except that the color map is sharpened. 2 parameters control the sharpening: Percentage and Size. See also `Unsharp masking <https://en.wikipedia.org/wiki/Unsharp_masking#Digital_unsharp_masking>`.

Parameters
+++++++++++
* The direction of the virtual light sources
* The brightness of the virtual light sources
* The color source: For white light recordings, this can be an RGB image or each red, green or blue channel separately. For Multi Spectral recordings, more combinations are possible. E.g. IRG will be displayed as a false color image with the infrared channel as red, the red channel as green and the green channel as blue.
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Reflectance source: Albedo (see :ref:`albedoMap`) vs. Ambient (see :ref:`ambientMap`)
* Percentage
* Size


Sketch 1
---------

This visual style is inspired by pencil drawings of objects with local relief like cuneiform tablets. Where the surface direction locally abrubtly changes, a black pixel value is set.

Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Sensitivity
* Thickness

Sketch 2
---------


Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Sensitivity
* Thickness

Curvature
-----------


Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.
* Intensity
* Area

Normals
--------

See :ref:`normalMap`.

Parameters
+++++++++++
* Normal source. For white light recordings, a single normal map is calculated. For multi spectral recordings, a normal map per spectral band is calculated.

RTI
====
