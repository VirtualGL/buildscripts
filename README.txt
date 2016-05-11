DRC's VirtualGL Build Scripts
=============================

These scripts are used to build the "official" VirtualGL binaries, which work
on any Linux platform with GLIB 2.5 and later, as well as Windows XP and
later and OS X 10.5 and later.

See BUILDING.md in the VirtualGL source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 5 64-bit

All development kits necessary to build a 32-bit and a 64-bit version of
VirtualGL (libjpeg-turbo SDK's should be installed in the default location.)


Build Environment: OS X
-----------------------

OS X 10.7 (Lion) or later required

Xcode 4.3.x (available at https://developer.apple.com/downloads --
Apple ID required.)  The build scripts need this in order to produce VirtualGL
binaries that are backward compatible with OS X 10.5.  Xcode should be
installed under /Applications/Xcode43.app.

XQuartz must be installed (XQuartz provides the X11 headers and link
libraries.)

CMake should be installed somewhere in the PATH.  The version in MacPorts
(http://www.MacPorts.org) works, or just install the CMake application from
the DMG (http://www.cmake.org) and sym link
/Applications/CMake\ {version}.app/Contents/bin/cmake to a directory in your
PATH.

libjpeg-turbo SDK installed in its default location.


Build Environment: Windows
--------------------------

Windows XP 64-bit or later required

CMake should be installed somewhere in the PATH.

All development kits necessary to build a 32-bit and a 64-bit version of
VirtualGL (libjpeg-turbo SDK's for Visual C++ should be installed in the
default location.)


Build Procedure
---------------

Executing

  buildvgl [branch/tag]

(where branch/tag is, for instance, "2.4.x" and defaults to "master") will
generate both a pristine source tarball and binaries for the platform on which
the script is executed.  These are placed under
$HOME/src/vgl.nightly/YYYYMMDD/files, where YYYYMMDD is a build number based
on today's date.  If the build is successful, then a sym link will be created
from $HOME/src/vgl.nightly/latest to $HOME/src/vgl.nightly/YYYYMMDD.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running 'buildvgl -e'
(assuming that $HOME/src is a shared directory.)

NOTE: On Windows, buildvgl should be run from inside an MSYS shell.  If you
are mounting your home directory as a drive letter (e.g. H:), then set the HOME
environment variable to the MinGW path for that drive (e.g. /h) prior to
running buildvgl.

Run 'buildvgl -h' for usage information.


Signing the Linux Packages
--------------------------

To sign the RPMs and DEBs using a GPG key, create a file called "gpgsign" in
the same directory as buildvgl, and include the following contents in the file:

GPG_KEY_NAME={full name of GPG key to use (as listed in 'gpg --list-keys')}
GPG_KEY_ID={key ID of GPG key to use (as listed in 'gpg --list-keys')}
GPG_KEY_PASS={password for GPG key}

debsigs (available at https://gitlab.com/debsigs/debsigs/tags) must be
installed and in the PATH.


Signing the Windows Packages
----------------------------

To sign the Windows installers using a code signing certificate, create a file
called "mssign" in the same directory as buildvgl, and include the following
contents in the file:

MS_KEY_FILE={full path to a .pfx file containing the code signing certificate}
MS_KEY_PASS={password for certificate}

signtool (available in the Windows SDK) must be in the PATH.


Distributing a Pre-Release Build
--------------------------------

Executing

  uploadvgl <SourceForge user name>

will upload the files from $HOME/src/vgl.nightly/latest/files to
http://virtualgl.sourceforge.net/vgl.nightly

Having an SSH key installed is highly recommended to avoid having to enter your
password multiple times.
