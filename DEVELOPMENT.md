# Development process

How to to compile and create font packages.  [FontCustom](https://github.com/FontCustom/fontcustom)
essentially does all the heavy lifting.  This build process is heavily dependent on the underlying
version of FontCustom installed.

## What you need

* Ruby 1.9.x or greater
* FontCustom 1.3.8 or greater

## Install

FontCustom does most of the heavy-lifting work.

### OSX

    brew update
    brew doctor

Fix any errors shown by `brew doctor`.

    brew install python
    brew install gettext libpng jpeg libtiff giflib cairo pango
    brew install libspiro czmq fontconfig automake libtool pkg-config glib

    brew install fontforge --with-cairo --with-czmq --with-gif --with-x --with-libspiro --with-pango --enable-pyextension --debug --with-python

    brew linkapps

    brew install eot-utils
    brew install ttfautohint
    gem install fontcustom

### Linux

On Debian and Ubuntu, you may need to install the ruby dev packages first
if not already on your machine:

    sudo apt-get install ruby ruby-dev

Then install the fontcustom dependencies:

    sudo apt-get install fontforge ttfautohint
    wget http://people.mozilla.com/~jkew/woff/woff-code-latest.zip
    unzip woff-code-latest.zip -d sfnt2woff
    cd sfnt2woff
    make
    sudo mv sfnt2woff /usr/local/bin/
    cd ..
    rm -Rf sfnt2woff woff-code-latest.zip
    sudo gem install fontcustom

If `ttfautohint` is not available in your distribution,
Font Custom will still work but your generated font will
not be properly hinted.

## Preparing the artwork

The simplest way to get the sizing right for the artwork is to open one of the
existing SVG files into your editor (Adobe Illustrator, InkScape), save a
copy of it with the new name, delete the current artwork, then paste yours in,
scale to the artboard size.

### Tips for editing in Inkscape

Open the file in Inkscape.  You'll want to start removing / editing paths to
simplify the source for working in a single color environment.  Here are 
tips to keep in mind (specifically for Inkscape, but could work in other apps).

- Remove any background layers/paths

- Paths should be fill only, no stroke, and a single color (black is best)

- Edit > XML Editor is your friend.  Some svg files will have too many layers
you won't see unless you get into the xml itself.  You may want to delete
many of them.

- File > Document properties > Resize page to content > 
Resize page to drawing or selection will usually fix the final viewport and
the size of your generated icon.
    
- Save as "Plain SVG"

## Building the font

Execute this:

    src/create-package.sh

This will re-generate the font directory in the root of the repo.  Browse a 
preview of your new font by opening this in your browser

    target/preview.html

## Build issues

The most likely issue is that your version of FontCustom is different than what
the scripts were developed with.  Check your version of FontCustom

    gem list fontcustom

If not v1.3.8 then you can either try that specific version OR figure out what
changed with FontCustom and submit a pull request to work with the newer version.

## Known issues

Due to a limitation with FontCustom, the glyphs are allocated specific Unicode
addresses **alphabetically***.

This means that if you add a new icon, the Unicode addresses
of all the icons that occur alphabetically after it will be different.

Currently, there does **not** appear to be a way to assign static addresses
to glyphs with FontCustom, but there has been some talk in their
Github issues.

This could be a real problem if you are using these icons in a non-CSS
environment.

## Notes

`src/create-package.sh` will compile the font into `font-mfizz`

There is a `preview.html` file in there which will display how the icons render.

You may need to tweak your source `.svg` files quite a bit to get fontcustom to correctly produce a font.

For example, you may need to tweak the xml of the `.svg` to limit how its painted, named, etc.

Every other `.svg` file in this package is an example of ones that work.

If you are having issues compiling a font, you may need to tweak the
SVG to get rid of any advanced formatting, etc. to get it to compile.
