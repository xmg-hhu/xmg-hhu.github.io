---
layout: page
title: Getting started
parent: Documentation
permalink: /documentation/getting-started

nav_order: 1

---

This section presents the different ways XMG can be used, and how to use it to generate a first resource from a toy example.
# Installation

There are several ways to use XMG: it can be installed on the system (only for linux users), used on a virtual system, or through a webpage. The first two options are recommanded for developing large scale resources. 
## Option 1: standard installation

if you are using a Debian based distribution (like Ubuntu), open a terminal and follow the following steps:

###  Git:

![alt text](https://raw.githubusercontent.com/spetitjean/XMG-2/master/Doc/images/git.png)

    sudo apt-get install git

### Download and install Gecode (4.0 and latest not supported yet):

![alt text](https://raw.githubusercontent.com/spetitjean/XMG-2/master/Doc/images/gecode.png)

From here: [Gecode](http://www.gecode.org/download.html) (recommended: [Gecode 3.7.3](https://github.com/Gecode/gecode/releases/tag/release-3.7.3), also available here: [Gecode 3.7.3](https://drive.google.com/uc?export=download&id=0B2gwCa-ajJXmOUd0VHBZYnNTZ2c)).

    ./configure --disable-qt --disable-gist
    make 
You should read the following if the installation is successful:        
    
    Compilation of Gecode finished successfully. To use Gecode, either add 
    /.../gecode-3.7.3 to your search path for libraries, or install Gecode using
    make install       
    
Then, as suggested, you can type: 

    make install    

Note that you might need to throw this command as superuser (`sudo make install`).
If the installation succeeds, you should be able to run Gecode, you can try it by typing:

    ./examples/queens
    
If the installation fails, there is probably a dependency missing. To install them: 

    sudo apt-get install g++    
    sudo apt-get install make
###  Download and install YAP (Yet Another Prolog):

    git clone https://github.com/spetitjean/yap-6.3.git

Then:

    ./configure --without-readline --without-minisat
    make 
    make install   
For dependencies: 

    apt-get install libgmp3-dev
### Install Python3 (>3.2):

![alt text](https://raw.githubusercontent.com/spetitjean/XMG-2/master/Doc/images/python.png)

    sudo apt-get install python3 python3-yaml python3-pyqt4

### Download XMG:

    git clone https://github.com/spetitjean/XMG-2.git    
You can also get it as an archive here: [XMG-2](https://drive.google.com/uc?export=download&id=0B2gwCa-ajJXmRmNfU1FvRFpwOGM) (this solution will not allow you to update XMG-2).

### Add XMG-2 to your PATH

Edit your `~/.bashrc` file and add this line (your path_to_xmg should be for example `~/XMG-2`):

    export PATH=path_to_xmg:$PATH    
To edit the `bashrc` file, you can type:

    emacs ~/.bashrc

## Option 2: using Virtualbox

A VirtualBox image of XMG is available for an easier installation.
Use [VirtualBox](https://www.virtualbox.org/) and download one of the XMG virtual images:

*  [Ubuntu 18.04 virtual image](https://www.dropbox.com/s/nltmtbxram2yd73/XMG-Ubuntu-18.04.ova?dl=1): includes the parser TuLiPA-frames (default password is xmg) - last updated 06/06/2018.

* [Ubuntu 18.04 virtual image, updated](https://uni-duesseldorf.sciebo.de/s/2kxtivg4hJ3HO47): includes the parser TuLiPA-frames (default password is xmg) - last updated 28/07/2023. 

*  [Xubuntu 18.04 virtual image](https://www.dropbox.com/s/knkg4qtfld3ir4g/XMG-Xubuntu-18.04.ova?dl=1): lighter version, includes the parser TuLiPA-frames (default password is xmg) - last updated 30/05/2018.

##  Using XMG without installing anything

An online compiler is available at this address: [http://xmg.phil.hhu.de/index.php/upload/workbench](http://xmg.phil.hhu.de/index.php/upload/workbench).


# Updating XMG-2

To get the latest version of XMG-2, regardless of the installation option you chose, you can type this command (from the XMG-2 directory):
    
    git pull
#  Creating a first compiler

The instructions detailed here is equivalent to using the script **reinstall.sh** (see page [Scripts](https://github.com/spetitjean/XMG-2/wiki/7:-Scripts)). This means that you can skip this section by only typing:

    ./reinstall.sh
    (at the root of the XMG-2 installation directory)

Before compiling a metagrammar, a compiler needs to be created. XMG-2 assembles compilers by combining compiler fragments called bricks. These bricks are distributed into packages called contributions. For example: 

*  the contribution `core` provides bricks offering support for the basic features of a compiler

*  the contribution `treemg` makes it possible to process tree descriptions

*  the contribution `synsemCompiler` makes the synsem compiler (equivalent to XMG-1) available

Installing a contribution, with the command `install` makes all the bricks of this contribution available for being assembled.

First, install the needed contributions for the synsem compiler:

    xmg bootstrap               
    cd contributions            
    xmg install core          
    xmg install treemg         
    xmg install compat         
    xmg install synsemCompiler  

Then, build the compiler:

    cd synsemCompiler/compilers/synsem
    xmg build


After these operations, the compiler synsem (Tree Adjoining Grammar with semantics based on predicate logic) is available.

#  Compiling a toy-metagrammar

The XMG system includes a toy metagrammars that we highly recommend to manipulate. The files containing these metagrammars should be in the Metagrammars directory of the XMG installation. To compile one of the synsem examples (adapted to the compiler we just built), just type:

    xmg compile synsem MetaGrammars/synsem/TagExample.mg
(see also List of XMG's options below)
The result of this compilation will be a file named TagExample.xml.

To launch the GUI, type:

    xmg gui tag

You can then open the grammar file (.xml) which was generated by the compiler (Fichier -> Ouvrir un XML).

#  Compiling an existing metagrammar

To compile metagrammars which were created using XMG1, it is usually necessary to use the `--notype` option to cancel the type checking steps which did not exist in XMG1.    

