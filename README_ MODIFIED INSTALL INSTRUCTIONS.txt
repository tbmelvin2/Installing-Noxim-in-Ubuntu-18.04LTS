MtB modified version of README
=========================

1. Install YAML
2. Install systemC
3. Install Noxim

All the necessary files and folders are present in this repository. These are checked versions so please use them. 
Do not download other versions of 1,2,3 if you are not experimenting yourself. 
All the install instructions are present below. I have modified certain steps.

Noxim - the NoC Simulator
=========================

Installation instructions
-------------------------

This guide will show you how to install Noxim on your computer.


Supported Platforms
-------------------

Noxim is written using the C++ language and the SystemC library, so it is easily portable to
any platform for which SystemC is supported: please refer to their (SystemC) documentation to
know which they are. Just to let you know, we usually work under GNU/Linux (mainly Ubuntu)
but SystemC is known to run under other platforms including Apple Mac OS X and Sun Solaris
with GCC and Microsoft Windows with Visual C++ (but Cygwin with GCC is also known to work).

This document will then detail the steps required to build Noxim from the sources, including
all prerequisites.


PREREQUISITES: C++ compiler, YAML (with Boost Libraries), SystemC

C++ compiler installation
----------------------------------
To compile SystemC you will obviously need a C++ compiler; if you still don't have it, on
 Debian/Ubuntu platforms you may install all the required tools with the following command:

   sudo apt-get install build-essential

YAML installation
----------------------------------
The name of the package depends on the Unix version you are using.

On Ubuntu 14.04, for example, it is possible to install yaml (and the required boost 
libraries) with:

    sudo apt-get install libyaml-cpp-dev libboost-dev

The above command works fine in Ubuntu 18.04LTS.

    ********************************************************************************
    If the packet libyaml-cpp-dev is not available just build yaml-cpp:
    Prerequisites:
    - CMake
        sudo apt-get install cmake

    - Boost libs
        sudo apt-get install libboost-dev

    Then download the code at https://github.com/jbeder/yaml-cpp/archive/master.zip
    and execute the folloqing commands: 

        unzip yaml-cpp-master.zip
        mv yaml-cpp-master yaml-cpp
        cd yaml-cpp
        mkdir lib
        cd lib
        cmake ..
        make
        sudo make install


On MacOSX, when using macports, just type:

    sudo port install yaml-cpp

Please also set properly the YAML path in the Makefile, as follows:
YAML    := /opt/local/

SystemC installation
----------------------------------

To build, install, and use SystemC on UNIX platforms, you need
the following tools:

  1. GNU C++ compiler, version 3.4 or later
     or
     Clang C++ compiler version 3.0 or later

  2. GNU Make (gmake)

GCC, Clang, and gmake are free software that you can
obtain from the following sources:

  GCC           http://www.gnu.org/software/gcc/gcc.html

  Clang         http://clang.llvm.org/

  gmake         http://www.gnu.org/software/make/make.html


