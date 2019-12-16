pixel+ 
************

Motivation
==========
Researchers or museum curators who want to study, document or disseminate cultural artifacts have a vast toolbox of HD imaging methods and 3D scanners at hand. A careful choice must be made, depending on the shape, size, location and materiality of the artifact and which characteristic is of interest. In the pixel+ project, we are focusing on single-camera multi-light imaging methods. 

pixel+ aims to bring single camera multi light scanning techniques closer together on several levels of integration:

* Existing processed RTI and PLD files can be opened in one web viewer with their respective viewing modes
* Existing processed RTI and PLD files can be reprocessed so that the viewing modes of the other technology become available.
* Existing RTI and PLD source data can be processed in both an RTI and a PLD pipeline.
* This document provides (background) information on various aspects of single camera multi light scanning techniques.

Integration
===========
Based on processed output files
-------------------------------
.. raw:: html

    <img src="_static/images/integration1.jpg" height="450px">
  
   Given the multitude of collections that have been scanned with both technologies, Pixel+ allows to view processed files (cun, zun for PLD and ptm, rti for RTI) with filters of both technologies. It achieves this by calculating intermediate data file formats like normal maps and ambient maps.

Based on original input files
-------------------------------
.. raw:: html

    <img src="_static/images/integration2.jpg" height="450px">
 
   The best possible form of integration starts from the original input images as, compared to the previous integration method, no information is thrown away. Because both technologies require the same sort of input, i.e. a set of images lighted from various light directions, Pixel+ allows to apply both the PLD as well as the RTI pipeline on both RTI and PLD input data.

PLD is developed with the goal of doing scientific measurements and as such, as much is thouroughly calibrated. RTI itself isn't focussed on extracting surface properties, so using RTI source data to produce PLD results poses some challenges. 
  
Project funding
===============

In 2017 the `RMAH <https://www.artandhistory.museum>`_ and its partners in the pixel+ project received the necessary funding for the implementation of the goals set out. The `Belgian Science Policy Office (BELSPO) <https://www.belspo.be>`_ granted pixel+ this support via their BRAIN-be programme (Pioneer Projects). 

Project partners
================

* `Royal Museums of Art and History (RMAH) <http://www.kmkg-mrah.be/>`_ – coordinator
* `KU Leuven Department of Electical Engineering (ESAT) <https://www.esat.kuleuven.be/psi>`_
* `Illuminare <http://www.illuminare.be/team/>`_
* `KU Leuven Libraries (BD Digitalisering) <https://bib.kuleuven.be/BD/digitalisering-en-document-delivery/digitalisering/digitalisering>`_
* `Royal Library of Belgium (KBR) <https://www.kbr.be/en/>`_


