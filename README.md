# FAVICSS

> F fmpeg
> A udio
> V ideo
> I mage
> C ompression
> S hell
> S cripts

## Introduction

FAVICSS was made because I needed to compress 5 GB of assets into 2 GB, and I
wanted to automate the process of shrinking thousands of files with FFMPEG via 
shell scripts, low and behold I spend an entire weekend on it.

Because this could be useful to other people I decided to make these shell
scripts public. Use them if you need to.

## How It Works

What FAVICSS does is use FFMPEG to turn images, audio, and video into a
compressed format and then replace the original file keeping the full file name.
Yes a webp file can and will be labeled with .png, however this isn't an issue
as file extensions only tell the OS what program processes the file, and then
that program will look at the unique file header to know what the file actually
is. This allows us to compress everything into a webp, and keep original names
to avoid breaking things that use the files by their name.

## Dependencies

- [ffmpeg](https://ffmpeg.org/download.html)
- [fontconv](https://github.com/marmooo/fontconv)

## Contents

Currently there are 3 shell scripts:
- cprdir    -   Compress directory
- cprdirdir -   Compress directory's directories
- cmpdir    -   Compare directory

cprdir uses FFMPEG to compress directories according to it's flags. cprdirdir
calls cprdir on all directories within the current directory (eaiser
parallelism). And cmpdir compares two directories moving all smaller files from
the second directory into the first directory. cmpdir was made because I tested
a lot of different configs, it's mostly useless. Files are recognized through
mime-type.

## How To Use

### cprdir
`cprdir [flags] [Quality level]`

Flags:

- h: Help
- y: Ignores prompts and automatically says yes
- b: Compress in a different directory (copies the entire directory over)
- v: Include videos (videos take a while)
- f: Include fonts (requires fontconv)
- q: Manual quality # for images
  - Quality #: Used by ffmpeg for images
- c: Tries multiple ffmpeg setups to see which one compresses the file best
(only majorly helps with images)

Ex:

```
cprdir -yc
cprdir -cqy 0
```


### cprdirdir
`cprdirdir [flags]`

Flags:

- h: Help
- y: Ignores prompts and automatically says yes
- b: Compress in a different directory (copies the entire directory over)
- n: Prompts you for every single directory (overwrites y in it's cases)
- v: Include videos (videos take a while)
- f: Include fonts (requires fontconv)
- q: Manual quality # for images
  - Quality #: Used by ffmpeg for images
- c: Tries multiple ffmpeg setups to see which one compresses the file best

Ex:

```
cprdirdir -ycn
cprdirdir -cqy 0
```


### cmpdir
`cmpdir <dir1> <dir2>`

dir1 and dir2 share the same file structure. dir2 is the same name as dir1 but
with a leading _ (ex: dir1="bob" dir2="_bob").

Compares all files in dir1 and dir2, if the file in dir2 is smaller than dir1
then we move the file from dir2 into dir1. This is destructive to dir2.

