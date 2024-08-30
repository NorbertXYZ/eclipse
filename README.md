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

So today I could make a successfull build of the port - at least with a "fake" port called eclipse-4.32 which I created in /usr/ports/java and which run successfully.  I extracted the content of the folder port and run: "make; make install".

There may be small adaptations neccessary - e.g. I put all patches in 1 file.  But it was more conveniant for me.

## Manual build

This may not be the sugested method anymore :-)

A simple way is to build the eclipse application is a "manual" build.  Download the file manual.tgz, then untar it in a folder and call

./build.sh

It will clone the eclipse aggregator repository, add files and patches, create some binaries and then the long build process.  Finally the result can be found in the folder

eclipse.platform.releng.aggregator/eclipse.platform.releng.tychoeclipsebuilder/eclipse.platform.repository/target/products

By default (I think most people run FreeBSD on Intel or AMD platforms) - take the file org.eclipse.platform.ide-freebsd.gtk.x86_64.tar.gz und untar it in a folder of your choice.  You will get a binary executable eclipse/eclipse.  Adjust your PATH settings, so eclipse can be called on command line, whenever you want.

It should be noted, that a "meven" folder will be created, too.  If everything went fine, delete the build folder, this frees a lot of storage!
