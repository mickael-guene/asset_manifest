# ASSET PCROP toolset

## How to build
Following installation instructions have been tested on an Ubuntu 14.04 docker
image (docker pull ubuntu:14.04).

* install prerequisites

First install following packages
```
~/pcrop $ apt-get install build-essential texinfo flex bison libncurses5-dev curl
git python wget automake autoconf libtool pkg-config zlib1g-dev libglib2.0-dev libelf-dev bc
```
Then install repo if you don't have it in your path
```
~/pcrop $ mkdir ~/bin
~/pcrop $ PATH=~/bin:$PATH
~/pcrop $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
~/pcrop $ chmod a+x ~/bin/repo
```
* fetch the sources
```
~/pcrop $ repo init -u https://github.com/mickael-guene/asset_manifest -b master
~/pcrop $ repo sync
```
* build toolchain package
```
~/pcrop $ ./scratch/build/scripts/build.sh
```

* build runtime package
```
~/pcrop $ ./scratch/build/scripts/build_runtime.sh
```

At the end of the process you will find two tarballs in the newly created 'out'
directory. One that contains the toolset and the other one that contains
umeq and umeq linker script with testsuite source code.

To speed up your build you can set JOBNB environment variable to the value you
want to use $JOBNB parallel jobs.

You can test your toolset is functional by running sanity check.

* sanity check
```
~/pcrop $ ./scratch/build/scripts/sanity.sh
RUNNING sha256                                            [ OK ]
...
RUNNING test1                                             [ OK ]

TEST RESULT SUMMARY
###################
FAIL                                                      [  0 /  8]
EXPECTED FAIL                                             [  1 /  8]
UNEXPECTED OK                                             [  0 /  8]
OK                                                        [  7 /  8]

full testsuite run in 37.561644438 seconds
~/pcrop $
```

 Results should only show 'OK' or 'EXPECTED FAIL' messages.

## How to install it
* install toolset

For that just untar in a directory the tarball you have generated during compilation.
```
~/pcrop $ mkdir install_test
~/pcrop $ tar -C install_test -xf out/asset-toolset-20160614-063208-aeaab522-armv7-m.tgz
```
* setup path and test your toolset is alive.
```
~/pcrop $ export PATH=~/pcrop/install_test/bin:${PATH}
~/pcrop $ arm-none-eabi-gcc -v