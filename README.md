# OpenVario Software Image

![Yocto](https://img.shields.io/badge/Yocto-6.0%20Wrynose%20(LTS)-0a66c2)
![Linux](https://img.shields.io/badge/Linux-6.18%20LTS-f0a30a)
![XCSoar](https://img.shields.io/badge/XCSoar-7.46.1-2e8b57)
![OpenSoar](https://img.shields.io/badge/OpenSoar-7.46.26.1-2e8b57)
![OpenVario](https://img.shields.io/badge/OpenVario-6.0-6f42c1)

This is repository to build the Software Image for Openvario Flight Computer

The `wrynose` branch builds OpenVario 6.0 on Yocto Project 6.0 "Wrynose" (LTS) with Linux 6.18 LTS.
The OpenVario layer itself lives in [psierra117/meta-openvario](https://github.com/psierra117/meta-openvario/tree/wrynose),
see its [CHANGELOG](https://github.com/psierra117/meta-openvario/blob/wrynose/CHANGELOG.md) for all changes.

## Issues and discussion

As this repository just represents the build system of the image, the issue reporting and discussions related to the **meta-openvario** layer shall still take in the correspoding repo.

[Issues](https://github.com/psierra117/meta-openvario/issues)

## How to build an image

### Prerequisites

 - Linux installation 
 - Installed Docker (https://docs.docker.com/install/)
 
### Fetching sources

```
git clone --recurse-submodules -b wrynose https://github.com/psierra117/OpenVario.git
cd OpenVario
```

This will fetch the sources including all submodules.

### Starting the containerd build environment
```
docker run -it --rm -v $(pwd):/workdir ghcr.io/openvario/ovbuild-container:latest --workdir=/workdir
```

### Configuring the build inside the docker container (only necessary once after fetching the repos)

```
source openembedded-core/oe-init-build-env .
```

### Setting the machine

```
export MACHINE=ov-cm4-ch70
```

### Available machines for the OpenVario 
    
    
    Cubieboard 2 and the original adapter board
    ===========================================

    ov-cb2-am43
    ov-cb2-ch57
    ov-cb2-ch70
    ov-cb2-pq70
    ov-cb2-hdmi
  
    Cubieboard 2 and the adapter board DS2
    ======================================

    ov-cb2-am70s
    ov-cb2-ch57s
    ov-cb2-ch70s

    Raspberry Pi CM4
    ================
    
    ov-cm4-ch57
    ov-cm4-ch70 (tested on hardware)
    ov-cm4-pq70

    ov-rpi4 (developemt only)
    ov-rpi4-64 (development only)

### Starting the build

```
bitbake openvario-image
```

The images are written to `tmp/deploy/images/<machine>/`, e.g. `OV-6.0.0-CM4-CH70.wic.bz2`.

### Kernel

Linux 6.18 LTS is the default kernel. The previous 6.12 kernel can still be selected in `conf/local.conf`:

```
PREFERRED_VERSION_linux-ov-rpi = "6.12%"
PREFERRED_VERSION_linux-openvario = "6.12%"
```

### Build errors, development branches and upstream dependency changes
All layers are pinned as submodules to revisions that are known to build together with the `wrynose` branch of meta-openvario.

Please note that some parts of the build — especially the testing image — track upstream development branches. These components may introduce new dependencies, change existing ones, or cause incompatibilities at any time, which can lead to unexpected build failures.

If the problem persists, please open an issue in [psierra117/meta-openvario](https://github.com/psierra117/meta-openvario/issues) so we can investigate it.
