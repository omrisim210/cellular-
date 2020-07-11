# cellular++

This will be a cellular automata library in C++, right now it's a work in progress.

## Getting & building

### Requirements:
 * A C++ compiler that supports C++2a (although C++17 might also work, YMMV)
 * Meson
 * Ninja
 * Catch2 (optional, can use meson wrap if not installed on system)
 * SDL2 (optional, for the upcoming graphical frontend for the binary)

### Compiling natively on *nix:

```
$ git clone https://github.com/omrisim210/cellularpp.git
$ cd cellularpp
$ meson build
$ meson compile -C build
$ meson wrap install catch2
$ meson test -C build
```

Then look inside `build/bin`.

### Cross compiling for Windows with Docker:

If running for the first time:
```
$ mkdir docker
$ sudo docker build -t meson-mingw-build-env -f Dockerfile ./docker
$ sudo docker run --name=mingw -it -v $PWD:/root meson-mingw-build-env
```

Else:

```
$ sudo docker start --attach --interactive mingw
```

Then, in the Docker shell, generate the build directory:

```
# $MESON_WRAPPER build-mingw && cd build-mingw
```

Optionally, change any options you like. Some to consider are:

```
# meson configure -Dstrip=<true|false>
# meson configure -Dbuildtype=<plain|debug|debugoptimized|release|minsize|custom>
# meson configure -Db_ndebug=<true|false|if-release>
```

Et cetera, [read the Meson docs](https://mesonbuild.com/Builtin-options.html) for more info about them.

Then compile:
```
# meson compile
# mingw_make_pkg.sh x86_64 cellular++ cellular++ bin/cellular++.exe
```

This should result in an archive `build-mingw/cellular++.7z` that contains something along the lines of this:

```
$ als build-mingw/cellular++
...some output skipped for brevity
   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2020-07-03 10:11:40 D....            0            0  cellular++
2020-07-03 10:11:40 ....A      1275918       823008  cellular++/SDL2.dll
2020-07-03 10:11:39 ....A       173622               cellular++/cellular++.exe
2020-07-03 10:11:40 ....A        82446               cellular++/libgcc_s_seh-1.dll
2020-07-03 10:11:40 ....A      1711630               cellular++/libstdc++-6.dll
2020-07-03 10:11:40 ....A        56832               cellular++/libwinpthread-1.dll
------------------- ----- ------------ ------------  ------------------------
2020-07-03 10:11:40            3300448       823008  5 files, 1 folders
```

### Hacking
The library is header-only and located in `include`, while the binary is in `bin`.
Coding style isn't to my liking yet, I think it's some default of `clang-format`.
I'll change it later, for now just try to stick to the project's style.


## TODO:

- [x] Implement automata in their most basic, buggy and unpolished form.
- [x] Fix stepping (look in `include/cellular.hpp` for bug descriptions).
- [x] Fix weird extra-cell bug (see previous item). (This was caused because of an overflow)
- [ ] Fix testing (the test just doesn't seem to work).
- [ ] Implement meaningful tests.
- [ ] Implement more automata like a powder toy type thing, WireWorld and some roguelike cave-generation algorithms.
- [ ] Add tutorial/docs/usage instructions, possibly using some automated tool (might not be needed since the library core is so small). For now use `bin/main.cpp` as a reference.
- [ ] Add an actual project binary with pluggable automata, possibly using multiple executables in a certain directory (like how Rust's `cargo` tool handles pluggable subcommands)
- [ ] Automate building with docker
- [ ] Automate testing with docker. We might need to use Wine for that ~~but I use WSL so it shouldn't run on my system~~.
- [ ] Setup a `.dockerignore` file. I think it's used to not include files we don't need from the base image?
- [ ] Implement generic graphical frontend for automata (working on this)
- [ ] Maybe integrate Lua for writing automata?
