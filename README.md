## McMap by Zahl

This is a utility to create maps of Minecraft worlds from an isometric perspective. It can be used to create timelapses with a script. See [here](http://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-tools/1260548-mcmap-isometric-renders-ssp-smp-minecraft-1-3-1]) for more information.

At the time of writing, it's compatible with Minecraft versions up to 1.9.X.

To build, you need `make`, `g++`, and `libpng`. On Ubuntu 14.04, installing dependencies might look something like:

`sudo apt-get install -y build-essential libpng12-dev`

To build, run `make` in the base directory.

For MSVC++, get "zlib compiled DLL" from http://www.zlib.net/ and read USAGE.txt
gcc from MinGW works with the static library of zlib; however, trying
static linking in MSVC++ gave me weird results and crashes (v2008) 
on linux, static linking works too, of course, but shouldn't be needed


For help, run `mcmap` without arguments or with the `-h` flag.

### Example usage

`./mcmap ~/.minecraft/saves/World1` would render your entire singleplayer world in slot 1.

`./mcmap -night -from -10 -10 -to 10 10 ~/.minecraft/saves/World1` would render the same world but at night, and only from chunk (-10 -10) to chunk (10 10)

If you're using Windows, your singleplayer world in slot 1 is probably in `%%APPDATA%%\\.minecraft\\saves\\World1` instead.

### Options
Format: `./mcmap [-from X Z -to X Z] [-night] [-cave] [-noise VAL] [...] WORLDPATH`

- `-from X Z`     sets the coordinate of the chunk to start rendering at
- `to X Z`       sets the coordinate of the chunk to end rendering at
> Note: Currently you need both -from and -to to define
> bounds, otherwise the entire world will be rendered.
- `-cave`         renders a map of all caves that have been explored by players
- `-blendcave`    overlay caves over normal map; doesn't work with incremental
                rendering (some parts will be hidden)
- `-night`        renders the world at night using blocklight (torches)
- `-skylight`     use skylight when rendering map (shadows below trees etc.)
> Note: using this with -night makes a difference
- `-noise VAL`    adds some noise to certain blocks, reasonable values are 0-20
- `-height VAL`   maximum height at which blocks will be rendered
- `-min/max VAL`  minimum/maximum Y index (height) of blocks to render
- `-file NAME`    sets the output filename to `NAME`; default is `output.png`
- `-mem VAL`      sets the amount of memory (in MiB) used for rendering. `mcmap` will use incremental rendering or disk caching to stick to this limit. Default is 1800.
- `-colors NAME`  loads user defined colors from file `NAME`
- `-dumpcolors`   creates a file which contains the default colors being used
                for rendering. Can be used to modify them and then use `-colors`
- `-north -east -south -west`
                controls which direction will point to the *top left* corner
                it only makes sense to pass one of them; East is default
- `-blendall`     always use blending mode for blocks
- `-hell`         render the hell/nether dimension of the given world
- `-end`          render the end dimension of the given world
- `-serverhell`   force cropping of blocks at the top (use for nether servers)
- `-nowater`  	render map with water being clear (as if it were air)
- `-texture NAME` extract colors from png file `NAME`; eg. `terrain.png`
- `-biomes`       apply biome colors to grass/leaves; requires that you run
                Donkey Kong's biome extractor first on your world
- `-biomecolors PATH`  load `grasscolor.png` and `foliagecolor.png` from `PATH`
- `-info NAME`    Write information about map to file `NAME` You can choose the
                format by using file extensions `.xml`, `.json` or `.txt` (default)
- `-split PATH`   create tiled output (128x128 to 4096x4096) in given `PATH`
- `-marker c x z` place marker at x z with color c (r g b w)

`WORLDPATH` is the path of the desired Minecraft world.
