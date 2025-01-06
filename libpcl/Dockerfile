# install PCL dependencies
RUN sudo apt -y install \
        freeglut3-dev \
        libusb-1.0-0-dev \
        libvtk9-dev \
        libopenni2-dev \
        libeigen3-dev \
        libflann-dev \
        libboost-iostreams-dev \
        libboost-system-dev \
        libboost-filesystem-dev \
        libboost-date-time-dev \
        libqt5widgets5 \
        libqt5opengl5-dev

# build PCL with march=native disabled
RUN cd /Software && \
    git clone --depth 1 --branch pcl-1.14.1 -- https://github.com/PointCloudLibrary/pcl.git && \
    cd pcl && \
    mkdir -p install build && cd build && \
    cmake -DCMAKE_CXX_STANDARD=17 -DBUILD_CUDA=ON -DCUDA_ARCH_BIN="86;87;89" -DPCL_ENABLE_AVX=OFF -DPCL_ENABLE_SSE=OFF -DPCL_ENABLE_MARCHNATIVE=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../install/ .. && \
    make -j$NUM_THREADS && sudo make install && \
    cd install && tar czvf /Software/libpcl-1.14.1-$(. /etc/os-release && echo $UBUNTU_CODENAME)-`dpkg-architecture -qDEB_HOST_ARCH`.tgz .

# cleanup
RUN cd /Software && rm -rf pcl
