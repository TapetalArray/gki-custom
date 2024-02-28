# GKI LXC

Enable LXC support for gki kernel, everything comes from [Common-Android-Kernel-Tree](https://github.com/lateautumn233/Common-Android-Kernel-Tree), base [android12-5.10-2023-05](https://source.android.com/docs/core/architecture/kernel/gki-android12-5_10-release-builds#may-2023-releases)

# Build

Build reference [KernelSU](https://kernelsu.org/guide/how-to-build.html)
```bash
# Sync the kernel source code
mkdir android-kernel & cd android-kernel
repo init --depth 1 -u https://android.googlesource.com/kernel/manifest -b common-android12-5.10-2023-05
repo sync
cd ..

# Clone this repo
git clone https://github.com/TapetalArray/gki-lxc

# Apply patches and configuration files
cp ./gki-lxc/gki_defconfig ./android-kernel/common/arch/arm64/configs/gki_defconfig
cd android-kernel/common
git apply ../../gki-lxc/*.patch
cd ..

# Build Kernel with KernelSU(option)
curl -LSs "https://raw.githubusercontent.com/tiann/KernelSU/main/kernel/setup.sh" | bash -

# Build
LTO=thin BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh

# Create img file
# raw compression format
wget https://dl.google.com/android/gki/gki-certified-boot-android12-5.10-2023-05_r8.zip -P ..
unzip -q ../gki-certified-boot-android12-5.10-2023-05_r8.zip
./tools/mkbootimg/unpack_bootimg.py --boot_img ../boot-5.10.img
./tools/mkbootimg/mkbootimg.py --header_version 4 --kernel out/android12-5.10/dist/Image --ramdisk out/ramdisk --os_version 12.0.0 --os_patch_level 2023-11 -o ../android12-5.10.168_2023-05-boot-lxc.img
cd ..

# Use AnyKernel3
git clone https://github.com/osm0sis/AnyKernel3.git
cp ./android-kernel/out/android12-5.10/dist/Image ./AnyKernel3
# Edit your anykernel.sh
zip -r -q AnyKernel3-android12-5.10.168_2023-05-boot-lxc.zip ./AnyKernel3
```

# Credits

[lateautumn233](https://github.com/lateautumn233)