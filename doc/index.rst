..  DAGMC master file, created by
    sphinx-quickstart on Fri Aug 31 10:08:00 2012.
    You can adapt this file completely to your liking, but it should at least
    contain the root `toctree` directive.

DAGMC: Direct Accelerated Geometry Monte Carlo
==============================================

..  raw:: html
    :file: slideshow.html

|

..  image:: https://github.com/svalinn/DAGMC/actions/workflows/linux_build_test.yml/badge.svg?branch=develop
    :target: https://github.com/svalinn/DAGMC/actions/workflows/linux_build_test.yml

..  image:: https://github.com/svalinn/DAGMC/actions/workflows/mac_build_test.yml/badge.svg?branch=develop
    :target: https://github.com/svalinn/DAGMC/actions/workflows/mac_build_test.yml

..  image:: https://github.com/svalinn/DAGMC/actions/workflows/windows_build_test.yml/badge.svg?branch=develop
    :target: https://github.com/svalinn/DAGMC/actions/workflows/windows_build_test.yml

..  image:: https://github.com/svalinn/DAGMC/actions/workflows/docker_publish.yml/badge.svg?branch=develop
    :target: https://github.com/svalinn/DAGMC/actions/workflows/docker_publish.yml

..  image:: https://anaconda.org/conda-forge/dagmc/badges/version.svg
    :target: https://anaconda.org/conda-forge/dagmc

|

Direct Accelerated Geometry Monte Carlo (DAGMC) is a software package that
allows users to perform Monte Carlo radiation transport directly on CAD models.

DAGMC has been integrated into a variety of Monte Carlo radiation codes.
Implementations are actively maintained from OpenMC_ and MCNP6_. Implementations
are occasionally updated for Geant4_ and FLUKA_.  The last implementation for
Shift_ is still used internally at ORNL. An implementation is available for
MCNP5_. DAGMC was demonstrated in Tripoli4_ in 2010, but not maintained.

DAGMC currently relies on using the commercial solid modeling software Cubit_ (or its
`government-use counterpart <https://cubit.sandia.gov>`_ available from 
Sandia National Laboratories)
to prepare solid models. These packages can be
used to import CAD models from other tools such as SolidWorks, CATIA, etc., or
to create geometry from scratch. DAGMC also relies on Cubit to assign
materials and other geometry-related information.

..  toctree::
    :maxdepth: 2

    install/index
    usersguide/index
    contribute/index
    CHANGELOG

..  toctree::
    :hidden:

    gallery/gallery

..  _CNERG: https://cnerg.github.io
..  _MOAB: https://sigma.mcs.anl.gov/moab-library/
..  _Cubit: https://coreform.com/products/coreform-cubit/
..  _MCNP5: https://mcnp.lanl.gov
..  _MCNP6: https://mcnp.lanl.gov
..  _Geant4: https://geant4.cern.ch
..  _FLUKA: http://www.fluka.org/fluka.php
..  _Tripoli4: https://rsicc.ornl.gov/codes/ccc/ccc8/ccc-806.html
..  _Shift: https://meitner.ornl.gov/doe-codes/shift
..  _OpenMC: https://docs.openmc.org/en/latest/index.html
