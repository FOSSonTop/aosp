---
order: 1
---

# Syncing AOSP Source code

## Setup: Installing `repo` and other dependencies

Repo is a tool developed by google to help manage the thousands of git repos used in a project like AOSP.

You must first install it on your computer/server.
```bash
mkdir ~/bin && curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo
```
After that you should check if ~/bin is already in your PATH by doing
```bash
printenv PATH|grep ~/bin
```
if you do not get any output you have to add:
```bash
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
```
to ~/.profile or some other script that runs on shell creation.

There are also some additional dependencies, which you need to install to build android.

Arch Linux:
```bash
sudo pacman -S repo base-devel git-lfs
```

Debian / Ubuntu based distros:
```bash
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-gtk3-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
```

Fedora / Fedora based distros:
```bash
sudo dnf install @development-tools android-tools automake bison bzip2 bzip2-libs ccache curl dpkg-dev flex gcc gcc-c++ git git-lfs glibc-devel.{x86_64,i686} gperf libstdc++.{x86_64,i686} libxcrypt-compat libxml2-devel libxslt lz4-libs lzop make maven ncurses ncurses-compat-libs ncurses-devel.{x86_64,i686} openssl openssl-devel pngcrush python python3-virtualenv python3 python3-mako python-mako python-networkx schedtool squashfs-tools syslinux-devel unzip zip zlib zlib-devel
```

You might need to remove metadata_csum_seed and orhpan_file from /etc/mke2fs.conf if you encounter error mentioning "Invalid filesystem option set" as per [Reddit](https://www.reddit.com/r/LineageOS/comments/18lej4b/if_your_build_is_failing_with_an_error_regarding/)


If you build a lot you should consider enabling ccache to save yourself some build time.
```bash
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
```
You should limit its size to something reasonable with the below command
```bash
ccache -M 50G
```
To further increase the number of cached files at the cost of slight decompression overhead you can enable compression.
```bash
ccache -o compression=true
```

## Initialising and syncing source

1. Find the manifest of your preferred android fork. This guide will be using LineageOS as an example.

2. Make a directory in which you want the android source code to be cloned
    ```bash
    mkdir Lineage
    ```

3. Cd into the directory
    ```bash
    cd Lineage
    ```

4. Run
    ```bash
    repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs
    ```

    Replace the url with the link to your preferred source of android. Replace the branch with the version of android you want to build. (Lineage 21 is A14, 20 is A13)

    - **Note #1**: If you get an error like `repo does not know who you are`, run 
        ```bash
        git config --global user.name "ProAndroidBuilder" # This can be any name, even your real name.
        ```

        ```bash
        git config --global user.email "android@veryrealemail.com" # This should be your email.
        ```
        Be warned, these info will be public if you start committing and pushing things.

    - **Note #2**: You can specify the `--depth=1` argument here to save storage space albeit at the loss of commit history.

5. Run
    ```bash
    repo sync -c --force-sync --optimized-fetch --no-tags --no-clone-bundle --prune -j$(nproc --all) --retry-fetches=25
    ```

In case you get RPC errors, try reducing the -j argument to something like four. I usually run this sync command two or three times, to clone any repos that failed to clone during the previous sync (bad internet problems).

6. Wait for the command(s) to complete.

Excellent! You have now successfully cloned android!

## Cloning device repositories.

In order to build android for a device, you must first clone a few things:

* Device tree
* Vendor tree
* Kernel tree
* Other hardware/ repos

The exact repos to clone varies from device to device. Some devices may use a common tree (which means that you have to clone 2 device and vendor trees), while others (especially samsung trees) have many hardware/ dependencies.

Sometimes, the device maintainer might have been kind enough to share a "local manifest". This basically allows you to clone the trees hassle-free with `repo`.

If you are lucky enough to have a local manifest, just drop it in to 
```
ANDROID_CLONE_DIRECTORY/.repo/local_manifests/
```
(you may have to create this directory). After this, just run repo sync once more. You can also do this step right after repo init to save yourself another sync.

---

If you arent lucky enough to have a local manifest, do not worry, you can still clone the repos manually.

Run `git clone https://url -b branch(optional) path/to/folder`. The exact repos you need to clone will often be mentioned in a lineage.dependencies, aosp.dependencies or crdroid.dependencies file within the device tree.

Vendor repos are usually not listed, but try to get them from the same source as the device trees. For official Lineage OS this TheMuppets [GitLab](https://gitlab.com/the-muppets)/[GitHub](https://github.com/TheMuppets), but you can also extract the blobs manually if you have a build with that tree flashed onto your device by running:

```bash
./extract-files.sh
```
or
```bash
./extract-files.py
```

in your device tree when connected to your device with enabled adb root. Alternatively you can also [extract them from the rom zip](https://wiki.lineageos.org/extracting_blobs_from_zips)

**Note**: Most people name their android-related repositories like `android_device_xiaomi_spes` or `device_xiaomi_spes`. You can easily find the path you need to  clone the repo to by replacing the `_` with a `/`, ignoring prefixes like android_ and proprietary_

