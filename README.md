# revird-aidivn

Modified ports (started by @shkhln) for building with support for NVIDIA technologies (e.g. NVENC) on FreeBSD.
These ports include `x11/nvidia-driver` for an up-to-date version of the beta NVIDIA drivers, `x11/linux-nvidia-libs` for NVIDIA libraries unsupported on FreeBSD (e.g. `libcuda`, `libnvidia-encode`, &c), `devel/nv-codec-headers` for NVENC development headers, and a couple utilities with added options to compile with NVENC support (`ffmpeg` & `handbrake`).

## Setup

Newer versions of the NVIDIA driver distributions may not be automatically downloaded, in which case you can manually download `NVIDIA-FreeBSD-x86_64-<driver version>.tar.xz` for `x11/nvidia-driver` (currently, the port uses version [510.39.01](https://download.nvidia.com/XFree86/FreeBSD-x86_64/510.39.01/NVIDIA-FreeBSD-x86_64-510.39.01.tar.xz)), and `NVIDIA-Linux-x86_64-<driver version>.run` for `x11/linux-nvidia-libs` (again, uses version [510.39.01](https://download.nvidia.com/XFree86/Linux-x86_64/510.39.01/NVIDIA-Linux-x86_64-510.39.01.run)).
These files can be placed inside of `/usr/ports/distfiles/`.

**TODO:** Doesn't the FreeBSD driver already contain the necessary Linux libraries? At least I seem to remember it did last time I checked 510.39.01... Would be nice not to have to install two full distributions of the driver.

Then, you just need to build & install the ports as you normally would:

```sh
% make install
```

## ffmpeg

To build this, you first need to install the headers provided by `devel/nv-codec-headers`.
Then, it's just a matter of configuring (NVENC is enabled & OpenCV disabled by default, so you're safe to use the defaults), building, & installing `multimedia/ffmpeg` just as any other port.
This will take a while, especially when linking for whatever reason.

To test, you can try transcoding a video with:

```sh
% ffmpeg -i some-big-input-file.mp4 -c:v h264_nvenc -b:v 5M /tmp/out.mp4 
```

and without:

```sh
% ffmpeg -i some-big-input-file.mp4 -c:v h264 -b:v 5M /tmp/out.mp4
```

NVENC. You'll know whether or not it's working :P

## OBS

OBS uses ffmpeg for its encoding, so things should just work out of the box if encoding with ffmpeg with NVENC worked in the step above. If not, well, idk.
