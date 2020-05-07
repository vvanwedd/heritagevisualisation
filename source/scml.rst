SCML
*******************
Overview of existing Single-Camera, Multi-Light file formats
=============================================================

Single-Camera, Multi-Light methods output several kinds of files.

* After acquisition: Typically a folder of images, and technical and other meta data. 
* After processing:  Intermediate results (e.g. the light positions obtained from Highlight RTI) 
* Final exports: These custom file types contain the processed results and can be opened in a custom viewer (or the pixel+ viewer)

   * CUN: PLD's default format containing metadata and losslessly compressed Albedo maps, Ambient maps, and Normal maps. CUN can save up to 6 sides of a recording (Top, Bottom, Left, Right, Front, and back) which are displayed as a `net <https://en.wikipedia.org/wiki/Net_(polyhedron)>`_. Recordings with white light LEDs have RGB Albedo and Ambient maps and a single Normal map. Multispectral recordings have 5 (NIR, R, G, B, NUV) greyscale Albedo and Ambient maps and 5 Normal maps. 
   * ZUN: Web optimized version of CUN, resulting in bigger files, but much faster decoding times in the browser.
   * PTM: See `www.loc.gov <https://www.loc.gov/preservation/digital/formats/fdd/fdd000487.shtml>`_
   * RTI: See `www.log.gov <https://www.loc.gov/preservation/digital/formats/fdd/fdd000486.shtml>`_

Need for an open, web optimized format
======================================

All final Single-Camera, Multi-Light file types (CUN, ZUN, PTM, RTI) contain (technical) meta data and per pixel rastered texture data (PTM coefficients, HSH coefficients, Albedo values, normals...). Efforts have been made to decrease the transfer time (compression) and time to open these files on desktop viewing applications.

Most browsers support WebGL, which can leverage the power of a GPU to efficiently render 3D scenes including the various visual styles of Single-Camera, Multi-Light files. Whereas a CPU implementation has to sequentially loop over all pixels, a GPU implementation can calculate the color value of each texel in parallel. 

Before the GPU can be put to work however, the files have to be parsed and decoded, followed by a preparation and transfer of (texture) data from CPU memory to GPU memory. Both steps can take some time - as can be seen when loading CUN, RTI, PTM files in the pixel+ viewer. 

SCML
=====

`GL Transmission Format <https://www.khronos.org/gltf/>`_ is a specification that tackles this problem of slow runtime decoding and preparing of the data before it can be sent to the GPU.
For Single-Camera, Multi-Light files, a minimal SCML implementation consists of a JSON typed file describing the various textures, including offset and scale values and images containing the per pixel RTI coefficients, PTM coefficients (both grouped as a triplet), Normal, Albedo and/or Ambient values.

.. code-block:: json

   {
   "width": 2513, "height": 2190,
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
    }],
    "hshCoef1": [
    {
     "bias": -0.910697,
     "scale": 1.71449,
     "path": "HSH_hsh1.png"
    }],
    "hshCoef2": [
    {
     "bias": -0.2255,
     "scale": 1.09892,
     "path": "HSH_hsh2.png"
    }],
    "hshCoef3": [
    {
     "bias": -0.829841,
     "scale": 1.67125,
     "path": "HSH_hsh3.png"
    }]
   }

   SCML Conversion Tool
   ====================

   Coming soon
    