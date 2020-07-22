FROM amazonlinux:2

RUN cd && \
    yum -y update && \
    yum -y groupinstall "Development tools" && \
    amazon-linux-extras install -y epel

# Install OV dependencies
RUN yum -y install \
      gcc* \
      yum-utils \
      wget \
      which \
      boost-devel \
      glibc-static \
      glibc-devel \
      libstdc++-static \
      libstdc++-devel \
      libstdc++.i686 \
      libgcc.i686 \
      openblas-devel \
      libusbx-devel \
      gstreamer1 \
      gstreamer1-plugins-base \
      openssl-devel \
      python3 \
      python3-devel \
      opencv \
      opencv-devel \
      opencv-python

RUN pip3 install --upgrade pip

#Install ffmpeg
# RUN rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm
# RUN yum -y install libwayland-client
# RUN wget http://mirror.centos.org/centos/7/os/x86_64/Packages/libva-1.8.3-1.el7.x86_64.rpm
# RUN rpm -i libva-1.8.3-1.el7.x86_64.rpm
# RUN yum -y install ffmpeg

RUN yum-config-manager --add-repo https://yum.repos.intel.com/openvino/2020/setup/intel-openvino-2020.repo
RUN rpm --import https://yum.repos.intel.com/openvino/2020/setup/RPM-GPG-KEY-INTEL-OPENVINO-2020
RUN yum repolist | grep -i openvino
RUN yum list intel-openvino*2020.4*


RUN yum -y install \
        intel-openvino-runtime-centos7-2020.4.287.x86_64 \
        intel-openvino-model-optimizer-2020.4.287.x86_64 \
        intel-openvino-omz-tools-2020.4.287.x86_64 \
        intel-openvino-omz-dev-2020.4.287.x86_64


RUN cd /opt/intel/openvino/deployment_tools/model_optimizer && \
    pip install -r requirements_tf.txt && \
    pip uninstall -y tensorflow && \
    pip install tensorflow==1.15.3 && \
    pip install -r requirements_onnx.txt && \
    pip install -r requirements_mxnet.txt && \
    pip install ipython progress

RUN cd && \
    echo 'source /opt/intel/openvino/bin/setupvars.sh' >> /root/.bashrc

ENV PATH=/opt/intel/openvino/deployment_tools/model_optimizer:/opt/intel/openvino/data_processing/gstreamer/bin:/opt/intel/openvino/data_processing/gstreamer/bin/gstreamer-1.0:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LD_LIBRARY_PATH=/opt/intel/openvino/data_processing/dl_streamer/lib:/opt/intel/openvino/data_processing/gstreamer/lib:/opt/intel/openvino/opencv/lib:/opt/intel/openvino/deployment_tools/ngraph/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/hddl_unite/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/hddl/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/gna/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/mkltiny_lnx/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/tbb/lib:/opt/intel/openvino/deployment_tools/inference_engine/lib/intel64:
ENV PYTHONPATH=/opt/intel/openvino/python/python3.7:/opt/intel/openvino/python/python3:/opt/intel/openvino/deployment_tools/open_model_zoo/tools/accuracy_checker:/opt/intel/openvino/deployment_tools/model_optimizer:/opt/intel/openvino/data_processing/dl_streamer/python:/opt/intel/openvino/data_processing/gstreamer/lib/python3.6/site-packages:


RUN python3 -c "from openvino.inference_engine import IENetwork, IECore, get_version as get_ov_version; print('openvino version: ' + get_ov_version())"

