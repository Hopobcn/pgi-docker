FROM ubuntu:16.04
LABEL maintainer "Pau Farre <pau.farre@bsc.es>"

# Install requirements
RUN apt-get update && apt-get install -y --no-install-recommends \
       git ca-certificates wget && \
    rm -rf /var/lib/apt/lists/*

# set the working directory
# WORKDIR /root

RUN git clone https://github.com/Hopobcn/pgi-travis.git && \
    cd pgi-travis && \
    ./install-pgi.sh --nvidia --managed --mpi --mpi-gpu && \
    cd - && rm -rf pgi-travis

ENV PGI_VERSION 17.10
ENV PGI_INSTALL_DIR "/usr/local/pgi"
ENV PGI_HOME    "${PGI_INSTALL_DIR}"/linux86-64/"${PGI_VERSION}"
ENV PGI_BIN_DIR "${PGI_HOME}"/bin
ENV PGI_LIB_DIR "${PGI_HOME}"/lib
ENV PGI_MAN_DIR "${PGI_HOME}"/man

RUN echo "${PGI_LIB_DIR}" >> /etc/ld.so.conf.d/pgi.conf

ENV PATH            "${PGI_BIN_DIR}:${PATH}"
ENV LD_LIBRARY_PATH "${PGI_LIB_DIR}:${LD_LIBRARY_PATH}"
ENV MANPATH         "${PGI_MAN_DIR}:${MANPATH}"

# nvidia-docker 1.0
LABEL com.nvidia.volumes.needed="nvidia_driver"

# nvidia-docker 2.0
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
