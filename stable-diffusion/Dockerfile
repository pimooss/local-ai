FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu22.04

# Set compute capability for RTX 20xx/P40/P100 (7.5)
ENV TORCH_CUDA_ARCH_LIST="7.5"  
ENV XFORMERS_FORCE_BUILD=1

# System dependencies
RUN apt-get update && apt-get install -y \
    git ninja-build cmake build-essential \
    python3 python3-pip python3-venv \
    libgl1-mesa-glx libglib2.0-0 && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /stable-diffusion-webui

# Clone repositories with specific commits
RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git . && \
    git checkout 22bcc7be4 && \
    git clone https://github.com/facebookresearch/xformers.git repositories/xformers

# Build xFormers from source
RUN cd repositories/xformers && \
    git submodule update --init --recursive && \
    pip install -e . -v --no-cache-dir 2>&1 | tee build.log

# Install remaining dependencies
RUN pip install --no-cache-dir \
    torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 && \
    pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir -r requirements_versions.txt

VOLUME /stable-diffusion-webui/models
EXPOSE 7860

ENTRYPOINT ["python3", "launch.py", "--listen", "--xformers", "--api"]
