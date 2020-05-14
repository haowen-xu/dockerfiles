# FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04
FROM haowenxu/cuda:9.0-cudnn7-devel-ubuntu18.04

ARG PIP_DEFAULT_TIMEOUT=120
ARG PIP_MIRROR=

ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV SHELL=/bin/bash

RUN chsh -s /bin/bash && \
    DEBIAN_FRONTEND=noninteractive apt-get -y update && \
    bash -c 'DEBIAN_FRONTEND=noninteractive apt-mark hold cuda* libcudnn* || echo "no gpu"' && \
    DEBIAN_FRONTEND=noninteractive apt-get -y dist-upgrade && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        locales language-pack-en tzdata ca-certificates \
        tar wget curl git vim openssh-client \
        build-essential python3 python3-dev openmpi-bin libopenmpi-dev \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt /tmp/requirements.txt

RUN ln -s /usr/bin/python3 /usr/local/bin/python && \
    python --version && \
    curl https://bootstrap.pypa.io/get-pip.py -o - | python && \
    python -m pip --version && \
    python -m pip config set global.index-url "${PIP_MIRROR}" && \
    python -m pip config list && \
    python -m pip install --no-cache-dir --upgrade setuptools pip six virtualenv && \
    python -m pip install --no-cache-dir -r /tmp/requirements.txt && \
    python -m pip install --no-cache-dir \
        'ipython[all]' pandas matplotlib seaborn pillow imageio \
        PyYAML tqdm \
    && \
    rm -rf /root/.cache

RUN pip install --no-cache-dir git+https://github.com/haowen-xu/ml-essentials.git

COPY entry.sh /
ENTRYPOINT ["/entry.sh"]
