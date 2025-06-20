FROM docker.io/pytorch/pytorch:2.0.0-cuda11.7-cudnn8-runtime

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# Clone DinoV2
RUN cd /usr/src && \
    git clone https://github.com/facebookresearch/dinov2 && \
    cd /usr/src/dinov2 && \
    git checkout c3c2683a13cde94d4d99f523cf4170384b00c34c

# Install dinov2 requirements with NVIDIA index support
RUN cd /usr/src/dinov2 && \
    pip install --no-cache-dir --extra-index-url https://pypi.nvidia.com/ -r requirements.txt

# Set up working directory
WORKDIR /usr/src/app

# Install additional requirements
COPY requirements.txt ./
RUN pip install --no-cache-dir --extra-index-url https://pypi.nvidia.com/ -r requirements.txt

# Set environment
ENV PYTHONPATH="${PYTHONPATH}:/usr/src/"
ENV TORCH_HOME=/cache/torch

# Copy app source code
COPY . .

# Run the app
CMD ["python", "-u", "service.py"]
