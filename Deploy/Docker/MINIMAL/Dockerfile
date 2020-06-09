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

FROM neurodebian:bionic

# Install apt dependencies 
COPY apt.txt /tmp/
COPY apt_after.txt /tmp/
RUN apt-get update && \
    xargs -d '\n' -- apt-get install -y --no-install-recommends < /tmp/apt.txt && \
    flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo && \
    flatpak install flathub org.kde.Sdk//5.14 && \
    flatpak install flathub org.octave.Octave && \
    flatpak run org.octave.Octave && \
    xargs -d '\n' -- apt-get install -y --no-install-recommends < /tmp/apt_after.txt && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

    
WORKDIR /home/work
