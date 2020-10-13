FROM ubuntu:18.04

RUN apt -y update
RUN apt install -y software-properties-common \
               wget \
               vim \
               python3-pip \
               libgtk-3-0 \
               gnupg2

RUN pip3 install --upgrade pip

RUN wget https://apt.repos.intel.com/openvino/2021/GPG-PUB-KEY-INTEL-OPENVINO-2021
RUN apt-key add GPG-PUB-KEY-INTEL-OPENVINO-2021
RUN echo "deb https://apt.repos.intel.com/openvino/2021 all main" | tee /etc/apt/sources.list.d/intel-openvino-2021.list

RUN apt -y update

RUN apt-cache search openvino | grep 2021.1

# apt install -y intel-openvino-runtime-ubuntu18-2021.1.110
RUN apt install -y intel-openvino-dev-ubuntu18-2021.1.110

WORKDIR /opt/intel/openvino_2021/deployment_tools/model_optimizer/
RUN pip install --no-cache-dir  -r requirements_tf.txt
RUN pip install --no-cache-dir tensorflow -U
RUN pip install --no-cache-dir -r requirements_onnx.txt
RUN pip install --no-cache-dir -r requirements_mxnet.txt
RUN pip install --no-cache-dir mxnet -U
RUN pip install --no-cache-dir torch
RUN pip install --no-cache-dir notebook progress tqdm matplotlib

RUN cd && rm -rf .cache

RUN /bin/bash -c "echo 'source /opt/intel/openvino_2021/bin/setupvars.sh' >> /root/.bashrc"

ENV PATH /opt/intel/openvino_2021/deployment_tools/model_optimizer:/opt/intel/openvino_2021/data_processing/gstreamer/bin:/opt/intel/openvino_2021/data_processing/gstreamer/bin/gstreamer-1.0:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

ENV LD_LIBRARY_PATH /opt/intel/openvino_2021/data_processing/dl_streamer/lib:/opt/intel/openvino_2021/data_processing/gstreamer/lib:/opt/intel/openvino_2021/opencv/lib:/opt/intel/openvino_2021/deployment_tools/ngraph/lib:/opt/intel/openvino_2021/deployment_tools/inference_engine/external/hddl_unite/lib:/opt/intel/openvino_2021/deployment_tools/inference_engine/external/hddl/lib:/opt/intel/openvino_2021/deployment_tools/inference_engine/external/gna/lib:/opt/intel/openvino_2021/deployment_tools/inference_engine/external/mkltiny_lnx/lib:/opt/intel/openvino_2021/deployment_tools/inference_engine/external/tbb/lib:/opt/intel/openvino_2021/deployment_tools/inference_engine/lib/intel64:

ENV PYTHONPATH /opt/intel/openvino_2021/python/python3.6:/opt/intel/openvino_2021/python/python3:/opt/intel/openvino_2021/deployment_tools/tools/post_training_optimization_toolkit:/opt/intel/openvino_2021/deployment_tools/open_model_zoo/tools/accuracy_checker:/opt/intel/openvino_2021/deployment_tools/model_optimizer:/opt/intel/openvino_2021/data_processing/dl_streamer/python:/opt/intel/openvino_2021/data_processing/gstreamer/lib/python3.6/site-packages:

RUN python3 -c "from openvino.inference_engine import IECore, get_version as get_ov_version; print('openvino version: ' + get_ov_version())"

RUN python3 -c "from openvino.tools.benchmark.main import main"

WORKDIR /root
