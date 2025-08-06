### How to build:

1. Establish [Android build environment](https://source.android.com/setup/initializing) and install [repo](https://source.android.com/docs/setup/develop#installing-repo).

2. Initialize repo:

```
repo init -u https://android.googlesource.com/kernel/manifest -b common-android16-6.12-lts
curl -o .repo/local_manifests/manifest_rk_opi.xml -L https://raw.githubusercontent.com/dvab-sarma/android_kernel_manifest/android-16.0/manifest_rk_opi.xml --create-dirs
```
3. Sync source code:

```
repo sync
```
4. Compile:

Orange Pi 5:
```
tools/bazel build --config=fast --config=stamp //common:opi5
```

Orange Pi 5 pro:
```
tools/bazel build --config=fast --config=stamp //common:opi5_pro
```

Orange Pi 3B:
```
tools/bazel build --config=fast --config=stamp //common:opi3b
```

Compiled kernel Image, dtb can be found in -

for opi 5 -
`bazel-bin/common/opi5/arch/arm64/boot` 

for opi 5 pro - 
`bazel-bin/common/opi5_pro/arch/arm64/boot`

for opi 3b - 
`bazel-bin/common/opi3b/arch/arm64/boot`

directory.

Replace existing files in `device/opi/opi5_pro-kernel` or for opi 3b `device/opi/opi3b-kernel` directory of the Android source tree to include them in Android 16 build. You can also replace existing files in the boot partition of the Orange Pi 5 pro or Orange Pi 3b Android 16 image .

If you want autoboot for Orange Pi 5 only build (the easy way: which is not generating bootscr.cmd from ground up), Just rename the dtb of Orange Pi 5 dtb(`rk3588s-orangepi-5.dtb`) to Orange Pi 5 Pro dtb name (`rk3588s-orangepi-5-pro-1.dtb`) and copy it into the boot patition of the image or into the   `device/opi/opi5_pro-kernel`.

5. rebuild steps - 
```
1. cd common
2. make mrproper
3. cd ..
4. rm -rf out/ bazel-out/ bazel-bin/ bazel-* .bazelrc
5. tools/bazel clean --expunge
```

and follow 4th step, to compile the kernel image and dtb without any errors.
