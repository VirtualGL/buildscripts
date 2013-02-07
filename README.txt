DRC's VirtualGL Build Scripts
=============================

These scripts are used to build the "official" VirtualGL binaries, which work
on any Linux platform with GLIB 2.3.2 and later, as well as Windows XP and
later and OS X 10.4 and later.

See BUILDING.txt in the VirtualGL source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 4 64-bit

All development kits necessary to build a 32-bit and a 64-bit version of
VirtualGL (libjpeg-turbo SDK's should be installed in the default location.)

NOTE:  If building on a distro that already has GCC 4, you should edit
buildvgl.linux and remove the lines that say "CC=gcc4" and "CXX=g++4".
Or, you could just create sym links from gcc4 -> gcc and g++4 -> g++.


Build Environment: OS X
-----------------------

Recommended version:  OS X 10.6 (Snow Leopard)

Xcode 3.2.x (available at https://developer.apple.com/downloads -- Apple ID
required)

CMake should be installed somewhere in the PATH.  The version in MacPorts
(http://www.MacPorts.org) works, or just install the CMake application from
the DMG (http://www.cmake.org) and sym link
/Applications/CMake\ {version}.app/Contents/bin/cmake to a directory in your
PATH.

libjpeg-turbo SDK installed in its default location.


Build Environment: Windows
--------------------------

Recommended O/S:  Windows XP 64-bit

CMake should be installed somewhere in the PATH.

All development kits necessary to build a 32-bit and a 64-bit version of
VirtualGL (libjpeg-turbo SDK's for Visual C++ should be installed in the
default location.)


Build Procedure
---------------

Executing

  buildvgl [repository path]

(where repository path is, for instance, "branches/2.2.x", and defaults to
"trunk") will generate both a pristine source tarball and binaries for the
platform on which the script is executed.  These are placed under
$HOME/src/vgl.nightly/YYYYMMDD/files, where YYYYMMDD is a build number based
on today's date.  If the build is successful, then a sym link will be created
from $HOME/src/vgl.nightly/latest to $HOME/src/vgl.nightly/YYYYMMDD.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running 'buildvgl -e'
(assuming that $HOME/src is a shared directory.)

NOTE: On Windows, buildvgl should be run from inside a MinGW shell.  If you
are mounting your home directory as a drive letter (e.g. H:), then set the HOME
environment variable to the MinGW path for that drive (e.g. /h) prior to
running buildvgl.

Run 'buildvgl -h' for usage information.


Distributing a Pre-Release Build
--------------------------------

Executing

  uploadvgl <SourceForge user name>

will upload the files from $HOME/src/vgl.nightly/latest/files to
http://virtualgl.sourceforge.net/vgl.nightly

Having an SSH key installed is highly recommended to avoid having to enter your
password multiple times.
