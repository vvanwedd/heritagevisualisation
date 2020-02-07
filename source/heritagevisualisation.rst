Introduction
*************
About
######

heritage-visualisation.org website is an initiative of the pixel+ project to promote enhanced visualisation solutions for the heritage community. It provides an overview of heritage scanning methods and focusses on one in particular: Single-Camera, Multi-Light technology.

Several methods to capture and study heritage artifacts with the Single-Camera, Multi-Light technology exist. This website aims to bring together existing methods. To achieve this a new file format standard and integrated viewer platform have been created. 

The website hosts the WebGL based `pixel+ viewer <http://www.heritage-visualisation.org/viewer/viewer.php>`_ which can handle several single-camera, multi-light datasets. This viewer allows the consultation of existing CUN, ZUN, PTM and RTI datasets and as a first uses the newly proposed glTF `glTF <http://www.heritage-visualisation.org/gltf.rst>`_file format standard. 

heritage-visualisation.org is maintained by a group of researchers in the Computer Vision and Cultural Heritage domains. The group focuses on the development and dissemination of new technologies for visualisation and has its origin in Belgium.

PIC PIC PIC PIC PIC PIC

Heritage Visualisation Methods
###############################

3D Reconstruction Methods
=========================

Plenty of methods exist to reconstruct a 3D representation of cultural artefacts. Depending on the used technology, some methods work better on objects with certain shapes and materials than others. Most of the time, these algorithms have parameters that the user should set and scanning methodologies that the user should follow carefully to obtain optimal results. 
The output file type can be open or closed, widely popular or scarcely used. 

The ideal 3D Reconstruction is a very small sized file containing all information of the object, i.e. a true virtual clone. In reality, 3D reconstructions only contain a small fraction of the information of an object. A mesh or point cloud contains information about the shape of an object's surface, but not what's inside. A 3D vector field, like the output of a X-ray microtomography scan, contains volume information. A 3D model can include an appearance models. The appearance of a material is caracterized by how that material interacts with light. Appearance models can vary between a single RGB color and a highly dimensional radiometric function describing how incident energy is scattered at a surface. For example, the appearance of the wings of butterflies cannot be synthetized into a single RGB color without loss of information, but require a higher dimensional scattering function.

In what follows in this section, we follow the taxonomy of methods for 3D shape extraction as described by 

Passive Methods
+++++++++++++++
Single Vantage Point Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Shape from Texture
------------------
Shape from Occlusion
--------------------
Shape from Defocus
-------------------
Shape from contour
------------------
Time to Contact
---------------
Multiple Vantage Points Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Passive Stereo
--------------
2 images of an object, taken at the same time but from different viewpoints. Typically the camera parameters (i.e. positions, orientations, focal lengths, radial and tangential distortion coefficients) are known, such that the position of a 3D point can be found using `triangulation <https://en.wikipedia.org/wiki/Triangulation_(computer_vision)>`_. This process can be repeated for every matched 2D keypoint pair in both images, resulting in a (sparse) point cloud. For a more in depth understanding of stereo vision and its extensions, the reference work on this topic is `Multiple View Geometry in computer vision <https://www.cambridge.org/core/books/multiple-view-geometry-in-computer-vision/0B6F289C78B2B23F596CAA76D3D43F7A>`_. The hardware typically consists of 2 cameras, placed in a rigid enclosure. The disparity (distance between the cameras) and camera parameters are typically chosen depending on the object to be scanned.

Structure from Motion
---------------------
For stationary objects, SfM allows the user to scan the object by taking several photos while moving a (regular consumer) camera. Depending on the exact algorithm and assumptions, the self-calibrated algorithm only needs a couple of images to estimate the camera parameters.
The hardware typically only consists of a single camera, although sometimes a collection of lenses and a lighting setup can be used to improve the results (of next parts of the pipeline). Free and Open Source implementations include `OpenSfM <https://github.com/mapillary/OpenSfM>`_ and `OpenMVG <https://github.com/openMVG/openMVG>`_. The result is a (sparse) point cloud and calibrated cameras. To get a dense point cloud or more detailed mesh reconstruction, Multi-View stereo libraries like `OpenMVS <https://github.com/cdcseacave/openMVS>`_ can be used. SfM is the first step of the popular software `Agisoft Metashape <https://www.agisoft.com/>`_.

Shape from Silhouettes
----------------------
Voxel carving, visual hull

Active Methods
++++++++++++++
Single Vantage Point Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Time of Flight
--------------
A Time of Flight camera sends out coded signals (e.g. using NIR LEDs or lasers) and detects the round trip delay to the particular point, corresponding to the point's distance to the sensor. Typically the spatial resolution is much lower than other methods, as the sensor

