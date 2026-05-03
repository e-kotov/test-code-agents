FROM rocker/r-ver:latest

# Set environment variables for non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive
ENV NCPUS=-1
ENV PIP_BREAK_SYSTEM_PACKAGES=1
ENV RENV_CONFIG_REPOS_OVERRIDE="https://p3m.dev/cran/__linux__/noble/latest"


# 1. Create a non-root user (vscode) early and install essential tools including sudo
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo \
    curl \
    git \
    wget \
    ca-certificates \
    xz-utils \
    && groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# 2. Combine all system prerequisites for Tidyverse, Geospatial, and httpgd into a single layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    cmake \
    default-libmysqlclient-dev \
    gdal-bin \
    gsfonts \
    htop \
    lbzip2 \
    libcairo2-dev \
    libcurl4-openssl-dev \
    libdbus-1-dev \
    libfftw3-dev \
    libfontconfig1-dev \
    libfreetype6-dev \
    libfribidi-dev \
    libgdal-dev \
    libgeos-dev \
    libgit2-dev \
    libgl1-mesa-dev \
    libglpk-dev \
    libglu1-mesa-dev \
    libgsl-dev \
    libharfbuzz-dev \
    libhdf4-alt-dev \
    libhdf5-dev \
    libicu-dev \
    libjpeg-dev \
    libjq-dev \
    libleptonica-dev \
    libmagick++-dev \
    libnetcdf-dev \
    libnode-dev \
    libomp-dev \
    libpng-dev \
    libpoppler-cpp-dev \
    libpq-dev \
    libproj-dev \
    libprotobuf-dev \
    libsasl2-dev \
    libsecret-1-dev \
    libsodium-dev \
    libsqlite3-dev \
    libssh2-1-dev \
    libssl-dev \
    libtesseract-dev \
    libtiff-dev \
    libudunits2-dev \
    libuv1-dev \
    libwebp-dev \
    libxml2-dev \
    libxtst6 \
    make \
    netcdf-bin \
    openssh-client \
    poppler-data \
    protobuf-compiler \
    python3-pip \
    python3-venv \
    smem \
    sqlite3 \
    tesseract-ocr-eng \
    tk-dev \
    unixodbc-dev \
    zlib1g-dev \
    && curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g \
        opencode-ai \
        @google/gemini-cli \
        @mariozechner/pi-coding-agent \
        @openai/codex \
        codeburn \
    && npm cache clean --force \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# 3. Configure Posit Public Package Manager for binary packages
RUN . /etc/os-release && \
    echo "options(repos = c(CRAN = \"https://p3m.dev/cran/__linux__/${VERSION_CODENAME}/latest\"))" >> /usr/local/lib/R/etc/Rprofile.site

# 4. Install Rust toolchain for modern R packages
ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --no-modify-path && \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME

# 5. Install all R packages (Tidyverse, Geospatial, Tooling) in a single layer to minimize cache bloat
RUN install2.r --error --skipmissing --skipinstalled -n "$NCPUS" \
    # Core Tooling & IDE
    languageserver \
    httpgd \
    jsonlite \
    rlang \
    devtools \
    rmarkdown \
    BiocManager \
    # Tidyverse & Data
    tidyverse \
    vroom \
    gert \
    arrow \
    dbplyr \
    DBI \
    dtplyr \
    duckdb \
    RSQLite \
    fst \
    # Geospatial
    RColorBrewer \
    RNetCDF \
    classInt \
    deldir \
    gstat \
    hdf5r \
    lidR \
    mapdata \
    mapview \
    ncdf4 \
    proj4 \
    raster \
    rlas \
    sf \
    sfarrow \
    sp \
    spacetime \
    spatstat \
    spatialreg \
    spdep \
    stars \
    terra \
    tidync \
    tmap \
    geoR \
    geosphere \
    && rm -rf /tmp/downloaded_packages

# 6. Switch to the non-root user to install Python and environment managers securely
USER $USERNAME
WORKDIR /home/$USERNAME

# Install all CLI tools in a single layer to minimize metadata and clean caches
RUN curl -fsSL -o /tmp/miniforge.sh https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh && \
    bash /tmp/miniforge.sh -b -p /home/$USERNAME/miniforge3 && \
    rm /tmp/miniforge.sh && \
    /home/$USERNAME/miniforge3/bin/conda init bash && \
    # Install uv (defaults to ~/.local/bin)
    curl -LsSf https://astral.sh/uv/install.sh | sh && \
    export PATH="/home/$USERNAME/.local/bin:$PATH" && \
    # Install Claude CLI
    curl -fsSL https://claude.ai/install.sh | CI=1 bash && \
    # Install NCA CLI
    (curl -fsSL https://nca-cli.com/install | bash || echo "NCA CLI not available for this architecture, skipping.") && \
    # Install pixi (defaults to ~/.pixi/bin)
    curl -fsSL https://pixi.sh/install.sh | bash && \
    # Install arf console (defaults to ~/.cargo/bin)
    curl --proto '=https' --tlsv1.2 -LsSf https://github.com/eitsupi/arf/releases/latest/download/arf-console-installer.sh | sh && \
    # Install aider
    uv tool install aider-chat --python python3 && \
    # Cleanup user caches
    rm -rf /home/$USERNAME/.cache/uv /home/$USERNAME/.cache/pip /home/$USERNAME/.npm/_cacache


# NVIDIA NIM (OpenAI-compatible) configuration for nca
ENV NCA_PROVIDER_OPENAI_BASE_URL="https://integrate.api.nvidia.com/v1"
ENV NCA_PROVIDER_OPENAI_MODEL="minimaxai/minimax-m2.7"

# Add an alias to the user's bashrc for nca-nvidia
RUN echo "alias nca-nvidia='NCA_PROVIDER=openai OPENAI_BASE_URL=\${NCA_PROVIDER_OPENAI_BASE_URL} NCA_MODEL=\${NCA_PROVIDER_OPENAI_MODEL} nca'" >> /home/$USERNAME/.bashrc

# Ensure all tools are in the PATH for the user session
ENV PATH="/home/$USERNAME/.local/bin:/home/$USERNAME/miniforge3/bin:/home/$USERNAME/.pixi/bin:/home/$USERNAME/.cargo/bin:${PATH}"



# Set the default workspace directory
WORKDIR /workspace
