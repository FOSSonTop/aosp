---
order: 5
---

# Setting up ota updates

First we need to tell the updater where to look for the updater info. We can do that using an [overlay in the device tree](https://github.com/SirRGB/android_device_oneplus_msm8998-common/commit/fe390cf65b910c9833b7cc4fbbd1359ff5b5fb9f). Be sure to use the exact url, that only provides text and not an entire webpage. {device} resolves to the devices codename, i.e. cheeseburger for the OnePlus 5.
On lineage you can simply pick [this commit](https://github.com/amyROM/vendor_amy/commit/eb0ff17ced46d969bdc5a4e8599433d76f93f8f5) and [use sha256sum](https://github.com/amyROM/vendor_amy/commit/1938b05bf3d0d8fc6e1bfb16a6be033b6a624502#diff-91c830a3d50e97454dfe32defa90f9635bd926ad3130f2a1cdf06f52e980026fR9) instead of md5sum.  
After the build you get a json file named similarly to the package name, which you can upload to the url you specified in the updater overlay after you uploaded the package and adapted the url within the ota info to reflect that.
