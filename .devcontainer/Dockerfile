FROM mambaorg/micromamba:latest

# Set environment variables
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

# Switch to root user for installing system dependencies
USER root

# Install necessary utilities and dependencies, including zsh and chromium
RUN apt-get update && \
    apt-get install -y \
    git \
    curl \
    unzip \
    chromium \
    chromium-driver \
    zsh && \
    apt-get clean

# Install AWS CLI
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf awscliv2.zip aws

# Set the working directory to /app
WORKDIR /app

# Copy the environment.yml file and modules to the working directory
COPY environment.yml ./
COPY modules ./modules

# Create the environment from the environment.yml file and specify conda-forge as the default channel
RUN micromamba install --file environment.yml --name base && \
    micromamba clean --all --yes

# Initialize zsh with micromamba
RUN micromamba shell init -s zsh -p /opt/conda

# Set SHELL to zsh for all subsequent commands
SHELL ["/bin/zsh", "-c"]

# Add GitHub to known hosts for SSH
RUN mkdir -p ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts

# Set Pandas display options for IPython
RUN mkdir -p /root/.ipython/profile_default/startup/ && \
    echo "import pandas as pd; pd.set_option('display.min_rows', 4); pd.set_option('display.max_rows', 7)" > /root/.ipython/profile_default/startup/pandas_startup.py

# Default command to ensure zsh is launched
CMD ["zsh"]