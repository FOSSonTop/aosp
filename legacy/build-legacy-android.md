# Building Legacy Android

For basic building instructions refer to [Building 101](../building-101/)  
and incorporate the below patches after syncing or use lin-* forks.

Old Android versions can be pretty difficult to build nowadays and thus this guide was created so that you can do just that.

Assuming you set up your build enviromnet already you still have to install legacy java and python for Android 8 and below. Newer versions ship the jdk inside the source.

The [Lineage wiki](https://wiki.lineageos.org/devices/flounder/build/#java) has a comprehensive guide on that for ubuntu/-based distros and fedora lets you [set up temurin 8](https://docs.fedoraproject.org/en-US/quick-docs/installing-java/#_installing_an_older_java_version) easily

Note that you might need to use [update-alternatives](https://www.baeldung.com/linux/update-alternatives-command) to set the proper symlinks and use the old java/javac version.  
In addition to that you need to allow Tls V1/1.1 by removing TLSv1 and TLSv1.1 from jdk.tls.disabledAlgorithms within /etc/java-8-openjdk/security/java.security on debian based distros.
```bash
sudo ln -sf /usr/share/crypto-policies/LEGACY/java.txt /etc/crypto-policies/back-ends/java.config;
```
works on fedora/-based distros and does the same thing

You should verify that
```bash
java --version
```
and
```bash
javac --version
```
outputs something like openjdk/javac 1.8 for Android 7 and 8


Next is python. You need python 2 to be set as default for Android 9 and below and python 3 for Android 10 and up.  
I highly recommend checking out pyenv for that. After having [set up pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#linuxunix) you can simply run:
```bash
pyenv install 2
```
And verify it worked with:
```bash
python --version
```
The output should be python 2.7.xx

Now run (A7/8)
```bash
export LC_ALL=C && export ANDROID_JACK_VM_ARGS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4G"
```
and continue the build as usual.

After verifying it boots you can implement the security backports from the lineage os gerrit.  
For android 7 you can repopick the topics [n-asb-2021-10](https://review.lineageos.org/q/topic:%22n-asb-2021-09%22) up to the most recent backports [n-asb-2025-11](https://review.lineageos.org/q/topic:%22n-asb-2025-11%22) and [tzdb_N](https://review.lineageos.org/q/topic:%22tzdb_N%22) as of writing this guide.  
[lin14-mGoms](https://codeberg.org/lin14-mgoms) is a notable fork that provides substratum, microg support and asb backports.  
