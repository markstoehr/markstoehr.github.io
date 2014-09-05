---
layout: post
title: Installing HTK on 64-bit Architectures
---

[HTK](http://htk.eng.cam.ac.uk/) is an important speech recognition tool
maintained and founded by the Cambridge University Engineering Department.
Once one [registers](http://htk.eng.cam.ac.uk/register.shtml),
[downloads the code](http://htk.eng.cam.ac.uk/download.shtml), and unpacks it,
the standard recipe for compiling the source on Linux is
{% highlight bash %}
./configure
make all
{% endhighlight %}
However, on a 64-bit architecture this will fail because of a bug on
line 5507 of `configure` we see that the generated Makefiles will set the
call to the C compiler to have the `-m32` flag (line 3):
{% highlight bash linenos%}
case "$host" in
		*x86_64*linux*)
				CFLAGS="-m32 -ansi -D_SVID_SOURCE -DOSS_AUDIO -D'ARCH=\"$host_cpu\"' $CFLAGS"
				LDFLAGS="-L/usr/X11R6/lib $LDFLAGS"
				ARCH=linux
				trad_bin_dir=linux
		;;
{% endhighlight %}
The `-m32` flag is meant for an
 i386 system since it sets "int", "long", 
and pointer types to 32 bits.  For x86-64 architectures 
we want the `-m64` flag so we need to change Line 3 above to
{% highlight bash %}
				CFLAGS="-m64 -ansi -D_SVID_SOURCE -DOSS_AUDIO -D'ARCH=\"$host_cpu\"' $CFLAGS"
{% endhighlight %}
After making this change I was able to compile HTK without
any further problems.

This note is for HTK version `3.4.1` and hopefully future versions
will include this bug fix.
