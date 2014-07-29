-----------------
Prebuilt Binaries
-----------------

Windows binaries for M2Crypto SSL Python library, courtesy of the `grr <https://code.google.com/p/grr/wiki/BuildingWindowsClient#M2Crypto>`_ project.

They're reproduced here, to improve availability.

- `Python 2.7 64-Bit <M2Crypto-0.21.1-openssl-1.0.1c-py2.7-win-amd64.zip?raw=true>`_
- `Python 2.7 32-Bit <M2Crypto-0.21.1-openssl-1.0.1e-py2.7-win32.zip?raw=true>`_


-----------------------------
Building M2Crypto From Source
-----------------------------

In the event that you want or need to build *M2Crypto* from source, the directions below were those that appeared at the *grr* page above at the time of this writing, for your reference. They have been updated for Markdown.


Instructions from the grr Project
=================================

Components
----------

ActiveState Perl
````````````````

Download and install *ActiveState Perl* from: `ActiveState Perl <http://www.activestate.com/activeperl>`_

*E.g. ActivePerl-5.16.1.1601-MSWin32-x86-296175.msi*

NASM
````

Download and install *NASM* from: `NASM <http://www.nasm.us/>`_

*E.g. nasm-2.10.06-installer.exe*

Swig
````

Download and install *swig* from: `swig <http://www.swig.org/>`_

*E.g. swigwin-2.0.9.zip*

Extract in `C:\\grr-build`

OpenSSL
```````

Grab a copy of the latest version of *OpenSSL* from: `OpenSSL <http://www.openssl.org/source/>`_ and extract it in the build directory.

*E.g. openssl-1.0.1e.tar.gz*

For more info see: `openssl-1.0.1e\\INSTALL.W32` or `openssl-1.0.1e\\INSTALL.W64`

To create a 32-bit build run the following commands from the *Visual Studio* command prompt::

    set PATH=%PATH%;C:\\Program Files\\nasm
    cd C:\\grr-build\\openssl-1.0.1e
    perl Configure VC-WIN32 --prefix=C:\\grr-build\\openssl
    ms\\do_nsam.bat
    nmake -f ms\\ntdll.mak
    nmake -f ms\\ntdll.mak install

To create a 64-bit build run the following commands from the *Visual Studio* command prompt::

    set PATH=%PATH%;C:\\Program Files\\nasm
    cd C:\\grr-build\\openssl-1.0.1e
    perl Configure VC-WIN64A --prefix=C:\\grr-build\\openssl
    ms\\do_win64a.bat
    nmake -f ms\\ntdll.mak
    nmake -f ms\\ntdll.mak install

Note that a `\\` at the end of the prefix path `--prefix=C:\\grr-build\\openssl` can cause `nmake -f ms\\ntdll.mak install` to fail.

This will install *openssl* in `C:\\grr-build\\openssl`

Copy `C:\\grr-build\\openssl\\bin\\libeay32.dll` and `C:\\grr-build\\openssl\\bin\\ssleay32.dll` to `C:\\Python27\\lib\\site-packages\\`.

M2Crypto
````````

Download the M2Crypto source from: `M2Crypto <http://chandlerproject.org/Projects/MeTooCrypto>`_

*E.g. M2Crypto-0.21.1.tar.gz*

Download the patch from: http://code.google.com/p/grr/downloads/detail?name=m2crypto-fixes.patch

To apply the patch run the following commands::

    cd C:\\grr-build\\M2Crypto-0.21.1
    patch -u -p0 < m2crypto-fixes.patch

In setup.py:

- replace self.openssl = 'C:\\pkg' by self.openssl = 'C:\\grr-build\\openssl'
- replace name = 'M2Crypto.__m2crypto' , by name = '__m2crypto' 

Build::

    set PATH=%PATH%;C:\\grr-build\\swigwin-2.0.9
    C:\\Python27\\python.exe setup.py build
    C:\\Python27\\python.exe setup.py install

To get M2Crypto to work for *PyInstaller*:

1. Rename C:\\Python27\\lib\\site-packages\\M2Crypto-0.21.1-py2.7-win32.egg to C:\\Python27\\lib\\site-packages\\M2Crypto-0.21.1-py2.7-win32.egg.zip
2. extract M2Crypto-0.21.1-py2.7-win32.egg.zip into C:\\Python27\\lib\\site-packages\\ without a M2Crypto-0.21.1-py2.7-win32.egg sub directory
3. remove M2Crypto-0.21.1-py2.7-win32.egg.zip 


Testing
-------

You may have issues with getting M2Crypto to run. Test by running::

    C:\\python27\\python.exe -c "import M2Crypto"

I get the error::

    ImportError: DLL load failed: The specified module could not be found.

The import cannot find: `libeay32.dll` and/or `ssleay32.dll` make sure they are in `C:\\Python27\\lib\\site-packages\\`
