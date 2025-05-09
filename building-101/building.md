---
order: 2
---

# Building Time!

1. Run
```bash
. build/envsetup.sh
```
or
```bash
source build/envsetup.sh
```

2. Run `lunch PRODUCT-RELEASE(IF A14 QPR2+)-VARIANT`. This is the device and variant you want to build for. 
(eg `lunch lineage_bacon-userdebug or lunch lineage_oxygen-ap2a-userdebug)

However, it is easier to use breakfast instead, like `breakfast PRODUCT VARIANT` 
(eg. breakfast oxygen userdebug)

- Product: Many roms have their own prefixes. Lineage and most lineage-based forks use lineage_device, LMOdroid uses lmo_device and other AOSP based forks use aosp_device.

- Release: Starting with A14 QPR2, you must include the `release` field. On A14 QPR2, this will look something like `ap1a`. On android `main` branch, this is `trunk_staging`.
   You can find out the release that the source ships by running:
   ```bash
   find $(gettop)/build/release/aconfig/* -maxdepth 0 -type d -name "[a-z][a-z][0-9][a-z]" -printf '%f\n' | tail -n1
   ```

- Variant: Variant is the type of build you want to build.
    - `eng` offers easy debugging, but runs slow on the device and is insecure. This should ideally only be used for tree developement or booting a new android version for the first time on a device.
    - `userdebug` is a hybrid between `eng` and `user`. This offers easier debugging than `user`, like `adb root`(if enabled through settings) and runs faster on the device. This builds slower. Most people ship custom roms as this variant.
    - `user` is the least debuggable variant. Most OEMs ship their stock rom in this variant. Here debuggability is minimal.

3. Run
```bash
m -j$(nproc --all)
```
- **Note #1**: If you get errors regarding memory, try reducing the -j argument. The j (jobs) argument is the number of threads which are used during compilation.

- **Note #2**: If you want a flashable zip instead of raw images, run (for most roms)
```bash
m bacon -j$(nproc --all)
```

Hopefully, android builds without errors. This guide does not cover fixing common errors. Once done, you should hopefully see `build completed`.
