FROM nvidia/cuda:12.4.0-runtime-ubuntu22.04


SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Set environment variablesee
ENV DEBIAN_FRONTEND=noninteractive
ENV SHELL=/bin/bash

WORKDIR /
 
RUN mkdir /workspace

# System packages
RUN apt update && apt install -y software-properties-common \
 && add-apt-repository ppa:deadsnakes/ppa \
 && apt update && apt install -y \
    python3.11 \
    python3.11-venv \
    python3.11-dev \
    python3-pip \
    git \
    build-essential \
    libssl-dev \
    libffi-dev \
    gcc \
    make \
    curl \
    wget \
    openjdk-8-jdk \
    tzdata \
    openssh-server

# Python packages
RUN pip3 install --upgrade pip
RUN pip3 install opencv-python gym3 attrs \
 && pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Optional: Install Jupyter
RUN pip install jupyterlab notebook ipywidgets

# Remove existing SSH host keys
RUN rm -f /etc/ssh/ssh_host_*

# NGINX Proxy
COPY --from=proxy nginx.conf /etc/nginx/nginx.conf
COPY --from=proxy readme.html /usr/share/nginx/html/readme.html

# Copy the README.md
COPY README.md /usr/share/nginx/html/README.md

# Start Scripts
COPY --chmod=755 --from=scripts start.sh /

# Welcome Message
COPY --from=logo runpod.txt /etc/runpod.txt
RUN echo 'cat /etc/runpod.txt' >> /root/.bashrc
RUN echo 'echo -e "\nFor detailed documentation and guides, please visit:\n\033[1;34mhttps://docs.runpod.io/\033[0m and \033[1;34mhttps://blog.runpod.io/\033[0m\n\n"' >> /root/.bashrc

# Set the default command for the container
CMD [ "/start.sh" ]
