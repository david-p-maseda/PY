FROM mambaorg/micromamba:latest

# Set environment variables
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

# Switch to root user for installing system dependencies
USER root

# Install necessary utilities and dependencies, including zsh, chromium, R, and build tools in a single apt-get command
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    curl \
    unzip \
    zsh \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set the working directory to /workspace
WORKDIR /workspace

# Copy the environment.yml file and modules to the working directory
COPY environment.yml ./

# Create the environment from the environment.yml file and specify conda-forge as the default channel
RUN micromamba install --file environment.yml --name base && \
    micromamba clean --all --yes

# Initialize zsh with micromamba
RUN micromamba shell init -s zsh

# Add GitHub to known hosts for SSH
RUN mkdir -p ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts

# Default command to ensure zsh is launched
CMD ["zsh"]