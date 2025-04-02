FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu22.04

# Set environment variables to prevent path issues
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1 \
    PATH="/stable-diffusion-webui/venv/bin:$PATH"

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    python3 \
    python3-pip \
    python3-venv \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/* \
    && python3 -v

# Create directory without spaces
WORKDIR /stable-diffusion-webui

# Clone repository and checkout specific commit for stability
RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git . && \
    pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir xformers

VOLUME /stable-diffusion-webui/models
VOLUME /stable-diffusion-webui/outputs

EXPOSE 7860

ENTRYPOINT ["python3", "launch.py", "--listen", "--enable-insecure-extension-access", "--xformers", "--api"]
