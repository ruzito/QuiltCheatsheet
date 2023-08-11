# Quilt Cheatsheet
This is my cheatsheet for quilt comand (patch manager)

## Creating patches

Interface is bit simillar to `git`

- `quilt init` (in the directory with files you want to patch, simillarly to `git init`)
- `quilt new patch-name` (also does `git push` of that new patch) (like `git checkout -b branch-name`)
- `quilt add path-to-file-to-track-changes-of`
- edit the file
- `quilt refresh` (this edits the patch on top to include the changes) (like `git commit --amend`)
- `quilt header <some options, which determine the header format>` (opens `$EDITOR` to edit the _"commit message"_ and metadata) (debian recommends `-e --dep3` options)
- `quilt pop` (unapply most recent patch) (like `git checkout HEAD~1`)
- `quilt pop -a` (unapply all patches aka restore original files)

## Directory structure

Commands above should create some directories/files

- `.pc` - internal information of `quilt` so that it isn't lost during pushing and popping patches (not needed to be shipped)
- `patches` - directory with all the diffs ready to be applied
- `patches/series` - plain old txt list of patches to be applied in order by `quilt`

these filenames can be changed using env variables or options.

## Using patches

When you get sent / download some piece of work that includes patches, there has to be some `patches` directory shipped with patches and a `series` file (may be named differently but probably shouldn't)

In such directory you just `quilt push -a`

## Configuration

See `man quilt` looooooower down

## Patch format

Seems to be very forgiving with the format

I haven't encountered something I cannot put into the header yet as long as the actual patch starts with
```
--- a/filepath
+++ b/filepath
@@ <position>, <position> @@
```
and continues as classic git diff

so for example
```
dcfvgbhnjujhgfdgxfc vfghb

--- a/file.txt
+++ b/file.txt
@@ -1 +1 @@
-Hello World
+Hello Earth
```

or

```
Fixes DEB_BUILD_OPTIONS=nodoc fail on illformed debian/rules

===============================================================

--- a/debian/rules
+++ b/debian/rules
@@ -54,12 +54,13 @@
 build-indep: build-indep-stamp
 # build-indep should not need to depend on build-arch, but currently it does at least to populate build/doc
 # Add the dependency until we get a chance to make it work properly
 build-indep-stamp:  build-arch
+	true
 ifeq (,$(findstring nodoc,$(DEB_BUILD_OPTIONS)))
	cd build/doc && make PYTHON=python3 substhtml substpdf
	ln -sf /usr/share/javascript/jquery/jquery.js build/doc/html_subst/_static/jquery.js
	ln -sf /usr/share/javascript/underscore/underscore.js build/doc/html_subst/_static/underscore.js
	ln -sf /usr/share/javascript/sphinxdoc/1.0/doctools.js build/doc/html_subst/_static/doctools.js
	ln -sf /usr/share/javascript/sphinxdoc/1.0/searchtools.js build/doc/html_subst/_static/searchtools.js
 	touch build-indep-stamp
 endif

```

## See also

- https://wiki.debian.org/UsingQuilt
- `man quilt`
