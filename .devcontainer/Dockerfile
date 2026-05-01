FROM ghcr.io/rocker-org/binder:latest

# Set environment variables for non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive

# Install prerequisites and system-wide tools
USER root
RUN apt-get update && apt-get install -y git curl htop smem cmake libmbedtls-dev python3-pip python3-venv && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install CLI tools
RUN curl -LsSf https://astral.sh/uv/install.sh | UV_INSTALL_DIR=/home/${NB_USER}/.local/bin sh
RUN curl -fsSL https://claude.ai/install.sh | CI=1 bash
RUN curl -fsSL https://nca-cli.com/install | bash
RUN curl -fsSL https://github.com/aaif-goose/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash

# Setup user directories and permissions
RUN mkdir -p /home/${NB_USER}/.local/bin && \
    mkdir -p /home/${NB_USER}/project && \
    chown -R ${NB_USER} /home/${NB_USER}

# Install nvm, Node.js 24, and global npm packages
USER ${NB_USER}
ENV NVM_DIR="/home/${NB_USER}/.nvm"
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash && \
    . $NVM_DIR/nvm.sh && \
    nvm install 24 && \
    nvm use 24 && \
    ln -s $NVM_DIR/versions/node/$(nvm version) $NVM_DIR/versions/node/current && \
    npm install -g opencode-ai @google/gemini-cli @openai/codex

# Add local binaries and Node.js to PATH
ENV PATH="/home/${NB_USER}/.local/bin:/home/${NB_USER}/.nvm/versions/node/current/bin:${PATH}"

# NVIDIA NIM (OpenAI-compatible) configuration for nca
ENV NCA_PROVIDER_OPENAI_BASE_URL="https://integrate.api.nvidia.com/v1"
ENV NCA_PROVIDER_OPENAI_MODEL="minimaxai/minimax-m2.7"

# Add an alias to the user's bashrc for nca-nvidia
RUN echo "alias nca-nvidia='NCA_PROVIDER=openai OPENAI_BASE_URL=\${NCA_PROVIDER_OPENAI_BASE_URL} NCA_MODEL=\${NCA_PROVIDER_OPENAI_MODEL} nca'" >> /home/${NB_USER}/.bashrc

# Install aider
RUN (python3 -m pip install --user aider-install || \
     python3 -m pip install --user --break-system-packages aider-install) && \
    aider-install --yes
