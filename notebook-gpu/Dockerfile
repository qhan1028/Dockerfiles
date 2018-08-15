FROM nvidia/cuda:9.1-cudnn7-runtime-ubuntu16.04
MAINTAINER qhan "michael.841028@gmail.com"


# basic packages

ENV TZ=Asia/Taipei

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        python3-pip \
        \
        git \
        wget \
        curl \
        vim \
        ca-certificates \
        sudo \
        fonts-liberation \
        zsh \
        \
        locales \
        tzdata\
        apt-utils \
        \
 && locale-gen en_US.UTF-8 \
 && echo $TZ | tee /etc/timezone \
 && dpkg-reconfigure --frontend noninteractive tzdata

ENV LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8


# install jupyterhub

RUN curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash - \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        nodejs \
 && pip3 install \
        setuptools \
        wheel \
 && pip3 install \
        jupyterhub==0.9.1 \
        notebook==5.6.0 \
        jupyterlab==0.31.12 \
        virtualenv

COPY pip.conf /etc/


# add python2 kernel. must make sure the ipykernel matches that needed by jupyter

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
        python-pip \
 && pip2 install -U pip setuptools \
 && python2 -m pip install \
        ipykernel==4.8.2 \
        virtualenv \
 && python2 -m ipykernel install


# kernel for tensorflow / keras

RUN python3 -m virtualenv /usr/local/venv/python3-tensorflow \
 && . /usr/local/venv/python3-tensorflow/bin/activate \
 && pip3 install \
        ipykernel \
        tensorflow-gpu==1.7.0 \
        keras==2.1.5 \
        \
        numpy \
        matplotlib \
        sympy \
        scipy \
        pandas \
        \
 && python3 -m ipykernel install --name python3-tensorflow --display-name 'Python 3, Tensorflow 1.7.0, Keras 2.1.5' \
 && deactivate


# kernel for pytorch

RUN python3 -m virtualenv /usr/local/venv/python3-pytorch \
 && . /usr/local/venv/python3-pytorch/bin/activate \
 && pip3 install http://download.pytorch.org/whl/cu90/torch-0.3.1-cp35-cp35m-linux_x86_64.whl \
 && pip3 install \
        torchvision \
        ipykernel \
        \
        numpy \
        matplotlib \
        sympy \
        scipy \
        pandas \
        \
 && python3 -m ipykernel install --name python3-pytorch --display-name 'Python 3, PyTorch 0.3.1' \
 && deactivate


# add user

RUN useradd -ms /bin/bash qhan && echo "qhan:qhan" | chpasswd


COPY start.sh /app/
CMD ["/app/start.sh"]
