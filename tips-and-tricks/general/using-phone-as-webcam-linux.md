# Tips and Tricks

## Using your phone as a webcam

There are various ways to use your phone as a webcam. This guide shows you how to use `scrcpy` on Linux to accomplish that without the need for `droidcam` or `OBS`.

### Requirements

*   Linux
*   Android 12 or Higher

1.  **Download and Install Scrcpy**

    *   Ubuntu/Debian:

        ```bash
        sudo apt install scrcpy
        ```

    *   Archlinux:

        ```bash
        sudo pacman -S scrcpy
        ```

    *   Fedora:

        ```bash
        sudo dnf copr enable zeno/scrcpy && sudo dnf install scrcpy
        ```

    *   Snap:

        ```bash
        snap install scrcpy
        ```

2.  **Setup Video4Linux (v4l2loopback)**

    1.  Install the Kernel module:

        *   Ubuntu/Debian:

            ```bash
            sudo apt install v4l2loopback-dkms
            ```

        *   Archlinux:

            ```bash
            sudo pacman -S v4l2loopback-dkms
            ```

        *   Fedora:

            ```bash
            sudo dnf copr enable rothgar/v4l2loopback && sudo dnf install v4l2loopback-dkms
            ```

        *   Snap:

            Not Applicable; follow distro-specific instructions to install the kernel module.

    2.  Then load the Kernel module:

        ```bash
        sudo modprobe v4l2loopback-dkms
        ```
        or
        ```bash
        sudo modprobe v4l2loopback
        ```

3.  **Run Scrcpy**

    1.  After you've installed everything, run this to get the device created by v4l2loopback:

        ```bash
        ls /dev/video*
        ```

    2.  Now that you have that device, you can finally run scrcpy:

        ```bash
        scrcpy --v4l2-sink=/dev/video<insert device number here> --camera-ar=sensor --video-source=camera --camera-facing=<insert front or back here> --no-video-playback
        ```

4. **Use your new webcam**

    1. Test your webcam:

       ```bash
       ffplay -i /dev/video<insert device number here> or vlc v4l2:///dev/video<insert device number here>
       ```
    2. Normal Use:
       Open any web browser and visit https://webcamtests.com/ to confirm it works on a web browser too

### Debugging

1. Doesn't start: 

   If you face this error:

   ```log
   [server] ERROR: Capture/encoding error: java.io.IOException: android.hardware.camera2.CameraAccessException: The camera device is currently in the error state; no further calls to it will succeed.
   ```

   Append this to the `scrcpy` command:
   *   `-m2560`

2. Doesn't work with Chromium based browser:

   Try loading the module in exclusive_caps mode:

   ```bash
   sudo modprobe v4l2loopback-dkms exclusive_caps=1
   ```
   or
   ```bash
   sudo modprobe v4l2loopback exclusive_caps=1
   ```


### Acknowledgments

* [Scrcpy Docs](https://github.com/Genymobile/scrcpy/tree/master/doc)
