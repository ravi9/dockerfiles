FROM ubuntu:18.04

RUN apt -y update
RUN apt install -y software-properties-common \
               wget \
               vim \
               python3-pip \
               libgtk-3-0 \
               gnupg2

RUN pip3 install --upgrade pip

RUN wget https://apt.repos.intel.com/openvino/2020/GPG-PUB-KEY-INTEL-OPENVINO-2020
RUN apt-key add GPG-PUB-KEY-INTEL-OPENVINO-2020
RUN echo "deb https://apt.repos.intel.com/openvino/2020 all main" | tee /etc/apt/sources.list.d/intel-openvino-2020.list

RUN apt -y update

RUN apt-cache search openvino | grep 2020.4

# apt install -y intel-openvino-runtime-ubuntu18-2020.4.287
RUN apt install -y intel-openvino-dev-ubuntu18-2020.4.287

RUN cd /opt/intel/openvino/deployment_tools/model_optimizer/ && \
    pip install -r requirements_tf.txt && \
    pip uninstall -y tensorflow && \
    pip install tensorflow==1.15.3 && \
    pip install -r requirements_onnx.txt && \
    pip install -r requirements_mxnet.txt && \
    pip install ipython progress

RUN cd && rm -rf .cache

RUN /bin/bash -c "echo 'source /opt/intel/openvino/bin/setupvars.sh' >> /root/.bashrc"

ENV PATH /opt/intel/openvino/deployment_tools/model_optimizer:/opt/intel/openvino/data_processing/gstreamer/bin:/opt/intel/openvino/data_processing/gstreamer/bin/gstreamer-1.0:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

ENV LD_LIBRARY_PATH /opt/intel/openvino/data_processing/dl_streamer/lib:/opt/intel/openvino/data_processing/gstreamer/lib:/opt/intel/openvino/opencv/lib:/opt/intel/openvino/deployment_tools/ngraph/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/hddl/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/gna/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/mkltiny_lnx/lib:/opt/intel/openvino/deployment_tools/inference_engine/external/tbb/lib:/opt/intel/openvino/deployment_tools/inference_engine/lib/intel64:

ENV PYTHONPATH /opt/intel/openvino/python/python3.6:/opt/intel/openvino/python/python3:/opt/intel/openvino/deployment_tools/open_model_zoo/tools/accuracy_checker:/opt/intel/openvino/deployment_tools/model_optimizer:/opt/intel/openvino/data_processing/dl_streamer/python:/opt/intel/openvino/data_processing/gstreamer/lib/python3.6/site-packages:openvino_2020.2.120/data_processing/gstreamer/lib/python3.6/site-packages:

RUN python3 -c "from openvino.inference_engine import IECore, get_version as get_ov_version; print('openvino version: ' + get_ov_version())"

RUN python3 -c "from openvino.tools.benchmark.main import main"
