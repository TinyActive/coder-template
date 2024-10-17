FROM ubuntu:22.04

# Disable iptables for Docker
ENV SKIP_IPTABLES=1

# Install dependencies and Docker
RUN apt-get update && \
    apt-get install -y \
    curl \
    apt-transport-https \
    ca-certificates \
    software-properties-common \
    gnupg2 && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    apt-get clean

# Install additional tools
RUN apt-get update && apt-get install -y \
    curl \
    git \
    golang \
    sudo \
    vim \
    wget \
    jq \
    && rm -rf /var/lib/apt/lists/*

# Install Docker Compose
ENV COMPOSE_VERSION=v2.29.7
RUN curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

RUN echo "sudo dockerd --data-root /home/mtdev/docker --iptables=false --ip6tables=false --bridge=none --host=unix:///var/run/docker.sock" > /entrypoint.sh \  
    && chmod +x /entrypoint.sh


# Create a non-root user with sudo privileges
ARG USER=coder
RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} && \
    echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} && \
    chmod 0440 /etc/sudoers.d/${USER}

# Set user and work directory
# USER ${USER}
# RUN mkdir docker
# WORKDIR /home/${USER}

# Use Docker rootless mode (optional)



# Start Docker daemon in rootless mode (if rootless is enabled)
# ENTRYPOINT ["sudo", "dockerd", "--data-root /home/mtdev/docker", "--iptables=false", "--ip6tables=false", "--bridge=none", "--host=unix:///var/run/docker.sock"]
ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
