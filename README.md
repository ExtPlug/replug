# non-lethal plug.dj reverse engineering

A messy script that spews as-sexy-as-computerly-possible versions of plug.dj
modules into a directory.

Minimal customizability, not enough automation. Awesome. :shipit:

Slap ReAnna if it doesn't work. (Metaphorically.)

## installing

For a globally installed CLI:

1. `npm i -g replug`

Or for source:

1. `git clone https://github.com/ExtPlug/replug.git`
1. `cd replug`
1. `npm install`

## actually doing things

    Options:

      -h, --help            output usage information
      -V, --version         output the version number
      -m, --mapping [file]  File containing the mapping JSON (optional, it's auto-generated if no file is given)
      -o, --out [dir]       Output directory [out/]
      -c, --copy            Copy deobfuscated files instead of symlinking (nice for Windows)
      --save-source         Copy the source javascript to the output directory
      --save-mapping        Copy the mapping file to the output directory

### easy way

**Plain:**

Dumps output in `out/`, and doesn't store the original JavaScript source or the
mapping file.

```
replug
```

**Full:**

Outputs in `output-directory/`, and stores the original JavaScript source in
`source.js`, and the mapping file as `mapping.json`.

```
replug --out output-directory \
  --save-source --save-mapping
```

The easy way runs [`plug-modules`](https://github.com/ExtPlug/plug-modules)
automatically to remap plug.dj's obfuscated module names to readable module
names. It does that by essentially fully loading and booting plug.dj, much like
the below old-fashioned way but headless.

Remapped module names are symlinked to the source files. If you're on Windows
or don't like symlinks, pass the `--copy` option which will output the full
source files in both the original and the remapped paths.

### harder, partly browser-based, but still pretty solid way:

1. Log in to plug.dj (lol)
1. Run the `getMapping.js` file in your browser console
1. A `mapping.json` file will be downloaded. This file contains the mapping from
   plug.dj's obfuscated module names, to `plug-modules`'s deobfuscated module
   names. `replug` will use it to determine the file names for modules.
1. Run the index.js file in this repo: `node index.js --mapping <file>`, eg.
   `node index.js --mapping mapping.json`
1. Wait
1. Check `out/app` which will now contain a ton of files. Actual files are in a
directory with a semi-random name, which is the actual plug.dj module name.
Another directory `$OUT_DIR/plug/` contains symlinks with nicer names where
possible.

Remember to delete the `out/app` directory before every rerun.
