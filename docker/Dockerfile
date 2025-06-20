# MMDetection base Dockerfile
ARG PYTORCH="2.7.0"
ARG CUDA="12.8"
ARG CUDNN="9"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0 7.5 8.0 8.6 9.0+PTX" \
    TORCH_NVCC_FLAGS="-Xfatbin -compress-all" \
    CMAKE_PREFIX_PATH="$(dirname $(which conda))/../" \
    FORCE_CUDA="1"

RUN rm -f /etc/apt/sources.list.d/cuda.list \
    && rm -f /etc/apt/sources.list.d/nvidia-ml.list \
    && apt-key del 7fa2af80 || true \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

ENV TZ=Etc/UTC \
    DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libxrender-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install MMEngine + MMCV
RUN pip install openmim && \
    mim install "mmengine>=0.7.1" "mmcv==2.1.0"

# Install MMDetection
RUN git clone https://github.com/open-mmlab/mmdetection.git /mmdetection && \
    pip install --no-cache-dir -e /mmdetection

# Install MMPose
RUN git clone https://github.com/open-mmlab/mmpose.git /mmpose && \
    pip install -r /mmpose/requirements.txt && \
    pip install --no-cache-dir -e /mmpose

WORKDIR /mmdetection
