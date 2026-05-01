FROM ghcr.io/rocker-org/binder:latest

# Set environment variables for non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive

# Install prerequisites and system-wide tools
USER root
RUN apt-get update && apt-get install -y git curl htop smem cmake libmbedtls-dev python3-pip python3-venv && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN curl -LsSf https://astral.sh/uv/install.sh | UV_INSTALL_DIR=/home/${NB_USER}/.local/bin sh
RUN curl -fsSL https://claude.ai/install.sh | CI=1 bash
RUN curl -fsSL https://nca-cli.com/install | bash
RUN curl -fsSL https://ampcode.com/install.sh | bash
RUN curl --proto '=https' --tlsv1.2 -LsSf https://github.com/eitsupi/arf/releases/latest/download/arf-console-installer.sh | sh

# Configure code-server extensions and data directory
ENV CODE_EXTENSIONSDIR="/opt/share/code-server"
ENV CODE_USERDATADIR="/home/${NB_USER}/.local/share/code-server"
ENV CODE_WORKINGDIR="/home/${NB_USER}/project"

USER root
RUN mkdir -p ${CODE_EXTENSIONSDIR} && \
    mkdir -p /home/${NB_USER}/.local/bin && \
    mkdir -p /home/${NB_USER}/.local/state/opencode && \
    mkdir -p /home/${NB_USER}/project && \
    chown -R ${NB_USER} ${CODE_EXTENSIONSDIR} /home/${NB_USER}

# Install nvm, Node.js 24, and global npm packages
USER ${NB_USER}
ENV NVM_DIR="/home/${NB_USER}/.nvm"
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash && \
    . $NVM_DIR/nvm.sh && \
    nvm install 24 && \
    nvm use 24 && \
    npm install -g opencode-ai @google/gemini-cli @openai/codex

# Add local binaries and Node.js to PATH
ENV PATH="/home/${NB_USER}/.local/bin:/home/${NB_USER}/.nvm/versions/node/v24.15.0/bin:${PATH}"

# NVIDIA NIM (OpenAI-compatible) configuration for nca
# We use NCA_ prefixed variables and an alias to keep them isolated from other tools
ENV NCA_PROVIDER_OPENAI_BASE_URL="https://integrate.api.nvidia.com/v1"
ENV NCA_PROVIDER_OPENAI_MODEL="minimaxai/minimax-m2.7"

# Add an alias to the user's bashrc to map all required variables when running nca-nvidia
RUN echo "alias nca-nvidia='NCA_PROVIDER=openai OPENAI_BASE_URL=\${NCA_PROVIDER_OPENAI_BASE_URL} NCA_MODEL=\${NCA_PROVIDER_OPENAI_MODEL} nca'" >> /home/${NB_USER}/.bashrc

# Install aider
RUN python3 -m pip install --user aider-install && \
    aider-install --yes

USER ${NB_USER}
# Uninstall Cline from the aligned extensions directory
RUN code-server --extensions-dir ${CODE_EXTENSIONSDIR} --uninstall-extension saoudrizwan.claude-dev || true
RUN code-server --extensions-dir ${CODE_EXTENSIONSDIR} --uninstall-extension cline.cline || true

# Install and force-update ALL requested and existing extensions
RUN code-server --extensions-dir ${CODE_EXTENSIONSDIR} \
                --install-extension sst-dev.opencode --force \
                --install-extension alefragnani.project-manager --force \
                --install-extension ms-vscode.live-server --force \
                --install-extension posit.air-vscode --force \
                --install-extension posit.shiny --force \
                --install-extension quarto.quarto --force \
                --install-extension reditorsupport.r --force \
                --install-extension reditorsupport.r-syntax --force

# Clear caches and internal state to prevent "Extension host restart" nag
RUN rm -rf ${CODE_USERDATADIR}/CachedExtensionVSIXs \
           ${CODE_USERDATADIR}/CachedExtensions && \
    rm -f ${CODE_EXTENSIONSDIR}/.obsolete

RUN code-server --extensions-dir ${CODE_EXTENSIONSDIR} --list-extensions
