# GKI Custom

Enable LXC, Docker support for GKI Kernel

# Build

### Build use action

Fork repo and run new workflow

### Manual Build

Sync sources</br>
Reference KernelSU [how to build](https://kernelsu.org/guide/how-to-build.html)

```bash
mkdir android-kernel; cd android-kernel
repo init --depth 1 -u https://android.googlesource.com/kernel/manifest -b [BRANCH]
repo sync
```

#### Clone repo

```bash
git clone https://github.com/TapetalArray/gki-custom
```

#### Apply patches

```bash
cp ./gki-custom/config/gki_defconfig-android12-5.10 ./android-kernel/common/arch/arm64/configs/gki_defconfig
cd android-kernel/common
git apply ../../gki-custom/patches/*.patch
cd ..
BUILD_CONFIG=common/build.config.gki.aarch64 build/config.sh savedefconfig
```

#### Build

```bash
LTO=thin BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh
```

#### Create img
Download [gki-release-builds](https://source.android.com/docs/core/architecture/kernel/gki-release-builds).

```bash
./tools/mkbootimg/unpack_bootimg.py --boot_img path/to/img
./tools/mkbootimg/mkbootimg.py --header_version 4 --kernel path/to/Image --ramdisk path/to/ramdisk --os_version [OS_VERSION] --os_patch_level [OS_PATCH_LEVEL] -o path/to/img
```

# Credits

[lateautumn233](https://github.com/lateautumn233)
