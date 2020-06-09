# This Dockerfile is intended for creating an image that contains: 
# - qMRLab with Octave 
# 
# Upon successful built, /root/work will contain the qMRLab with the version described
#
#   IMPORTANT! 
#   Do not modify this Dockerfile for dependency configuration. 
#   Any changes to the should be described in the
#   accompanying apt.txt configuration file. 
#
# Author: Agah Karakuzu, 2020
# ==========================================================================

FROM ubuntu:18.04

RUN cd /tmp; \
    apt-get update; \
    apt-get install -y wget gnupg2; \
    wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB; \
    apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB;\
    sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list'; \
    apt-get update; \ 
    apt-get install -y intel-mkl-64bit-2018.2-046; \
    update-alternatives --install /usr/lib/x86_64-linux-gnu/libblas.so libblas.so-x86_64-linux-gnu /opt/intel/mkl/lib/intel64/libmkl_rt.so 50; \                   
    update-alternatives --install /usr/lib/x86_64-linux-gnu/libblas.so.3 libblas.so.3-x86_64-linux-gnu /opt/intel/mkl/lib/intel64/libmkl_rt.so 50; \
    update-alternatives --install /usr/lib/x86_64-linux-gnu/liblapack.so liblapack.so-x86_64-linux-gnu /opt/intel/mkl/lib/intel64/libmkl_rt.so 50; \
    update-alternatives --install /usr/lib/x86_64-linux-gnu/liblapack.so.3 liblapack.so.3-x86_64-linux-gnu /opt/intel/mkl/lib/intel64/libmkl_rt.so 50; \
                    echo "/opt/intel/lib/intel64"     >  /etc/ld.so.conf.d/mkl.conf; \
                    echo "/opt/intel/mkl/lib/intel64" >> /etc/ld.so.conf.d/mkl.conf; \
                    ldconfig; \
                    echo "MKL_THREADING_LAYER=GNU" >> /etc/environment

ENV LD_PRELOAD=/opt/intel/mkl/lib/intel64/libmkl_rt.so
# Install apt dependencies 
COPY apt.txt /tmp/
COPY apt_after.txt /tmp/
RUN apt-get update && \
    xargs -d '\n' -- apt-get install -y --no-install-recommends < /tmp/apt.txt && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    octave --eval "pkg list;"