Shape from Shading
------------------
Multiple Vantage Points Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Structured Light
----------------
If the object to be scanned doesn't have enough textural variation or small 3D variation, feature detection and feature matching, 2 steps in the passive stereo algorithm, might fail. By replacing one of the cameras with a projector, a series of grids (e.g. a Gray code pattern sequence) or a single grid (e.g. for non-rigid objects) can be projected onto the object. Typically the camera-projector pair is, like the camera-camera pair of passive stereo, fully calibrated, so that triangulation becomes a relatively easy problem.

Active Stereo
--------------
Photometric Stereo
-------------------

Multi-Light Technology
=====================================================

Single-Camera Multi-Light technology is a well studied research topic. This website and the pixel+ viewer focuses on PTM, HSH RTI, RELIGHT RTI and PLD. For a more in depth overview of these types, see :ref:`singlecameramultilight:Single-Camera, Multi-Light Technology`. Other RTI interpolation models for photo realistic relighting include Spherical Harmonics, Discrete Modal Decomposition and Deep Learning methods. From the set of multi light images directly or from the coefficients of the interpolation models, non photo realistic viewing styles have been developed to accentuate and reveal surface details. PLD follows a different approach and disentangles the shape and appearance information. The shape is modeled based on Photometric Stereo, whereas the appearance information is represented as a sparsely sampled lower dimensional BRDF. Shape and appearance modeling is studied in the fields of Computer Vision, Computer Graphics, Digital Heritage, and Optics and less relevant for heritage visualisation in Medical Imaging, Remote Sensing, Astrophysics, etc. Below is a compiled list of related material for background reading.

