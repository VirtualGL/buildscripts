DRC's VirtualGL Build Scripts
=============================

These scripts are used to build the "official" VirtualGL binaries, which work
on any Linux platform with GLIBC 2.12 and later, as well as Windows XP and
later and OS X/macOS 10.7 and later.

See **BUILDING.md** in the VirtualGL source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 6 x86-64

Install all software necessary to build an i386 and an x86-64 version of
VirtualGL (both i386 and x86-64 libjpeg-turbo SDKs should be installed in their
default locations.  Refer to **BUILDING.md** for more information.)

Install a compatible x86-64 Linux hosted/AArch64 Linux target toolchain
(available at
<https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads>)
under **/opt/gcc.arm64**.  Note that 9.2-2019.12 is the latest toolchain that
will run on RHEL 6.

Using `rpm2cpio`, install the following packages from Red Hat or CentOS
Enterprise Linux 7 AArch64 under **/opt/arm64**:

- libglvnd
- libglvnd-devel
- libglvnd-egl
- libglvnd-glx
- libglvnd-opengl
- libstdc++
- libX11
- libX11-devel
- libXau
- libxcb
- libxcb-devel
- libXext
- libXext-devel
- libXi
- libXi-devel
- libXtst
- libXtst-devel
- libXv
- libXv-devel
- mesa-khr-devel
- mesa-libEGL
- mesa-libEGL-devel
- mesa-libGL
- mesa-libGL-devel
- mesa-libGLU
- mesa-libGLU-devel
- xcb-util-keysyms
- xcb-util-keysyms-devel
- xorg-x11-proto-devel

Using `rpm2cpio`, install the AArch64 libjpeg-turbo RPM under **/opt/arm64**.

For convenience, a Docker recipe is available at
<https://github.com/VirtualGL/docker>.


Build Environment: OS X/macOS
-----------------------------

OS X/macOS 10.7 (Lion) or later required

CMake should be installed somewhere in the `PATH`.  The version in MacPorts
(<http://www.MacPorts.org>) works, or just install the CMake application from
the DMG (<http://www.cmake.org>) and add
**/Applications/CMake.app/Contents/bin** to the `PATH`.

Xcode 4.6.x (available at <https://developer.apple.com/downloads> --
Apple ID required.)  The build scripts need this in order to produce VirtualGL
binaries that are backward compatible with OS X 10.7.  Xcode should be
installed under **/Applications/Xcode46.app**.

XQuartz must be installed (XQuartz provides the X11 headers and link
libraries.)

The libjpeg-turbo SDK should be installed in its default location.


Build Environment: Windows
--------------------------

Windows XP 64-bit or later required

CMake (the Windows native version, not the Cygwin version) should be installed
somewhere in the `PATH`.

Install all other software necessary to build a 32-bit and a 64-bit version of
VirtualGL-Utils.  Refer to **BUILDING-win.md** for more information.


Build Procedure
---------------

Executing

    buildvgl [branch/tag]

(where branch/tag is, for instance, "2.4.x" and defaults to "main") will
generate both a pristine source tarball and binaries for the platform on which
the script is executed.  These are placed under
**$HOME/src/vgl.nightly/YYYYMMDD/files**, where YYYYMMDD is a build number
based on today's date.  If the build is successful, then a sym link will be
created from **$HOME/src/vgl.nightly/latest** to
**$HOME/src/vgl.nightly/YYYYMMDD**.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running `buildvgl -e`
(assuming that **$HOME/src** is a shared directory.)

NOTE: On Windows, `buildvgl` should be run from inside an MSYS shell.  If you
are mounting your home directory as a drive letter (e.g. **H:**), then set the
`HOME` environment variable to the MinGW path for that drive (e.g. **/h**)
prior to running `buildvgl`.

Run `buildvgl -h` for usage information.


Signing the Linux Packages
--------------------------

To sign the RPMs and DEBs using a GPG key, create a file called **gpgsign** in
the same directory as **buildvgl**, and include the following contents in the
file:

    GPG_KEY_NAME={full name of GPG key to use (as listed in 'gpg --list-keys')}
    GPG_KEY_ID={key ID of GPG key to use (as listed in 'gpg --list-keys')}
    GPG_KEY_PASS={password for GPG key}

[debsigs](https://gitlab.com/debsigs/debsigs/tags) must be installed and in the
`PATH`.

Signing the Mac Package/DMG
---------------------------

To sign the Mac installer package and DMG, create a file called **macsign**
under **setupscripts/**, and include the following contents in the file:

    MACOS_APP_CERT_NAME={full name of Mac Developer ID Application certficate (in the macOS keychain) used to sign the DMG}
    MACOS_INST_CERT_NAME={full name of Mac Developer ID Installer certificate (in the macOS keychain) used to sign the installer package}

OS X/macOS 10.11 "El Capitan" or later is required in order to sign the Mac
package/DMG.

Signing the Windows Packages
----------------------------

To sign the Windows installers using a code signing certificate, create a file
called **mssign** in the same directory as **buildvgl**, and include the
following contents in the file:

    MS_KEY_FILE={full MinGW path to a .pfx file containing the code signing certificate}
    MS_KEY_PASS={password for certificate}

**signtool** (available in the Windows SDK) must be in the `PATH`.
