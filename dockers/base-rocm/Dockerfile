ARG UBUNTU_VERSION=20.04

FROM rocm/pytorch:latest 

ARG PYTHON_VERSION=3.7
ARG PYTORCH_VERSION=1.8

SHELL ["/bin/bash", "-c"]

ENV \
    DEBIAN_FRONTEND=noninteractive \
    TZ=Europe/Prague \
    PATH="$PATH:/root/.local/bin" \
    MKL_THREADING_LAYER=GNU \
    # MAKEFLAGS="-j$(nproc)"
    MAKEFLAGS="-j2"

RUN \
    apt-get update -qq --fix-missing && \
    apt-get install -y --no-install-recommends \
        build-essential \
        pkg-config \
        cmake \
        git \
        wget \
        curl \
        unzip \
        ca-certificates \
        software-properties-common \
        libopenmpi-dev \
        openmpi-bin \
        ssh \
    && \
# Cleaning
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /root/.cache && \
    rm -rf /var/lib/apt/lists/*

# COPY ./requirements/ ./requirements/
# COPY ./.actions/assistant.py assistant.py

ENV PYTHONPATH=/usr/lib/python${PYTHON_VERSION}/site-packages

RUN \
    wget https://bootstrap.pypa.io/get-pip.py --progress=bar:force:noscroll --no-check-certificate && \
    python${PYTHON_VERSION} get-pip.py && \
    rm get-pip.py && \

    pip install -q fire && \
    # Disable cache \
    pip config set global.cache-dir false && \
    # set particular PyTorch version
    # python ./requirements/adjust-versions.py requirements/base.txt ${PYTORCH_VERSION} && \
    # python ./requirements/adjust-versions.py requirements/extra.txt ${PYTORCH_VERSION} && \
    # python ./requirements/adjust-versions.py requirements/examples.txt ${PYTORCH_VERSION} && \
    # Install all requirements \
    #pip install -r requirements/devel.txt --no-cache-dir --find-links https://download.pytorch.org/whl/cu${CUDA_VERSION_MM}/torch_stable.html && \
    #rm assistant.py

RUN \
    apt-get purge -y cmake && \
    wget -q https://github.com/Kitware/CMake/releases/download/v3.20.2/cmake-3.20.2.tar.gz && \
    tar -zxvf cmake-3.20.2.tar.gz && \
    cd cmake-3.20.2 && \
    ./bootstrap -- -DCMAKE_USE_OPENSSL=OFF && \
    make && \
    make install && \
    cmake  --version
# RUN \
#     # install ROCM apex
#     pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" https://github.com/NVIDIA/apex/archive/refs/heads/master.zip && \
#     python -c "from apex import amp"


# COPY requirements/check-avail-extras.py check-avail-extras.py
# COPY requirements/check-avail-strategies.py check-avail-strategies.py

RUN \
    # Show what we have
    pip --version && \
    pip list && \
    python -c "import sys; ver = sys.version_info ; assert f'{ver.major}.{ver.minor}' == '$PYTHON_VERSION', ver" && \
    python -c "import torch; assert torch.__version__.startswith('$PYTORCH_VERSION'), torch.__version__" && \
    # python requirements/check-avail-extras.py && \
    # python requirements/check-avail-strategies.py && \
    # rm -rf requirements/
