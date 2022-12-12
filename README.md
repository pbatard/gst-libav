# gst-libav

This module contains a GStreamer plugin for using the encoders, decoders,
muxers, and demuxers provided by FFmpeg. It is called gst-libav for historical
reasons.

# LG branch

This specific branch is designed to be used to restore DTS playback on newer
LG webOS based TVs, such as OLED CX models.

# Building

The following assumes that you have cloned this repo and installed the WebOS
SDK from: https://github.com/webosbrew/meta-lg-webos-ndk/releases

```
. /opt/webos-sdk-x86_64/1.0.g/environment-setup-armv7a-neon-webos-linux-gnueabi
./autogen.sh --noconfigure
cd gst-libs/ext/libav/
git am ../../../libav-Force-stereo-downmix-and-integer-output-.patch
cd -
./configure --enable-shared --with-libav-extra-configure="--disable-everything --enable-parser=dca --enable-decoder=dca --enable-decoder=aac --enable-decoder=ac3 --enable-decoder=alac --enable-decoder=amrnb --enable-decoder=amrwb --enable-decoder=eac3 --enable-decoder=flac --enable-decoder=mp3 --enable-decoder=wmapro --enable-decoder=wmav1 --enable-decoder=wmav2 --enable-decoder=wmavoice --enable-decoder=h264" --host=arm-webos-linux-gnueabi --with-sysroot="$SDKTARGETSYSROOT" --prefix="$SDKTARGETSYSROOT" CPPFLAGS="$CPPFLAGS -I$SDKTARGETSYSROOT/include"
make
```

You can then find `libgstlibav.so` under `ext/libav/.libs/`.

# Plugin Dependencies and Licenses

GStreamer is developed under the terms of the LGPL-2.1 (see COPYING file for
details), and that includes the code in this repository.

However, this repository depends on FFmpeg, which can be built in the following
modes using various `./configure` switches: LGPL-2.1, LGPL-3, GPL, or non-free.

This can mean, for example, that if you are distributing an application which
has a non-GPL compatible license (like a closed-source application) with
GStreamer, you have to make sure not to build FFmpeg with GPL code enabled.

Overall, when using plugins that link to GPL libraries, GStreamer is for all
practical reasons under the GPL itself.

The above recommendations are not legal advice, and you are responsible for
ensuring that you meet your licensing obligations.
