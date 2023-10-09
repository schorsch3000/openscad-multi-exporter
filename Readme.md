# Openscad Multi Exporter
This tool has 3 main features, it works as a command line tool on a single `.scad` file or a directory containing `.scad` files and does the following things to them in the given order:

## Formatting
It reformats the file using [openscad-format](https://github.com/Maxattax97/openscad-format).

## Exporting multiple variants.
This is the main feature, you can export multiple files from one file to STL's.
Did you ever hat a single `.scad` file with a bunch of objects belonging to a single project and you wanted to export them all to STL's? Well this tool does that for you.
It does this by looking for a special comment in the file, it looks like this:
```
/***exporter
// generich export options like:
$fn=64;
***box
!box();
***lid
!lid();
***handle
!handle();
**/
```
Imagine a file `Lunchbox.scad` that has 3 modules that produces the 3 parts of the box, the box itself, the lid and the handle, and the above code, which ist just a comment to openscad.
The comment above tells the tool to export 4 files, containing the whole file itself as a copy, but the comment above is altered.


The first file is named `Lunchbox-export.scad`.
Instead of the comment above it contains the following:
```
$fn=64;
```


The Second file is named `Lunchbox-export-box.scad`.
Instead of the comment above it contains the following:
```
$fn=64;
!box();
```

The Third file is named `Lunchbox-export-lid.scad`.
Instead of the comment above it contains the following:
```
$fn=64;
!lid();
```

The Fourth file is named `Lunchbox-export-handle.scad`.
Instead of the comment above it contains the following:
```
$fn=64;
!handle();
```

To clarify:
within the comment, every line starting with `***` starts a variant-file. text before the first variant is global and will be added to every variant-file and the non-variant export.

Variants don't need do follow the `!modulename();` pattern. you might just want to set or overwrite variables.

You don't need to use variants at all, you can just ode the global export to have debug things turned off.

## Exporting to STL
This is the last step, it exports all `.scad` files in the given directory to `.stl` files.

naming convention is: its the same name, but with `.stl` instead of `.scad`.

This also works with the exported files from the previous step.

While exporting to a `.stl` the variable `$export` is set to 1. This can be used to overwrite things like `$fn`.

# usage
just run `openscad-converter` to have it do it's thing on the current directory.
You might want to specify a directory either by giving a parameter or by setting an ENV-var `OPENSCAD_CONVERTER_DIR`

# dependencies
- [openscad-format](https://github.com/Maxattax97/openscad-format)
- [openscad](https://www.openscad.org/)
- a php-cli interpreter
- a bash shell
- some basic shell tools that you propperly already have installed: `find` `realpath` `xargs` 
