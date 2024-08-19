# GKI Custom

Enable LXC, Docker support for GKI Kernel, everything comes from [Common-Android-Kernel-Tree](https://github.com/lateautumn233/Common-Android-Kernel-Tree)

# Build

If you don't want to build it yourself, you can jump to the [actions](https://github.com/TapetalArray/GKI-Custom/actions) to download or fork repo run a new workflow

Sync the kernel source code, build reference [KernelSU](https://kernelsu.org/guide/how-to-build.html)

```bash
mkdir android-kernel; cd android-kernel
repo init --depth 1 -u https://android.googlesource.com/kernel/manifest -b [BRANCH]
repo sync
```

Clone this repo

```bash
git clone https://github.com/TapetalArray/GKI-Custom
```

Apply patches and configuration files

```bash
cp ./GKI-Custom/config/gki_defconfig-android12-5.10 ./android-kernel/common/arch/arm64/configs/gki_defconfig
cd android-kernel/common
git apply ../../GKI-Custom/patchs/*.patch
```

Build

```bash
LTO=thin BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh
```

Create the img file, Download boot from [gki-release-builds](https://source.android.com/docs/core/architecture/kernel/gki-release-builds).

```bash
./tools/mkbootimg/unpack_bootimg.py --boot_img path/to/img
./tools/mkbootimg/mkbootimg.py --header_version 4 --kernel path/to/Image --ramdisk path/to/ramdisk --os_version [OS_VERSION] --os_patch_level [OS_PATCH_LEVEL] -o path/to/img
```

# Credits

[lateautumn233](https://github.com/lateautumn233)
