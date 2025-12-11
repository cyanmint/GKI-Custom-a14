# GKI Custom

Enable LXC, Docker support for GKI Kernel

Also supports integrating KernelSU, SukiSU-Ultra, or KernelSU with SUSFS as root solutions.

## Root Solutions

- **KernelSU**: A kernel-based root solution for Android GKI devices
- **SukiSU-Ultra**: An alternative root solution (mutually exclusive with KernelSU)
- **SUSFS**: An addon root hiding kernel patches for KernelSU (requires KernelSU to be enabled)

**Note**: KernelSU and SukiSU-Ultra are mutually exclusive. SUSFS can only be used with KernelSU, not with SukiSU-Ultra.

## Artifacts

The build process creates both traditional boot images and AnyKernel3 flashable ZIPs:
- Boot images: `boot.img`, `boot-lz4.img`, `boot-gz.img`
- AnyKernel3 ZIP: Can be flashed via custom recovery (TWRP, etc.)

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

[simonpunk (SUSFS)](https://gitlab.com/simonpunk/susfs4ksu)

[WildKernels (GKI_KernelSU_SUSFS)](https://github.com/WildKernels/GKI_KernelSU_SUSFS)