.. list-table:: Single Camera Multi Light Background Material
   :widths: 75 25
   :header-rows: 1

   * - Paper
     - Keywords
   * - Malzbender, T., Gelb, D., & Wolters, H. (2001, August). Polynomial texture maps. In Proceedings of the 28th annual conference on Computer graphics and interactive techniques (pp. 519-528).
     - PTM, RTI, Photorealistic Relighting
   * - Mudge, M., Malzbender, T., Chalmers, A., Scopigno, R., Davis, J., Wang, O., ... & Barbosa, J. (2008). Image-Based Empirical Information Acquisition, Scientific Reliability, and Long-Term Digital Preservation for the Natural Sciences and Cultural Heritage. Eurographics (Tutorials), 2(4).
     - PTM, HSH, RTI, Photorealistic Relighting
   * - Pitard, G., Le Goïc, G., Mansouri, A., Favrelière, H., Desage, S. F., Samper, S. & Pillet, M. (2017). Discrete Modal Decomposition: a new approach for the reflectance modeling and rendering of real surfaces. Machine Vision and Applications, 28(5-6), 607-621.
     - RTI, DCT, Photorealistic Relighting
   * - Drew, M. S., Hel-Or, Y., Malzbender, T., & Hajari, N. (2012). Robust estimation of surface properties and interpolation of shadow/specularity components. Image and Vision Computing, 30(4-5), 317-331.
     - PTM, RTI, Photorealistic Relighting
   * - Woodham, R. J. (1980). Photometric method for determining surface orientation from multiple images. Optical engineering, 19(1), 191139.
     - Photometric Stereo, Shape Modeling
   * - Ackermann, J., & Goesele, M. (2015). A survey of photometric stereo techniques. Foundations and Trends® in Computer Graphics and Vision, 9(3-4), 149-254.
     - Photometric Stereo, Shape Modeling, Depth Integration
   * - Hameeuw, H., Willems, G., Verbiest, F., Moreau, W., Van Lerberghe, K., & Van Gool, L. (2005). Easy and cost-effective cuneiform digitizing. In The 6th International Symposium on Virtual Reality, Archaeology and Cultural Heritage (VAST 2005) (pp. 73-80). Eurographics Association.
     - PLD, Photometric Stereo, Photorealistic Relighting
   * - Verbiest, F., Willems, G., & Van Gool, L. (2006). Image-based rendering for photo-realistic visualization. Virtual and Physical Prototyping, 1(1), 19-30.
     - PLD, Photometric Stereo, Photorealistic Relighting
   * - Willems, G., Verbiest, F., Vergauwen, M., & Van Gool, L. (2005, June). Real-time image based rendering from uncalibrated images. In Fifth International Conference on 3-D Digital Imaging and Modeling (3DIM'05) (pp. 221-228). IEEE 2005
     - PLD, Photometric stereo, Photorealistic Relighting
   * - Hameeuw, H., & Willems, G. (2011). New visualization techniques for cuneiform texts and sealings. Akkadica, 132(2), 163-178.
     - PLD, Photometric stereo
   * -  Watteeuw, L., Vandermeulen, B., & Proesmans, M. (2015). On the surface and beyond. an new approach with multispectral photometric stereo to assess illuminated manuscripts and their condition. Science and Engineering in Arts, Multispectral Imaging Heritage and Archaeology, book of abstracts, 1, 103-103.
     - PLD, Photometric Stereo, Multispectral Imaging, Photorealistic Relighting
   * - Van der Perre, A., Hameeuw, H., Boschloos, V., Delvaux, L., Proesmans, M., Vandermeulen, B., ... & Watteeuw, L. (2016). Towards a combined use of IR, UV and 3D-Imaging for the study of small inscribed and illuminated artefacts. Multispectral Imaging Lights on… Cultural Heritage and Museums!, 163-192.
     - PLD, Photometric Stereo, Multispectral Imaging, Photorealistic Relighting
   * - Vandermeulen, B., Hameeuw, H., Watteeuw, L., Van Gool, L., & Proesmans, M. (2018, April). Bridging Multi-light & Multi-Spectral images to study, preserve and disseminate archival documents. In Archiving Conference (Vol. 2018, No. 1, pp. 64-69). Society for Imaging Science and Technology.
     - PLD, Photometric Stereo, Multispectral Imaging, Photorealistic Relighting
   * - Hameeuw, H., Vanweddingen, V., Van Gool, L., Proesmans, M., Vastenhoud, C., Van Der Perre, A., Vandermeulen, B. and Watteeuw, G. Pixel : Visualising Our Heritage. 2018. DH Benelux.
     - PLD, PTM, HSH, RTI, Photorealistic Relighting
   * - Vanweddingen, V., Vastenhoud, C., Proesmans, M., Hameeuw, H., Vandermeulen, B., Van der Perre, A., Lemmers, F., Watteeuw, L., Van Gool, L. A Status Quaestionis and Future Solutions for Using Multi-Light Reflectance Imaging Approaches for Preserving Cultural Heritage Artifacts. Digital Heritage. Progress in Cultural Heritage: Documentation, Preservation, and Protection. EuroMed 2018. Lecture Notes in Computer Science, vol. 11197, 2018, pp. 204–211.
     - PLD, PTM, HSH, RTI, Photorealistic Relighting
   * - Hameeuw, H., Vanweddingen, V., Proesmans, M., Vastenhoud, C.,  Vandermeulen, B., Van der Perre, A., Watteeuw, L., Lemmers, F.,  Van Gool, L., Schroer, C., Mudge, M., Earl, G. Portable Light Domes in PIXEL+: Acquisition, Viewing, and Analysis. Digital Heritage 2018 3rd International Congress & Expo (San Fransisco)
     - PLD, PTM, HSH, RTI, Photorealistic Relighting, Data Preservation
   * - Hameeuw, H., Vanweddingen, V.,  Vandermeulen, B., Vastenhoud, C., Watteeuw, L., Lemmers, F., Van der Perre, A., Konijn, P., Van Gool, L., Proesmans, M. PIXEL+: integrating and standardizing of various interactive pixel-based imagery. SPIE Optics, Photonics and Digital Technologies for Imaging Applications VI 2020
     - PLD, PTM, HSH, RTI, RELIGHT, Photorealistic Relighting, Data Preservation



- PTM/RTI:
    - Zhang, M., & Drew, M. S. (2014). Efficient robust image interpolation and surface properties using polynomial texture mapping. EURASIP Journal on Image and Video Processing, 2014(1), 25.
    - MacDonald, L. W. (2015). Realistic visualisation of cultural heritage objects (Doctoral dissertation, UCL (University College London)).
    - Ponchio, F., Corsini, M., & Scopigno, R. (2018, June). A compact representation of relightable images for the web. In Proceedings of the 23rd International ACM Conference on 3D Web Technology (pp. 1-10).
    - Irina, M. C., Tinsae, G. D., Andrea, G., Ruggero, P., Alberto, J. V., & Enrico, G. (2018, June). Artworks in the spotlight: characterization with a multispectral LED dome. In IOP Conference Series: Materials Science and Engineering (Vol. 364, No. 1, p. 012025). IOP Publishing.
    - Pintus, R., Giachetti, A., Pintore, G., & Gobbetti, E. (2017). Guided robust matte-model fitting for accelerating multi-light reflectance processing techniques.
    -

    - Peter, F., Andrea, B., Aeneas, K., & Lukas, R. (2017). Enhanced RTI for gloss reproduction. Electronic Imaging, 2017(8), 66-72.



- Photometric Stereo:


    - Basri, R., Jacobs, D., & Kemelmacher, I. (2007). Photometric stereo with general, unknown lighting. International Journal of computer vision, 72(3), 239-257.


- Multi-Light:
    - Fattal, R., Agrawala, M., & Rusinkiewicz, S. (2007). Multiscale shape and detail enhancement from multi-light image collections. ACM Transactions on Graphics (TOG), 26(3), 51.
    - Zheng, J., Li, Z., Rahardja, S., Yao, S., & Yao, W. (2010, March). Collaborative image processing algorithm for detail refinement and enhancement via multi-light images. In 2010 IEEE International Conference on Acoustics, Speech and Signal Processing (pp. 1382-1385). IEEE.
    - Raskar, R., Tan, K. H., Feris, R., Yu, J., & Turk, M. (2004). Non-photorealistic camera: depth edge detection and stylized rendering using multi-flash imaging. ACM transactions on graphics (TOG), 23(3), 679-688.
    - Cosentino, A., Stout, S., & Scandurra, C. (2015). Innovative imaging techniques for examination and documentation of mural paintings and historical graffiti in the catacombs of San Giovanni, Syracuse. International Journal of Conservation Science, 6(1), 23-34.



Infrared Photography
====================

- Cosentino, Antonino. (2016). Infrared Technical Photography for Art Examination. e-Preservation Science. 13. 1-6. `Researchgate <https://www.researchgate.net/publication/295086868_Infrared_Technical_Photography_for_Art_Examination>`_

Multispectral Imaging
=========================

- MacDonald, L.W., Vitorino, T., Picollo, M. et al. Assessment of multispectral and hyperspectral imaging systems for digitisation of a Russian icon. Herit Sci 5, 41 (2017) `doi:10.1186/s40494-017-0154-1 <https://doi.org/10.1186/s40494-017-0154-1>`_

Optical Coherence Tomography
============================

- Targowski, P. & Iwanicka, M. Appl. Phys. A (2012) 106: 265. `doi: 10.1007/s00339-011-6687-3 <https://doi.org/10.1007/s00339-011-6687-3>`_

Phase-Contrast X-ray Imaging
============================

- Albertin, Fauzia & Astolfo, Alberto & Peccenini, Eva & Hwu, Yeukuang & Kaplan, Frederic & Margaritondo, G.. (2015). Ancient administrative handwritten documents: X-ray analysis and imaging. Journal of Synchrotron Radiation. 22. `doi: 10.1107/S1600577515000314 <https://doi.org/10.1107/S1600577515000314>_

Photogrammetry
==============

Radiography
===========
Raking Light Illumination
=========================
Terahertz Imaging
=================

- Gillian C. Walker, John W. Bowen, Wendy Matthews, Soumali Roychowdhury, Julien Labaune, Gerard Mourou, Michel Menu, Ian Hodder, and J. Bianca Jackson, "Sub-surface terahertz imaging through uneven surfaces: visualizing Neolithic wall paintings in Çatalhöyük," Opt. Express 21, 8126-8134 (2013) `doi:10.1364/OE.21.008126 <https://doi.org/10.1364%2FOE.21.008126>`_

- Pastorelli, G., Trafela, T., Taday, P. F., Portieri, A., Lowe, D., Fukunaga, K., & Strlič, M. (2012). Characterisation of historic plastics using terahertz time-domain spectroscopy and pulsed imaging. Analytical and bioanalytical chemistry, 403(5), 1405-1414. `doi: 10.1007/s00216-012-5931-9 <https://doi.org/10.1007/s00216-012-5931-9>`_

- `"Terahertz for Conservation of Paintings, Manuscripts and Artefacts" <https://web.archive.org/web/20130603025727/http://www.teraview.com/applications/nondestructive-testing/art.html>`_. TeraView. Archived from the original on 2013-06-03.

Ultraviolet Photography
=======================

X-ray Fluorescence
==================

- Beckhoff, B., Kanngießer, B., Langhoff, N., Wedell, R., & Wolff, H. (Eds.). (2007). Handbook of practical X-ray fluorescence analysis. Springer Science & Business Media. `www.springer.com <https://www.springer.com/gp/book/9783540286035>`_


X-ray Microtomography
=====================

- Hain, M., Bartl, J., & Jacko, V. (2017, May). Use of X-ray microtomography and radiography in cultural heritage testing. In 2017 11th International Conference on Measurement (pp. 119-122). IEEE. `doi: 10.23919/MEASUREMENT.2017.7983550 <https://doi.org/10.23919/MEASUREMENT.2017.7983550>`_
