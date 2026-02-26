ARG BASE_IMAGE="ghcr.io/mattiaferrarini/dlab-runai-images/base:master"
FROM ${BASE_IMAGE}

# Copy requirements
COPY inference/requirements.txt /tmp/requirements.txt

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