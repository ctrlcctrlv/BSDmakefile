# BSDmakefile v1.0

Makes testing Makefile is (Free|Open)BSDmake compatible easier on GNU/Linux

## How to use this in your own projects

* Copy `GNUmakefile` and the directory `mk` to your project directory.
* Copy `Makefile.EXAMPLE` to your project directory as `Makefile`.

## Benefits

`GNUmakefile` is a file that is only understood by GNU Make. So, when `make` is run, if you are on GNU/Linux, (or using `gmake` on another system,) it will attempt to run all common commands through Busybox to make sure your `Makefile` works with stripped down non-GNU coreutils.

However, when run on a system with only `bsdmake` (`bmake` on GNU/Linux), the file is totally ignored and `busybox` is not used.

## Writing the `Makefile`

All of your recipes must be written as:

```make
myrecipe:
	$(DOBEFORE) && (\
		# …
	)

# …

.include <mk/before.mk>
```

Furthermore, it is highly recommended to enforce:

```make
SHELL=/bin/sh
```

At the top of the `Makefile`. This ensures compatibility with POSIX shells,
such as those in use by default on *BSD.

## Accessing BSD-compatible coreutils

By default `sed` and `awk` will use generic versions, with their paths at `$SED` and `$AWK`, either through Busybox or through the system coreutils as appropriate. You can add any program `busybox` supports by editing the `all` recipe in `GNUmakefile`, and adding more `$(MAKE) test_busybox_has` lines. E.g., to add `cat` as `$CAT`, add the line:

```make
all:
	# …
	$(eval export $(shell $(MAKE) WHAT=cat test_busybox_has))
	# …
```

## Full project example

I got this idea while working on [yeslogic/glyph-names№3](https://github.com/yeslogic/glyph-names/pull/3) and added `BSDmakefile` to that project in [yeslogic/glyph-names№2](https://github.com/yeslogic/glyph-names/pull/2).

## LICENSE

```plain
###############################################################################
#-       BSDmakefile © 2022 Fredrick R. Brennan <copypaste@kittens.ph>       -#
###############################################################################
#- Permission  is  hereby granted, free of charge, to any person obtaining  a #
#- copy of this software and associated documentation files (the "Software"), #
#- to  deal in the Software without restriction, including without limitation #
#- the  rights to use, copy, modify, merge, publish, distribute,  sublicense, #
#- and/or  sell  copies  of the Software, and to permit persons to  whom  the #
#- Software is furnished to do so, subject to the following conditions:       #
#-                                                                            #
#- The above copyright notice and this permission notice shall be included in #
#- all copies or substantial portions of the Software.                        #
#-                                                                            #
#- THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR #
#- IMPLIED,  INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF  MERCHANTABILITY, #
#- FITNESS  FOR  A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT  SHALL #
#- THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER #
#- LIABILITY,  WHETHER  IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,  ARISING #
#- FROM,  OUT  OF  OR  IN CONNECTION WITH THE SOFTWARE OR THE  USE  OR  OTHER #
#- DEALINGS IN THE SOFTWARE.                                                  #
###############################################################################
```
