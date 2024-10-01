# eclipse running on FreeBSD

First of all: eclipse has (sadly) not considered other operating system as Apple, Windows and Linux.  Yes, they are mostly used, but if you look over the edge of the plate, there as also BSD system like FreeBSD, OpenBSD and NetBSD, along with Opensolaris systems like Illumnos and OmniOS, and others.  Many of this systems are very stable and can be used on daily basis, too.

This port is focussed on FreeBSD and eclipse.  It will be the source for (1) a manual compilations as also for (2) integration in the "ports" system of FreeBSD.  And yes, there are some patches to be made, alongside with additional files.  But overall the changes are not huge, they just took a lot of hours to be made.

But why did I do it?  There are also other development environments available.  I made it, becasue I used eclipse since nearly 20 years, before that I was using Visual Age.

I hope you have benefit with this all and fun running eclipse on FreeBSD :-)


## Remarks

To proceed with the build process, I took x86_64 as the architecture, instead of amd64, which is used by FreeBSD.  With x86_64 the build process was done sucesfully, otherwise it stopped always near the end.

For running eclipse, you need the package openjdk17 to be installed.

For building eclipse you need the packages: openjdk17 maven39 gmake git py311-setuptools pkgconf libsecret bsddialog webkit2-gtk3 bison


## Using ports

The folder port-release (where repelase is the eclipse release) contains the portfiles.  Usually it should be used by something like "portsnap" or over the "pkg install".  But it is also possible to build it in a new folder like /usr/ports/java/eclipse-release folder, by copying the content of the port-release into and run

> make makesum
make
make install

or you use the ports data

> cd /usr/ports/java/eclipse
make
make install

or you use (when the new version appears)

> pkg install eclipse


## Manual build

A munaul build is outdated, but I leave it inside :-)

A simple way is to build the eclipse application is a "manual" build.  Download the file manual.tgz, then untar it in a folder and call

./build.sh

It will clone the eclipse aggregator repository, add files and patches, create some binaries and then the long build process.  Finally the result can be found in the folder

eclipse.platform.releng.aggregator/eclipse.platform.releng.tychoeclipsebuilder/eclipse.platform.repository/target/products

By default (I think most people run FreeBSD on Intel or AMD platforms) - take the file org.eclipse.platform.ide-freebsd.gtk.x86_64.tar.gz und untar it in a folder of your choice.  You will get a binary executable eclipse/eclipse.  Adjust your PATH settings, so eclipse can be called on command line, whenever you want.

It should be noted, that a "meven" folder will be created, too.  If everything went fine, delete the build folder, this frees a lot of storage!