- Enter the newly created directory and refer to the file INSTALL which details all the steps
  required for building. The necessary steps from INSTALL file inside systemc are as below:

  1. Go to extracted directory (systemc-2.3.1)

  2. Create a temporary directory, e.g.,

        > mkdir objdir

  3. Change to the temporary directory, e.g.,

        > cd objdir (All the below steps are to be done from inside this folder.)

  4. Choose your compiler by setting the CXX environment variable
     (the configure script tries to guess the default compiler, if
      this step is omitted):

     If you use a POSIX-compatible shell (e.g. bash):
        > export CXX="<compiler>"

     e.g. for GCC compilers (UBUNTU)

        > export CXX=g++

     The Clang compiler is usually named 'clang++', thus e.g.

        > export CXX=clang++

     When using a C shell (e.g. csh/tcsh), the syntax to set the
     environment variable is different:
        > setenv CXX g++
     For the Sun/Solaris Studio compilers, use
        > setenv CXX CC

     You can also specify an absolute path to the compiler of your choice.
     See also the Section "Compilation and Linking Options" in INSTALL file.

  5. Configure the package for your system, e.g.,

     In case you want to install the package in another place than the
     top level directory (systemc-2.3.1), configure the package e.g. as
     follows: (To install inside /usr/local/ folder which is safer as it is not easily modifiable)

        > sudo ../configure --prefix=/usr/local/systemc-2.3.1     

     TO install in same directory (not suggested)

        > sudo ../configure

     While the 'configure' script is running, which takes a few moments, 
     it prints messages to inform you of the features it is checking.
     It also detects the platform.
     
     Note for System V users: 
     If you are using `csh' on an older version of System V, you might 
     need to use the `sh ../configure' command instead of '../configure'.
     Otherwise, `csh' will attempt to `configure' itself.

     SystemC 2.3 includes a fixed-point package that is always built.
     When compiling your applications with fixed-point types, you still have
     to use compiler flag -DSC_INCLUDE_FX. Note that compile times increase
     significantly when using this compiler flag.


  6. Compile the package.

        > sudo gmake

  7. Ignore this as there are lot of errors and corrections required.
     At this point you can either ignore or run but both is not going to affect Noxim installation.
     This step is if you wish to verify the compiled package by
     testing the example suite given along with systemc.

        > gmake check

     This will compile and run the examples in the subdirectory
     examples.

  8. Install the package.

        > sudo gmake install

  9. You can now remove the temporary directory(i.e. objdir), .e.g,

        > cd ..
        > rm -rf objdir

     Alternatively, you can keep the temporary directory to allow you to:

     a) Experiment with the examples.

     b) Later uninstall the package. To clean up the temporary 
        directory, enter:

            > gmake clean

        To uninstall the package, enter:

            > gmake uninstall
  Once you have installed SystemC correctly, you may then jump to the next step.


Build Noxim
-------------

If SystemC is installed correctly, then you just have to compile Noxim.

1) Extract the noxim zip file and go to the "bin" directory.

2) Important STEP. [Change makefile]
In the bin directory edit the file Makefile to modify "SYSTEMC" and "YAML" 
environment variables according to your installation paths. Please, note that the
values already set in the Makefile should be fine if you are using Ubuntu  

The following worked for me because these are my installation paths for YAML and SYSTEMC
##### LIBRARIES CONFIGURATION #####

SYSTEMC := /usr/local/systemc-2.3.1
YAML    := libs/yaml-cpp

###################################

3) > sudo make

You may ignore warning messages (if any), so if you don't get any error you are
ready to run Noxim for the first time using the command:

    ./noxim

If everything works fine, it is now safe for you to copy or move this executable
elsewhere; if you are a maniac of cleaning please note that "make clean" will
also delete the executable... so move it before cleaning!

To load examples go to config_examples folder and see the example files available
	Open terminal and cd to bin directory of Noxim folder
	where there is noxim file and use the following command
	
	> ./noxim -config ../config_examples/"name_of_any_example_file".yaml	

################################################################################

POSSIBLE ERRORS:
--------------

1. Error while loading shared libraries: libsystemc-2.3.1.so

- Environment variables are the same as in MS Windows. 
	> echo $PATH 
	in terminal to see the PATH's content. 
	You cannot link this library without root privileges

-This is a environment setting issue for dynamic linking, because the shared library is 
installed outside of the system default library directories. 
When you execute the binary, the loader failed to find libsystemc-2.3.0.so.

	Follow either of these steps to set path

	setting your LD_LIBRARY_PATH.

	> export LD_LIBRARY_PATH=/usr/local/systemc-2.3.1/lib-linux64:$LD_LIBRARY_PATH

	or, if your default LD_LIBRARY_PATH is empty

	> export LD_LIBRARY_PATH=/usr/local/systemc-2.3.1/lib-linux64

Use the following webpage as reference where the problem was with systemc-2.3.0

https://stackoverflow.com/questions/12408882/error-while-loading-shared-libraries-libsystemc-2-3-0-so

***************************************--------------***********************************
That's all, folks!

