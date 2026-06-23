# Fluida.lv2

Fluidsynth as LV2 plugin 

![Fluida](https://raw.githubusercontent.com/brummer10/Fluida.lv2/master/Fluida.png)

Channel Matrix Editor

![Fluida](https://raw.githubusercontent.com/brummer10/Fluida.lv2/master/Fluida_Channel_matrix.png)

## Dependencies

- libcairo2-dev
- libx11-dev
- lv2-dev
- libfluidsynth-dev


## Build
- git clone https://github.com/brummer10/Fluida.lv2.git
- cd Fluida.lv2
- git submodule init
- git submodule update
- make
- make install # will install into ~/.lv2 ... AND/OR....
- sudo make install # will install into /usr/lib/lv2

## Windows build with MSYS2 UCRT64

Open an MSYS2 UCRT64 shell and install the build dependencies:

```sh
pacman -S --needed \
  mingw-w64-ucrt-x86_64-fluidsynth \
  mingw-w64-ucrt-x86_64-gcc \
  mingw-w64-ucrt-x86_64-lv2 \
  mingw-w64-ucrt-x86_64-make \
  mingw-w64-ucrt-x86_64-pkgconf
```

If you are building the full GUI target instead of the mod-host/nogui target,
install Cairo as well:

```sh
pacman -S --needed mingw-w64-ucrt-x86_64-cairo
```

Clone the repository, initialize the submodules, and build the Windows LV2
bundle for mod-host:

```sh
git submodule update --init --recursive
make TARGET=Windows LIB_EXT=dll STRIP=strip mod
```

The `STRIP=strip` override is needed in UCRT64 because the default
`x86_64-w64-mingw32-strip` executable may not be present.

The generated bundle is written to `bin/Fluida.lv2`. To install it for the
current user:

```sh
mkdir -p ~/.lv2
rm -rf ~/.lv2/Fluida.lv2
cp -R bin/Fluida.lv2 ~/.lv2/
```

Before launching mod-host, make sure `~/.lv2` is in the LV2 search path:

```sh
export LV2_PATH="$HOME/.lv2${LV2_PATH:+:$LV2_PATH}"
```

For a Windows build, the bundle metadata should point at the DLL:

```sh
grep -R "lv2:binary" bin/Fluida.lv2/*.ttl
```

Expected output includes `Fluida.dll`, not `Fluida.so`.

## Binary
Checkout the latest release for binaries compatible with Linux x86_64 or Windows (64bit)

## Modifying your XDG `user-dirs.dirs` file

It is likely that you don't store your soundfonts in the root of your home directory. If you are using Fluida.lv2 frequently then navigating to your soundfont directory every time you create a new instance of Fluida.lv2 can become tedious in which case you should add your soundfonts dir(s) your `~/.config/user-dirs.dirs` file by adding a line such as:

```
XDG_SF2_DIR="/usr/share/sounds/sf2"
```

Replace `SF2` with a label of your choice for paths you add. You will then be able to access the defined path(s) with a single click from the `Places` panel of Fluida's File Selector window.
