# Dockerfile for bloodyAD
# This file builds a self-contained environment for CravateRouge's bloodyAD,
# a tool for Active Directory administration and security assessment.

#--------------------------------------------------------------------------------
# STAGE 1: The Python Environment
# We start with a slim Python base image to keep the final size down.
#--------------------------------------------------------------------------------
FROM python:3.11-slim

# Set the primary working directory inside the container.
WORKDIR /app

# Install dependencies needed for the build process.
# 'wget' is for downloading, and 'curl' is for querying the GitHub API.
RUN apt-get update && \
    apt-get install -y --no-install-recommends wget curl && \
    rm -rf /var/lib/apt/lists/*

#--------------------------------------------------------------------------------
# STAGE 2: Tool Installation and Dependency Management
# This stage dynamically finds the latest release, downloads it, and installs
# its dependencies into an isolated virtual environment.
#--------------------------------------------------------------------------------
# Add a build-time argument to specify the version.
# Defaults to 'latest' to automatically fetch the newest release.
# Can be overridden during build, e.g., --build-arg BLOODYAD_VERSION=v2.1.26
ARG BLOODYAD_VERSION=latest

# Find the desired release tag, download and extract it.
# If 'latest' is specified, we query the GitHub API. Otherwise, we use the provided tag.
RUN if [ "${BLOODYAD_VERSION}" = "latest" ]; then \
        VERSION_TAG=$(curl -s https://api.github.com/repos/CravateRouge/bloodyAD/releases/latest | grep "tag_name" | cut -d '"' -f 4); \
    else \
        VERSION_TAG=${BLOODYAD_VERSION}; \
    fi && \
    wget https://github.com/CravateRouge/bloodyAD/archive/refs/tags/${VERSION_TAG}.tar.gz && \
    tar -xzf ${VERSION_TAG}.tar.gz && \
    rm ${VERSION_TAG}.tar.gz && \
    mv bloodyAD-* bloodyAD

# Set the working directory to be inside the renamed tool's folder.
WORKDIR /app/bloodyAD

# Create a Python virtual environment to keep dependencies neatly isolated.
RUN python3 -m venv .venv

# Install the required Python packages into the venv using its own pip.
# Using --no-cache-dir helps keep the final image size a bit smaller.
RUN .venv/bin/pip install --no-cache-dir -r requirements.txt

#--------------------------------------------------------------------------------
# STAGE 3: Final Configuration
# This stage sets up the container to be run as an executable.
#--------------------------------------------------------------------------------
# Add the virtual environment's bin directory to the system's PATH.
# This ensures that when we call 'python3', it's the one from our isolated venv.
ENV PATH="/app/bloodyAD/.venv/bin:$PATH"

# Set the entrypoint to execute the main bloodyAD script.
# We use an absolute path to make the entrypoint robust and reliable.
ENTRYPOINT ["python3", "/app/bloodyAD/bloodyAD.py"]
