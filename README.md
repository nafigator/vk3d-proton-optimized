# VKD3D-Proton (x86-64-v4 optimized)

Custom builds of [VKD3D-Proton](https://github.com/HansKristian-Work/vkd3d-proton) with `-march=x86-64-v4 -O3` and related flags for Linux on AMD Zen 4 CPUs.

## Requirements

- **Architecture:** amd64
- **CPU:** with **AVX-512** support (x86-64-v4)
   - AMD Zen 4 (Ryzen 7000/8000/9000)
- Wine 8.0+ or Proton 8.0+

Packages will crash with `Illegal instruction` on CPUs without AVX-512.

## Installation

1. Download the archive from the [Releases](../../releases) page.

2. Extract it:

   ```bash
   tar --zstd -xf vkd3d-proton-*.tar.zst
   cd vkd3d-proton-*/
   ```

3. Install into your Wine prefix:

   ```bash
   WINEPREFIX=/path/to/your/prefix ./setup_vkd3d_proton.sh install
   ```

   Use `--symlink` if you want symlinks instead of copying files (convenient when updating).

## Clear shader caches after installation

**Mandatory.** Old shader caches are incompatible with the new library version and will cause crashes or rendering artifacts. Clear them before launching any game.

### Global caches

```bash
rm -rf ~/.cache/vkd3d-proton/* \
       ~/.cache/dxvk/* \
       ~/.cache/mesa_shader_cache*
```

### Per-game caches

VKD3D-Proton stores its pipeline cache **next to the game's `.exe`**. Remove it manually:

```bash
rm -f /path/to/game/vkd3d-proton.cache \
      /path/to/game/vkd3d-proton.cache.write
```

First launch after clearing the cache will be slower — shaders are recompiled and a fresh cache is created.

## Build Architecture

### x86_64

- Compiler: `Clang 22`
- Build flags: `-march=x86-64-v4, -mtune=znver4, -O3, -fomit-frame-pointer, -falign-functions=32, -falign-loops=32`

## Notes

- Built from upstream tag `v3.0.1`. Source patches only touch compiler flags.
- Debug symbols are not included.
- Not affiliated with the upstream project. Report build-specific issues in this repository's [Issues](../../issues) tracker.

## License

Build scripts and workflows in this repository are licensed under the MIT License.

VKD3D-Proton itself is distributed under the [VKD3D license](https://github.com/HansKristian-Work/vkd3d-proton/blob/master/LICENSE). The compiled binaries in Releases are redistributions of VKD3D-Proton under its original license.
