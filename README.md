# GKI Custom

Enable LXC, Docker support for gki kernel, everything comes from [Common-Android-Kernel-Tree](https://github.com/lateautumn233/Common-Android-Kernel-Tree), Based on [android12-5.10](https://source.android.com/docs/core/architecture/kernel/gki-android12-5_10-release-builds)

# Build

Sync the kernel source code

Build reference [KernelSU](https://kernelsu.org/guide/how-to-build.html)
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
cp ./GKI-Custom/gki_defconfig ./android-kernel/common/arch/arm64/configs/gki_defconfig
cd android-kernel/common
git apply ../../GKI-Custom/patchs/*.patch
```

Build
```bash
LTO=thin BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh
```

Create the img file

Compression format: raw

Download boot from [this](https://source.android.com/docs/core/architecture/kernel/gki-android12-5_10-release-builds)
```bash
./tools/mkbootimg/unpack_bootimg.py --boot_img ../boot-5.10-xxxx.img
./tools/mkbootimg/mkbootimg.py --header_version 4 --kernel out/android12-5.10/dist/Image --ramdisk out/ramdisk --os_version [OS_VERSION] --os_patch_level [OS_PATCH_LEVEL] -o ../android12-5.10.xxx_[OS_PATCH_LEVEL]-boot.img
```

# Credits

[lateautumn233](https://github.com/lateautumn233)
