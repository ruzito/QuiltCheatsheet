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
- `quilt pop` (unapply most recent patch)
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

## See also

- https://wiki.debian.org/UsingQuilt
