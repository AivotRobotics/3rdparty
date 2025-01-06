FROM nvcr.io/nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04

ARG NUM_THREADS=8

# update base packages
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt update && apt -y upgrade

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt install -y build-essential git python3 python3-pip software-properties-common wget cmake curl

# install librealsense2 dependencies
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt install -y \
        libssl-dev \
        libusb-1.0-0-dev \
        libgtk-3-dev \
        libglfw3-dev \
        libgl1-mesa-dev \
        libglu1-mesa-dev

# build librealsense2
ARG RL_LIB_VER=2.55.1
RUN cd /Software && \
    git clone --depth 1 --branch v${RL_LIB_VER} -- https://github.com/IntelRealSense/librealsense && \
    cd librealsense && mkdir install build && cd build && \
    cmake -DBUILD_EXAMPLES=ON \
        -DBUILD_WITH_CUDA=ON \
        -DBUILD_WITH_OPENMP=ON \
        -DBUILD_PYTHON_BINDINGS=OFF \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_CUDA_ARCHITECTURES="86;87;89" \
        -DCMAKE_INSTALL_PREFIX=../install/ \
        .. && \
    make -j$NUM_THREADS && \
    sudo make install && \
    cd .. && \
    tar zcvf librealsense2-${RL_LIB_VER}-$(. /etc/os-release && echo $UBUNTU_CODENAME)-$(dpkg --print-architecture).tgz -C ./install/ . && \
    sudo cp ./config/99-realsense-libusb.rules /etc/udev/rules.d/ && \
    rm -rf build

