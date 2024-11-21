---
layout: default
title: Scripts
parent: Documentation
permalink: /documentation/scripts

nav_order: 7

---

To ease the installation of some compilers, scripts are available at the root of the XMG-2 installation directory. 

By typing:

    ./reinstall.sh
    
The synsem compiler will be built and installed (all the other contributions will be uninstalled). To add the lex and the mph compiler, one can use the script 

    ./install_lex_mph.sh
    
Other scripts are by convention named after the compiler(s) they install. All scripts starting with `reinstall` will first uninstall all existing compilers, all scripts starting with `install` will add compilers to the already installed set of compilers.

    ./reinstall_all.sh
    
will make all other bricks available (compilers still need to be built).
