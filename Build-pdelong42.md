Things I had to do to get this working for me...

In the examples below, `BASEDIR=/home/pdelong/Stuff/Install`, on my Fedora 21
install, and `BASEDIR=/Users/pd72/Stuff/Install` on my work MacBook.

I had to rebuild ffmpeg, and install a local copy of it in my home directory,
for use by this Node app.  The ffmpeg that comes with Fedora Linux is not built
to link against the vo-aacenc libraries.

Here's the command-line for building ffmpeg:
```
   xargs sh configure <<EOF
   --extra-cflags=-I${BASEDIR}/include
   --extra-ldflags=-L${BASEDIR}/lib
   --prefix=$BASEDIR
   --enable-libvo-aacenc
   --enable-version3
   --disable-yasm
   --enable-libx264
   --enable-gpl
   EOF

   make install
```
Prior to that, I also had to download, build, and install the vo-aacenc
libraries from the opencore-amr sourceforge page: 

   http://sourceforge.net/projects/opencore-amr/

However, the highlighted download is not the one you want.  You should click
the "Browse All Files" link right below that, then grab the latest tarball in
the "vo-aacenc" folder (or just use this link):

   http://sourceforge.net/projects/opencore-amr/files/vo-aacenc/

Here's the command-line for building vo-aacenc:
```
   sh configure --prefix=${BASEDIR} && make install
```
I had to override PATH and LD_LIBRARY_PATH when running the Node app:
```
   env PATH=${BASEDIR}/bin:${PATH} LD_LIBRARY_PATH=${BASEDIR}/lib node app
```
This needed to be run on my Linux workstation, and accessed from my laptop,
because the laptop would have quickly overheated from all the ffmpeg activity
(plus, the workstation has eight cores, and I think my laptop only has two).  
My workstation can't be used as the client, because it doesn't have WiFi.
