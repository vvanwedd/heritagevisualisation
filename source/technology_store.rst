Store
*****

RTI
=====
Output
-------
PTM / HSH coefficients
++++++++++++++++++++++++
The output of the RTI fitting algorithm are coefficients, e.g. PTM for a PTM RTI and HSH for a HSH RTI. These coefficients are calculated per pixel and stored in a map.

Normal map
+++++++++++
From certain types of RTI files, i.e. from PTM and HSH RTI files, a normal map can be calculated.

Generally, a normal map originating from an HSH RTI is of higher quality than one generated from PTM coefficients.
Any spatial blurring that seems to exist is because the PTM or HSH coefficients don't capture the light space function perfectly. The resulting normal map can therefore also be more blurred than a result obtained with a photometric stereo approach such as PLD.

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
Besides the normal, the albedo is another material property that is calculated. The albedo is the amount of diffuse reflected light. In radiometric terms, the albedo corresponds to the radiosity (i.e. radiant flux leaving the surface per unit area) to the irradiance (i.e. the radiant flux received by the surface per unit area).

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
The normal map encodes the surface direction, i.e. gradient. Often times, the user is not only interested in the surface gradient, but also the surface itself. Surface integration algorithms calculate the surface starting from a normal map.

File formats
-------------
The recorded images are stored before they are debayered and before a gamma correction is applied. Thus, the corresponding pixel values correlate with the (spectrally filtered) observed intensities. The images are stored as [PGM]_

After processing, a CFD file contains the various texture maps and metadata.
Next to this file, 2 file types are used to easily exchange data: CUN (textures bzip2 compressed) and ZUN (textures gunzip compressed). Note that the normal textures are stored in a packed way, allowing a smaller file size.

glTF
====
Overview of existing Single-Camera, Multi-Light file formats
------------------------------------------------------------

Single-Camera, Multi-Light methods output several kinds of files.

* After acquisition: Typically a folder of images, and technical and other meta data.
* After processing:  Intermediate results (e.g. the light positions obtained from Highlight RTI)
* Final exports: These custom file types contain the processed results and can be opened in a custom viewer (or the pixel+ viewer)

   * CUN: PLD's default format containing metadata and losslessly compressed Albedo maps, Ambient maps, and Normal maps. CUN can save up to 6 sides of a recording (Top, Bottom, Left, Right, Front, and back) which are displayed as a `net <https://en.wikipedia.org/wiki/Net_(polyhedron)>`_. Recordings with white light LEDs have RGB Albedo and Ambient maps and a single Normal map. Multispectral recordings have 5 (NIR, R, G, B, NUV) greyscale Albedo and Ambient maps and 5 Normal maps.
   * ZUN: Web optimized version of CUN, resulting in bigger files, but much faster decoding times in the browser.
   * PTM: See `www.loc.gov <https://www.loc.gov/preservation/digital/formats/fdd/fdd000487.shtml>`_
   * RTI: See `www.log.gov <https://www.loc.gov/preservation/digital/formats/fdd/fdd000486.shtml>`_

Need for an open, web optimized format
--------------------------------------

All final Single-Camera, Multi-Light file types (CUN, ZUN, PTM, RTI) contain (technical) meta data and per pixel rastered texture data (PTM coefficients, HSH coefficients, Albedo values, normals...). Efforts have been made to decrease the transfer time (compression) and time to open these files on desktop viewing applications.

Most browsers support WebGL, which can leverage the power of a GPU to efficiently render 3D scenes including the various visual styles of Single-Camera, Multi-Light files. Whereas a CPU implementation has to sequentially loop over all pixels, a GPU implementation can calculate the color value of each texel in parallel.

Before the GPU can be put to work however, the files have to be parsed and decoded, followed by a preparation and transfer of (texture) data from CPU memory to GPU memory. Both steps can take some time - as can be seen when loading CUN, RTI, PTM files in the pixel+ viewer.

glTF
----

`GL Transmission Format <https://www.khronos.org/gltf/>`_ is a specification that tackles this problem of slow runtime decoding and preparing of the data before it can be sent to the GPU.
For Single-Camera, Multi-Light files, a minimal glTF implementation consists of a JSON typed file describing the various textures, including offset and scale values and images containing the per pixel RTI coefficients, PTM coefficients (both grouped as a triplet), Normal, Albedo and/or Ambient values.



.. code-block:: json

    {
      "width": 2513,
      "height": 2190,
      "pld": [
        {
          "ambTex": "PLD_amb.png",
          "albTex": "PLD_alb.png",
          "normalTex": "PLD_nor.png"
        }
      ],
      "hsh": [
        {
          "hshCoef0": [
            {
              "bias": 0.0189997,
              "scale": 1.92338,
              "path": "HSH_hsh0.png"
            }
          ],
          "hshCoef1": [
            {
              "bias": -0.910697,
              "scale": 1.71449,
              "path": "HSH_hsh1.png"
            }
          ],
          "hshCoef2": [
            {
              "bias": -0.2255,
              "scale": 1.09892,
              "path": "HSH_hsh2.png"
            }
          ],
          "hshCoef3": [
            {
              "bias": -0.829841,
              "scale": 1.67125,
              "path": "HSH_hsh3.png"
            }
          ]
        }
      ]
    }  

glTF Conversion Tool
--------------------

coming soon

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
