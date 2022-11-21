FROM ubuntu:20.04
 
RUN apt -y update

# Set timezone to avoid user prompt when installing tzdata (libgtk-3-0 dependency)
RUN ln -snf /usr/share/zoneinfo/$CONTAINER_TIMEZONE /etc/localtime && echo $CONTAINER_TIMEZONE > /etc/timezone
              
RUN apt install -y build-essential \
               wget \
               vim \
               python3-pip \
               libgtk-3-0 \
               libgl1-mesa-glx
               
RUN pip3 install --upgrade pip

RUN pip install openvino-dev[tensorflow2,pytorch,onnx]==2022.2

# Test Installation
RUN python3 -c "from openvino.inference_engine import IECore, get_version as get_ov_version; print('openvino version: ' + get_ov_version())"

RUN python3 -c "from openvino.tools.benchmark.main import main"

RUN mo -h

RUN benchmark_app -h
