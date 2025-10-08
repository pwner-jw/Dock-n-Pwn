# Use a slim, lightweight base image, which is great for single-purpose tool images.
FROM python:3-slim

# Set a general working directory for our tools.
WORKDIR /app

# Install git, which is not included in the slim base image.
# We update, install, and then clean up in a single RUN layer to keep the image small.
RUN apt-get update && apt-get install -y git --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Clone the specific tool into its own subdirectory for better organization.
# This prevents cluttering the /app directory if you were to add more tools.
RUN git clone https://github.com/ShutdownRepo/targetedKerberoast.git targetedKerberoast

# Now, change the working directory to be inside the tool's folder.
WORKDIR /app/targetedKerberoast

# Create a Python virtual environment. This is the core of your request.
# The .venv will be created inside /app/targetedKerberoast.
RUN python3 -m venv .venv

# Install the tool's dependencies into the virtual environment.
# We use the pip from the .venv to ensure dependencies are isolated.
# Using --no-cache-dir helps keep the final image size smaller.
RUN .venv/bin/pip install --no-cache-dir -r requirements.txt

# Add the virtual environment's bin directory to the system's PATH.
# This makes our ENTRYPOINT cleaner, as the system will now prefer the
# python executable from our .venv.
ENV PATH="/app/targetedKerberoast/.venv/bin:$PATH"

# This is the magic part! ✨
# The ENTRYPOINT now calls the correct script. Because of the ENV PATH command above,
# 'python3' will automatically resolve to the one inside our .venv.
ENTRYPOINT ["python3", "/app/targetedKerberoast/targetedKerberoast.py"]
