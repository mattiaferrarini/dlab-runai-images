# Retain your original base image
ARG BASE_IMAGE="ghcr.io/jkminder/dlab-runai-images/base:master"
FROM ${BASE_IMAGE}

# Install uv directly from the official Astral image
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Copy requirements.txt 
COPY generalizations/requirements.txt /tmp/requirements.txt

# Create a virtual environment and install dependencies using uv
RUN uv venv /opt/venv --python 3.12 && \
    . /opt/venv/bin/activate && \
    uv pip install --no-cache -r /tmp/requirements.txt && \
    rm /tmp/requirements.txt

# Activate the virtual environment for all runtime container commands
ENV PATH="/opt/venv/bin:$PATH"

# Install ngrok
RUN curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
	| sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null \
	&& echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
	| sudo tee /etc/apt/sources.list.d/ngrok.list \
	&& sudo apt update \
	&& sudo apt install ngrok