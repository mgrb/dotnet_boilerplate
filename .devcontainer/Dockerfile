# Description: Dockerfile for the development container
# Author: Marlos Ribeiro
# Using Ubuntu 22.04 as base image for the development container for dotnet 8.0 compatibility.
# see https://learn.microsoft.com/en-us/dotnet/core/install/linux
FROM ubuntu:22.04

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Configure apt and install packages
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y \
    zsh \ 
    git \
    curl \
    dotnet-sdk-8.0

# Install Starship prompt
RUN sh -c "$(curl -sS https://starship.rs/install.sh)" -y -f

# Clean up
RUN apt-get clean && \
    apt-get autoclean  && \
    rm -rf /var/lib/apt/lists/*

# Create a non-root user
RUN groupadd -r dotnet_grp && \
    useradd -m -g dotnet_grp -u 1001 -s /bin/zsh dotnet_usr

USER dotnet_usr

# Install dotnet Entity Framework Core
RUN dotnet tool install --global dotnet-ef
ENV PATH="$PATH:/home/dotnet_usr/.dotnet/tools"
ENV TMPDIR="/home/dotnet_usr/temp/"
RUN mkdir -p $TMPDIR

# coping zsh configuration
COPY .zshrc /home/dotnet_usr/

WORKDIR /dotnet_project

COPY ../ .

CMD ["tail", "-f", "/dev/null"]