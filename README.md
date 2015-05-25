# Kconfiglib #

A Python library for doing stuff with Kconfig-based configuration systems. Can
extract information, query and set symbol values, and read and write
<i>.config</i> files. Highly compatible with the <i>scripts/kconfig/\*conf</i>
utilities in the kernel, usually invoked via make targets such as
<i>menuconfig</i> and <i>defconfig</i>.

**Update: Mar 13 2015**

Thanks to a patch from [Philip Craig](https://github.com/philipc) that adds support
for the new `allnoconfig_y` option (which sets the user value of certain symbols
to `y` during `make allnoconfig` to improve coverage) and fixes an obscure issue
with `comment`s inside `choice`s (that didn't affect correctness but made outputs
differ) the test suite now passes with Linux v4.0-rc3. Very little seems to have
changed in the C implementation over the past years, which is nice. :)

Despite the warnings, modules *are* supported by the way (or tests would break
horribly). The warnings are related to `option modules`, which specifies what
symbol serves as the `MODULES` symbol. It's always `MODULES` in the kernel, and
older versions of the `Kconfig` files did not set `option modules` on it -- hence the
warnings. With newer versions of the C implementation it's required. I should add
support for `option modules` in a backwards-compatible way.

## Installation ##

Installation instructions for the Linux kernel (in the kernel root):

    $ git clone git://github.com/ulfalizer/Kconfiglib.git  
    $ git am Kconfiglib/makefile.patch

<i>(Note: The directory name Kconfiglib/ is significant.)</i>

In addition to creating a handy interface, the make targets are needed to pick
up environment variables set in the kernel makefiles which are referenced in
the Kconfig files (<i>ARCH</i>, <i>SRCARCH</i>, and <i>KERNELVERSION</i> as of
Linux v4.0-rc3).

Please tell me if the patch does not apply. It should be trivial to apply
manually.

## Documentation ##

The (extensive) documentation is generated by running

    $ pydoc kconfiglib

in the <i>Kconfiglib/</i> directory. For HTML output,
use

    $ pydoc -w kconfiglib

## Examples ##

The <i>examples/</i> directory contains simple example programs that make use
of Kconfiglib. See the documentation for how to run them.

## Test suite ##

The test suite is run with

    $ python Kconfiglib/testsuite.py

It comprises a set of selftests and a set of compatibility tests that compare
configurations generated by Kconfiglib with configurations generated by
<i>scripts/kconfig/conf</i> for a number of cases. You might want to use the
"speedy" option; see testsuite.py.

## Misc. notes ##

Please tell me if you miss some API instead of digging into internals. The
internal data structures and APIs, and dependency stuff in particular, are
unlikely to be exactly what you want as a user (hence why they're internal :).
Patches are welcome too of course. ;)

The test suite failures (should be the only ones) for the following Blackfin
defconfigs on e.g. Linux 3.7.0-rc8 are due to
[a bug in the C implementation](https://lkml.org/lkml/2012/12/5/458):

 * arch/blackfin/configs/CM-BF537U\_defconfig  
 * arch/blackfin/configs/BF548-EZKIT\_defconfig  
 * arch/blackfin/configs/BF527-EZKIT\_defconfig  
 * arch/blackfin/configs/BF527-EZKIT-V2\_defconfig  
 * arch/blackfin/configs/TCM-BF537\_defconfig

## License (ISC) ##

Copyright (c) 2011-2015, Ulf Magnusson <ulfalizer@gmail.com>

Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
