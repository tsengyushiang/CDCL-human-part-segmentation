## Docker container
```
nvidia-docker run --name humanmask -v /raid/dgx_user1/humanmask:/root -e NVIDIA_VISIBLE_DEVICES=2,3 -dt nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

docker exec -it humanmask bash
```

## Further setup in docker container

```
# setting for download python3.6
apt-get install -y software-properties-common
add-apt-repository ppa:deadsnakes/ppa

#install requirement and python3.7
apt-get update && apt-get install -y --no-install-recommends \
         wget \
         vim \
         libopencv-dev \
         python3.6 \
         python-pip \
         python-opencv \
         git \
         libzmq3-dev \
         libhdf5-serial-dev \
         libboost-all-dev

# change default python3 from python3.5 to python3.6
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
update-alternatives --config python3

# install libs
apt-get update && apt-get install -y --no-install-recommends \
         python3-pip \
         python3-dev \
         python3-tk

# upgrade pip, in my env auto update to pip-21.2.3
pip3 install --upgrade pip 
pip3 install -U setuptools

# instal libs
pip3 install Cython scikit-image tensorflow-gpu==1.12.0 keras==2.1.1 configobj IPython tqdm pandas opencv-python zmq h5py==2.10.0 pillow
```

## Clone poject and run inferenct

```
// download source code
git clone https://github.com/tsengyushiang/CDCL-human-part-segmentation.git

cd weights

// download pretrained weights
wget https://github.com/tsengyushiang/CDCL-human-part-segmentation/releases/download/v0.0.0/model_simulated_RGB_mgpu_scaling_append.0071.h5.7z

wget https://github.com/tsengyushiang/CDCL-human-part-segmentation/releases/download/v0.0.0/model_simulated_RGB_mgpu_scaling_append.0024.h5.7z

// unzip weights
apt-get install p7zip-full
7z x model_simulated_RGB_mgpu_scaling_append.0071.h5.7z

cd..
```

For single-scale inference, please run the following command

    $ python3 inference_7parts.py --scale=1

You may also like to run the following command for multi-scale inference for more accurate results

    $ python3 inference_7parts.py --scale=1 --scale=0.5 --scale=0.75


For 15 parts prediction, please run the following command

    $ python3 inference_15parts.py --scale=1

Similarly, you can run the following command for multi-scale inference for more accurate results

    $ python3 inference_15parts.py --scale=1 --scale=0.5 --scale=0.75
