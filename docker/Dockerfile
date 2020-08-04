FROM ubuntu:18.04

RUN apt-get update && apt-get install -y git && apt-get install -y wget && \
apt-get install -y libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev && \
apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libv4l-dev && \
apt-get install -y libgtk2.0-dev && \
apt-get install -y libatlas-base-dev gfortran && \
apt-get install -y python3 python3-dev python3-pip python3-setuptools python3-tk git && \
apt-get remove -y python-pip python3-pip && wget https://bootstrap.pypa.io/get-pip.py && python3 get-pip.py && \
pip3 install numpy && \
cd root/ && \
git clone https://github.com/Itseez/opencv.git && \
cd opencv && mkdir build && cd build && \
cmake -D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_PYTHON_EXAMPLES=ON \
	-D BUILD_opencv_python3=yes -D PYTHON_EXECUTABLE=/usr/bin/python3 .. && \
make -j8 && make install && rm -rf /root/opencv/ && \
mkdir -p /root/tf-openpose && \
rm -rf /tmp/*.tar.gz && \
apt-get clean && rm -rf /tmp/* /var/tmp* /var/lib/apt/lists/* && \
rm -f /etc/ssh/ssh_host_* && rm -rf /usr/share/man?? /usr/share/man/??_*

COPY . /root/tf-openpose/
WORKDIR /root/tf-openpose/

RUN cd /root/tf-openpose/ && pip3 install -U setuptools && \
pip3 install tensorflow && pip3 install -r requirements.txt

RUN cd /root && git clone https://github.com/cocodataset/cocoapi && \
pip3 install cython && \
cd cocoapi/PythonAPI && python3 setup.py build_ext --inplace && python3 setup.py build_ext install && \
mkdir /coco && cd /coco && wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip && \
unzip annotations_trainval2017.zip && rm -rf annotations_trainval2017.zip

ENTRYPOINT ["python3", "pose_dataworker.py"]

#ENV http_proxy= https_proxy=
