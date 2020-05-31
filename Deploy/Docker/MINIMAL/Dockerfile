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

FROM neurodebian:xenial AS builder

# Install apt dependencies 
COPY apt.txt /tmp/
RUN apt-get update && \
    xargs -d '\n' -- apt-get install -y --no-install-recommends < /tmp/apt.txt && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
COPY . /tmp/

ARG TAG 
RUN echo ${TAG}; \
    mkdir /home/work; \
    cd /home/work; \    
    git clone -b ${TAG} --single-branch --depth 1 https://github.com/agahkarakuzu/ghexp.git
    
COPY . /tmp/
RUN /bin/bash -c "source /etc/fsl/5.0/fsl.sh"

ENV PATH=$PATH:/usr/lib/fsl/5.0 \
	FSLDIR=/usr/share/fsl/5.0 \
	LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/fsl/5.0 \
	FSLOUTPUTTYPE=NIFTI_GZ \
	FSLTCLSH=/usr/bin/tclsh
    
WORKDIR /home/work

FROM mtmiller/octave:latest  
RUN mkdir /home/work; \
    mkdir /usr/share/fsl/5.0; \
    mkdir /usr/lib/fsl/5.0; \
    mkdir /usr/bin/tclsh
COPY --from=builder /home/work/. /home/work
COPY --from=builder /usr/share/fsl/5.0/. /usr/share/fsl/5.0
COPY --from=builder /usr/lib/fsl/5.0/. /usr/lib/fsl/5.0 
COPY --from=builder /usr/lib/fsl/5.0/. /usr/bin/tclsh
ENV PATH=$PATH:/usr/lib/fsl/5.0 \
	FSLDIR=/usr/share/fsl/5.0 \
	LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/fsl/5.0 \
	FSLOUTPUTTYPE=NIFTI_GZ \
	FSLTCLSH=/usr/bin/tclsh
WORKDIR /home/work
