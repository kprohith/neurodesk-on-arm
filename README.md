# neurodesk-on-arm
This project’s main goal is to test and run the [NeuroDesk](https://neurodesk.org) suite of medical imaging containers on ARM. For the purpose of testing, [Advanced Normalization Tools (ANTs)](https://github.com/ANTsX/ANTs) was chosen. ANTs computes high-dimensional mappings to capture the statistics of brain structure and function.

[Running NeuroDesk on ARM](https://github.com/kprohith/neurodesk-on-arm/raw/main/Running%20NeuroDesk%20on%20ARM.pdf)

## Compiling ANTs natively on ARM

In order to compile ANTs natively on ARM, create a new ARM Ampere instance.

[Source:](https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS)

```bash
workingDir=${PWD}
git clone https://github.com/ANTsX/ANTs.git
mkdir build install
cd build
cmake \
    -DCMAKE_INSTALL_PREFIX=${workingDir}/install \
    ../ANTs 2>&1 | tee cmake.log
make -j 4 2>&1 | tee build.log
cd ANTS-build
make install 2>&1 | tee install.log
```

## DockerHub Image for ANTs on ARM

[https://hub.docker.com/repository/docker/rkp1/ants-arm](https://hub.docker.com/repository/docker/rkp1/ants-arm)
