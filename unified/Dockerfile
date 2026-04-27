ARG BASE_IMAGE="ghcr.io/mattiaferrarini/dlab-runai-images/base:master"
FROM ${BASE_IMAGE}

# Copy requirements
COPY unified/requirements.txt /tmp/requirements.txt

# Install ripserplusplus from a patched source tarball for CUDA 12.x toolchains.
RUN apt-get update && \
	apt-get install -y --no-install-recommends cmake && \
	python3 -m pip download --no-deps --no-binary :all: ripserplusplus==1.1.2 -d /tmp && \
	tar -xzf /tmp/ripserplusplus-1.1.2.tar.gz -C /tmp && \
	sed -i '/#include <thrust\/unique.h>/a #include <thrust\/sort.h>' /tmp/ripserplusplus-1.1.2/ripserplusplus/ripser++.cu && \
	uv pip install --no-cache /tmp/ripserplusplus-1.1.2 && \
	rm -rf /tmp/ripserplusplus-1.1.2 /tmp/ripserplusplus-1.1.2.tar.gz /var/lib/apt/lists/*

# Install dependencies directly into the base image's venv
RUN uv pip install --no-cache -r /tmp/requirements.txt && \
    rm /tmp/requirements.txt

# Install ngrok
RUN curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
	| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
	&& echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
	| sudo tee /etc/apt/sources.list.d/ngrok.list \
	&& sudo apt update \
	&& sudo apt install ngrok