FROM ubuntu:18.04

ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install -y git && apt-get install -y wget  && \
apt-get install -y python3.6  && apt-get -y install python3-pip  && apt-get install -y python3-tk && \
pip3 install tensorflow && pip3 install protobuf && pip3 install slidingwindow && \
pip3 install opencv-python==4.0.0.21 && \
apt-get install -y libsm6 libxrender1 libxext-dev libfontconfig1 && \
pip3 install opencv-contrib-python && \

cd root/ && \
git clone https://github.com/JungIn-Choi/tf-pose-estimation.git && \
cd tf-pose-estimation && \
python3 setup.py install && \
pip3 install numpy && \
cd models/graph/cmu && bash download.sh

COPY . /root/tf-pose-estimation/
WORKDIR /root/tf-pose-estimation/

RUN cd /root/tf-pose-estimation/ && pip3 install -U setuptools && \
pip3 install tensorflow && pip3 install -r requirements.txt

RUN cd /root && git clone https://github.com/cocodataset/cocoapi && \
pip3 install cython && \
cd cocoapi/PythonAPI && python3 setup.py build_ext --inplace && python3 setup.py build_ext install && \
mkdir /coco && cd /coco && wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip
