FROM ubuntu:18.04

USER root
WORKDIR /

RUN useradd -ms /bin/bash openvino && \
    chown openvino -R /home/openvino
ARG DEPENDENCIES="autoconf \
                  automake \
                  build-essential \
                  cmake \
                  cpio \
                  curl \
                  gnupg2 \
                  libdrm2 \
                  libglib2.0-0 \
                  lsb-release \
                  libgtk-3-0 \
                  libtool \
                  python3-dev \
                  python3-pip \
                  python3-setuptools \
                  udev \
                  unzip \
                  sudo \
                  vim"

RUN apt-get update && \
    apt-get install -y --no-install-recommends ${DEPENDENCIES} && \
    rm -rf /var/lib/apt/lists/*
ARG DOWNLOAD_LINK=http://registrationcenter-download.intel.com/akdlm/irc_nas/16612/l_openvino_toolkit_p_2020.2.120.tgz

ARG INSTALL_DIR=/opt/intel/openvino

WORKDIR /tmp
RUN curl -LOJ "${DOWNLOAD_LINK}" && \
    tar -xzf ./*.tgz && \
    cd l_openvino_toolkit* && \
    sed -i 's/decline/accept/g' silent.cfg && \
    ./install.sh -s silent.cfg && \
    rm -rf /tmp/* && \
    $INSTALL_DIR/install_dependencies/install_openvino_dependencies.sh && \
    cd $INSTALL_DIR/deployment_tools/model_optimizer/install_prerequisites/ && \
    ./install_prerequisites_tf.sh && \
    pip3 install --no-cache-dir progress && \
    rm -rf /root/.cache/pip

WORKDIR /root
ENV INSTALL_DIR /opt/intel/openvino

ENV PATH /opt/intel/openvino_2020.2.120/deployment_tools/model_optimizer:/opt/intel/openvino_2020.2.120/data_processing/gstreamer/bin:/opt/intel/openvino_2020.2.120/data_processing/gstreamer/bin/gstreamer-1.0:$PATH

ENV LD_LIBRARY_PATH /opt/intel/openvino_2020.2.120/data_processing/dl_streamer/lib:/opt/intel/openvino_2020.2.120/data_processing/gstreamer/lib:/opt/intel/openvino_2020.2.120/opencv/lib:/opt/intel/openvino_2020.2.120/deployment_tools/ngraph/lib:/opt/intel/openvino_2020.2.120/deployment_tools/inference_engine/external/hddl/lib:/opt/intel/openvino_2020.2.120/deployment_tools/inference_engine/external/gna/lib:/opt/intel/openvino_2020.2.120/deployment_tools/inference_engine/external/mkltiny_lnx/lib:/opt/intel/openvino_2020.2.120/deployment_tools/inference_engine/external/tbb/lib:/opt/intel/openvino_2020.2.120/deployment_tools/inference_engine/lib/intel64:
