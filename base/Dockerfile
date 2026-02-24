ARG BASE_IMAGE="nvidia/cuda:12.2.2-devel-ubuntu22.04"

FROM ${BASE_IMAGE}

USER root
RUN apt-get update && \
   apt-get install -y  --no-install-recommends  \
    curl \
    gcc \
    git \
    gosu \
    git-lfs \
    htop \
    libgl1 \
    libglib2.0-0 \
    ncdu \
    tree \
    openssh-client \
    openssh-server \
    psmisc \
    rsync \
    screen \
    sudo \
    tmux \
    unzip \
    vim \
    ldap-utils \
    nvtop \
    wget && \
    wget -q  https://github.com/justjanne/powerline-go/releases/download/v1.24/powerline-go-linux-"$(dpkg --print-architecture)" -O /usr/local/bin/powerline-shell && \
    chmod a+x /usr/local/bin/powerline-shell

# Install uv directly from the official image
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Install Python 3.11 and create the global virtual environment
RUN uv python install 3.11 && \
    uv venv /opt/venv --python 3.11

# Make the venv writable for users so they can install their own packages at runtime
RUN chmod -R 777 /opt/venv

# 4. Set PATH so the venv is the default Python for all non-interactive commands
ENV PATH="/opt/venv/bin:$PATH"

# setup ssh
RUN ssh-keygen -A
EXPOSE 22

COPY base/user-entrypoint.sh /tmp/user-entrypoint.sh
COPY base/.bashrc /tmp/.bashrc
RUN chmod +x /tmp/user-entrypoint.sh

ENTRYPOINT ["/tmp/user-entrypoint.sh"]
CMD ["/bin/bash"]