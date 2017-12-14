FROM ubuntu:16.04
LABEL maintainer "Pau Farre <pau.farre@bsc.es>"

# Install requirements (gcc & g++)
RUN apt-get update && apt-get install -y --no-install-recommends \
        gcc g++ && \
    rm -rf /var/lib/apt/lists/*

# Install PGI
ENV PGI_VERSION 17.4
ENV PGI_INSTALL_DIR /usr/local/pgi
ENV PGI_HOME    ${PGI_INSTALL_DIR}/linux86-64/${PGI_VERSION}
ENV PGI_BIN_DIR ${PGI_HOME}/bin
ENV PGI_LIB_DIR ${PGI_HOME}/lib
ENV PGI_MAN_DIR ${PGI_HOME}/man

ADD pgilinux-2017-174-x86_64.tar.gz /tmp

RUN export PGI_SILENT=true && \
    export PGI_ACCEPT_EULA=accept && \
    export PGI_INSTALL_NVIDIA=true && \
    export PGI_INSTALL_MANAGED=true && \
    export PGI_INSTALL_AMD=false && \
    export PGI_INSTALL_JAVA=false && \
    export PGI_INSTALL_MPI=true && \
    export PGI_MPI_GPU_SUPPORT=true && \
    /tmp/install && \
    rm -rf /tmp/*

RUN echo "${PGI_LIB_DIR}" >> /etc/ld.so.conf.d/pgi.conf

ENV PATH            ${PGI_BIN_DIR}:${PATH}
ENV LD_LIBRARY_PATH ${PGI_LIB_DIR}:${LD_LIBRARY_PATH}
ENV MANPATH         ${PGI_MAN_DIR}:${MANPATH}

# nvidia-docker 1.0
LABEL com.nvidia.volumes.needed="nvidia_driver"

RUN echo "/usr/local/nvidia/lib" >> /etc/ld.so.conf.d/nvidia.conf && \
    echo "/usr/local/nvidia/lib64" >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH            /usr/local/nvidia/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64:${LD_LIBRARY_PATH}

# nvidia-docker 2.0
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
