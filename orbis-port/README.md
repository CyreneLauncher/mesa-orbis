# Orbis Mesa port

This checkout retains the pinned upstream Mesa source and the complete imported
launcher patch history. `patches/*.patch` is the authoritative ordered port series.
The source checkout is a clean base; build_mesa.py applies the series to a separate
revision-pinned build checkout and reverses it after building. Do not apply it twice.

This repository builds Mesa without the launcher:

```sh
python3 orbis-port/tools/prepare_sources.py --output build/sources
python3 orbis-port/tools/bootstrap_deps.py --output build/deps/clean
OO_PS4_TOOLCHAIN=/path/to/PS4Toolchain build/sources/orbis-compat/build.sh
python3 build_mesa.py --sources build/sources --sdk /path/to/PS4Toolchain --output build/target
```

`orbis-port/build.lock.json` pins the Mesa base revision (a commit of this repository), the
orbis-compat and orbis-porting-kit revisions, the host tools (Meson and its Python wheels,
Flex, Bison), zlib and the expected compat archive, and the SDK files Mesa was verified with.
`prepare_sources.py` checks those revisions out using the sibling checkouts as seeds.
Bootstrap needs Python 3.12 on Ubuntu 24.04 for the pinned wheel tags and Debian packages.

`ORBIS_BUILD_ROOT` (default `build/`) holds `deps/`, `sources/` and the outputs; every tool
refuses to write outside it. The launcher sets it to its `graphics-probe/build`. lwjgl-orbis's
platform PRX links the archives this build produces. Port changes belong here; update the
launcher's repository pin after committing them.

Release: `.github/workflows/orbis-release.yml` builds this chain on GitHub-hosted Ubuntu and, with
`publish` checked, releases `mesa-<commit>/mesa-orbis-sdk.tar.xz` with the contents
`release.json` lists: the driver archives, build manifest, Mesa headers and licence, and the built
orbis-compat and porting-kit trees. lwjgl-orbis links its platform PRX against that release.
`dependencies.lock.json` pins the orbis-runtime-support revision the workflow uses.
