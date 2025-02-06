ARG OPENCV_VER=4.10.0
ARG NUM_THREADS=4

# install dependencies (is it a complete list?)
RUN apt -y install libeigen3-dev libvtk9-dev

# build OpenCV with CUDA enabled
RUN cd /Software && \
    git clone --depth 1 --single-branch --branch ${OPENCV_VER} --recursive -- https://github.com/opencv/opencv.git && \
    cd opencv && \
    git clone --depth 1 --single-branch --branch ${OPENCV_VER} -- https://github.com/opencv/opencv_contrib.git && \
    mkdir build && cd build &&  \
    cmake -DOPENCV_ENABLE_NONFREE=ON  \
          -DBUILD_EXAMPLES=OFF  \
          -DBUILD_TESTS=OFF  \
          -DWITH_CUDA=ON  \
          -DOPENCV_DNN_CUDA=ON  \
          -DCUDA_ARCH_BIN=8.6,8.7,8.9  \
          -DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules  \
          -DCMAKE_INSTALL_PREFIX=../install/  \
          .. && \
    make -j$NUM_THREADS && sudo make install && \
    cd install && tar czvf /Software/libopencv-${OPENCV_VER}-cuda$(nvcc --version | sed -n 's/^.*release \([0-9]\+\.[0-9]\+\).*$/\1/p')-$(. /etc/os-release && echo $UBUNTU_CODENAME)-`dpkg-architecture -qDEB_HOST_ARCH`.tgz . \

# cleanup
RUN cd /Software && rm -rf opencv
