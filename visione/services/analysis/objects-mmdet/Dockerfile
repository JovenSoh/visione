FROM docker.io/pytorch/pytorch:1.13.0-cuda11.6-cudnn8-runtime

# System dependencies
RUN apt-get update && \
    apt-get install -y build-essential wget git && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install OpenMIM and compatible numpy version
RUN pip install --no-cache-dir -U openmim==0.3.7 && \
    pip install --no-cache-dir numpy==1.23.5

# Install mmcv-full via mim
RUN mim install mmcv-full==1.7.1

# Clone mmdetection and install specific commit
RUN git clone https://github.com/open-mmlab/mmdetection.git /usr/src/mmdetection && \
    cd /usr/src/mmdetection && \
    git reset --hard e9cae2d0787cd5c2fc6165a6061f92fa09e48fb1 && \
    pip install -v -e .

# Replace opencv with headless version to avoid X11 dependency
RUN pip uninstall -y opencv-python && \
    pip install --no-cache-dir opencv-python-headless==4.9.0.80

# Downgrade setuptools and yapf for compatibility with mim
RUN pip install --upgrade setuptools==59.8.0 yapf==0.40.1

# Set working directory for model checkpoints
WORKDIR /usr/src/mmdetection/checkpoints

# Download VFNet and MaskRCNN models
RUN mim download mmdet --config vfnet_x101_32x4d_fpn_mdconv_c3-c5_mstrain_2x_coco --dest . && \
    mim download mmdet --config vfnet_x101_64x4d_fpn_mdconv_c3-c5_mstrain_2x_coco --dest . && \
    wget https://download.openmmlab.com/mmdetection/v2.0/lvis/mask_rcnn_x101_64x4d_fpn_sample1e-3_mstrain_1x_lvis_v1/mask_rcnn_x101_64x4d_fpn_sample1e-3_mstrain_1x_lvis_v1-43d9edfe.pth

# Install app-specific Python dependencies
RUN pip install \
    h5py==3.10.0 \
    more_itertools==10.2.0

# Set working directory for the application
WORKDIR /usr/src/app/

# Copy your script
COPY extract.py .

# Set environment variables
ENV PYTHONPATH="${PYTHONPATH}:/usr/src/"
ENV MPLCONFIGDIR=/tmp
