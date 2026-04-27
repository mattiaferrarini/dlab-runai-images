ARG BASE_IMAGE="ghcr.io/mattiaferrarini/dlab-runai-images/base:master"
FROM ${BASE_IMAGE}

# Copy requirements
COPY unified/requirements.txt /tmp/requirements.txt

# ripserplusplus builds from source and invokes a system cmake binary.
RUN apt-get update && \
	apt-get install -y --no-install-recommends cmake && \
	rm -rf /var/lib/apt/lists/*

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