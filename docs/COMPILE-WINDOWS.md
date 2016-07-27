We also show steps to cross-compile Unicorn for Microsoft Windows.

To compile for Linux, Mac OS X and Unix-based OS, see [COMPILE-NIX.md](COMPILE-NIX.md)

---


[0] Dependencies

For Windows, cross-compile requires Mingw. Mingw-glib2 is needed.
At the moment, it is confirmed that Unicorn can be compiled either on Ubuntu
or Windows.

- On Ubuntu 14.04 64-bit, do:

  - Download DEB packages for Mingw64 from:

  https://launchpad.net/~greg-hellings/+archive/ubuntu/mingw-libs/+build/2924251

  - To cross-compile for Windows 32-bit, install Mingw with (ignore all the warnings):

          $ sudo dpkg -i --force-depends mingw64-x86-glib2_2.31.0_all.deb

  To cross-compile for Windows 64-bit, install Mingw with:

        $ sudo dpkg -i --force-depends mingw64-x64-glib2_2.31.0_all.deb


- On Windows, install MinGW via package MSYS2 at https://msys2.github.io/

  Follow the install instructions and don't forget to update the system packages with:

        $ pacman --needed -Sy bash pacman pacman-mirrors msys2-runtime

  Then close MSYS2, run it again from Start menu and update the rest with:

        $ pacman -Su

  Finally, install required toolchain to build C projects.

  - To compile for Windows 32-bit, run:

          $ pacman -S python2
          $ pacman -S make
          $ pacman -S pkg-config
          $ pacman -S mingw-w64-i686-glib2
          $ pacman -S mingw-w64-i686-toolchain

  - To compile for Windows 64-bit, run:

          $ pacman -S python2
          $ pacman -S make
          $ pacman -S pkg-config
          $ pacman -S mingw-w64-x86_64-glib2
          $ pacman -S mingw-w64-x86_64-toolchain

- For Cygwin, "make", "gcc-core", "pkg-config", "libpcre-devel", "zlib-devel"
  and "libglib2.0-devel" are needed.

  If apt-cyg is available, you can install these with:

        $ apt-cyg install make gcc-core pkg-config libpcre-devel zlib-devel libglib2.0-devel



[1] Tailor Unicorn to your need.

Out of 6 archtitectures supported by Unicorn (Arm, Arm64, M68K, Mips, Sparc,
& X86), if you just need several selected archs, choose which ones you want
to compile in by editing "config.mk" before going to next steps.

By default, all 6 architectures are compiled.

The other way of customize Unicorn without having to edit config.mk is to
pass the desired options on the commandline to ./make.sh. Currently,
Unicorn supports 4 options, as follows.

  - UNICORN_ARCHS: specify list of architectures to compiled in.
  - UNICORN_STATIC: build static library.
  - UNICORN_SHARED: build dynamic (shared) library.
  - UNICORN_QEMU_FLAGS: specify extra flags for qemu's configure script

To avoid editing config.mk for these customization, we can pass their values to
make.sh, as follows.

        $ UNICORN_ARCHS="arm aarch64 x86" ./make.sh

NOTE: on commandline, put these values in front of ./make.sh, not after it.

For each option, refer to docs/README for more details.



[2] Compile from source on Windows - with MinGW (MSYS2)

To compile with MinGW, install MSYS2 as instructed in the first section.
Then, build Unicorn with the next steps:

- To compile Windows 32-bit binary with MinGW, run:

        $ ./make.sh cross-win32

- To compile Windows 64-bit binary with MinGW, run:

        $ ./make.sh cross-win64

Resulted files unicorn.dll, unicorn.lib & samples/sample*.exe can then
be used on Windows machine.

To run sample_x86.exe on Windows 32-bit, you need the following files:

        unicorn.dll
        %MSYS2%\mingw32\bin\libiconv-2.dll
        %MSYS2%\mingw32\bin\libintl-8.dll
        %MSYS2%\mingw32\bin\libglib-2.0-0.dll
        %MSYS2%\mingw32\bin\libgcc_s_dw2-1.dll
        %MSYS2%\mingw32\bin\libwinpthread-1.dll

To run sample_x86.exe on Windows 64-bit, you need the following files:

        unicorn.dll
        %MSYS2%\mingw64\bin\libiconv-2.dll
        %MSYS2%\mingw64\bin\libintl-8.dll
        %MSYS2%\mingw64\bin\libglib-2.0-0.dll
        %MSYS2%\mingw64\bin\libgcc_s_seh-1.dll
        %MSYS2%\mingw64\bin\libwinpthread-1.dll



[3] Compile and install from source on Cygwin

To build Unicorn on Cygwin, run:

        $ ./make.sh

After compiling, install Unicorn with:

        $ ./make.sh install

Resulted files cygunicorn.dll, libunicorn.dll.a and libunicorn.a can be 
used on Cygwin but not native Windows.

NOTE: The core framework installed by "./make.sh install" consist of
following files:

        /usr/include/unicorn/*.h
        /usr/bin/cygunicorn.dll
        /usr/lib/libunicorn.dll.a
        /usr/lib/libunicorn.a



[4] Cross-compile for Windows from *nix

To cross-compile for Windows, Linux & gcc-mingw-w64-i686 (and also gcc-mingw-w64-x86-64
for 64-bit binaries) are required.

- To cross-compile Windows 32-bit binary, simply run:

        $ ./make.sh cross-win32

- To cross-compile Windows 64-bit binary, run:

        $ ./make.sh cross-win64

Resulted files unicorn.dll, unicorn.lib & samples/sample*.exe can then
be used on Windows machine.

To run sample_x86.exe on Windows 32-bit, you need the following files:

        unicorn.dll
        /usr/i686-w64-mingw32/sys-root/mingw/bin/libglib-2.0-0.dll
        /usr/lib/gcc/i686-w64-mingw32/4.8/libgcc_s_sjlj-1.dll
        /usr/i686-w64-mingw32/lib/libwinpthread-1.dll

To run sample_x86.exe on Windows 64-bit, you need the following files:

        unicorn.dll
        /usr/x86_64-w64-mingw32/sys-root/mingw/bin/libglib-2.0-0.dll
        /usr/lib/gcc/x86_64-w64-mingw32/4.8/libgcc_s_sjlj-1.dll
        /usr/x86_64-w64-mingw32/lib/libwinpthread-1.dll

Then run either "sample_x86.exe -32" or "sample_x86.exe -64" to test emulators for X86 32-bit or X86 64-bit.
For other architectures, run "sample_xxx.exe" found in the same directory.



[5] Language bindings

Look for the bindings under directory bindings/, and refer to README file
of corresponding languages.



[6] Unit tests

Automated unit tests use the cmocka unit testing framework (https://cmocka.org/).
It can be installed in most Linux distros using the package manager, e.g.
`sudo yum install libcmocka libcmocka-devel`, or you can easily build and install it from source.

You can run the tests by running `make test` in the project directory.