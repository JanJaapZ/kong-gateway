# Use the official Ubuntu base image
FROM ubuntu:latest

# Set environment variables to avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update package list and install necessary tools
RUN apt update && apt install -y \
    curl \
    jq \
    git \
    vim \
    htop \
    wget \
    nano \
    unzip \
    tree \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sL https://github.com/kong/deck/releases/download/v1.40.2/deck_1.40.2_linux_amd64.tar.gz -o deck.tar.gz
RUN tar -xf deck.tar.gz -C /tmp
RUN cp /tmp/deck /usr/local/bin/


# Set the default command to start bash
CMD ["tail", "-f", "/dev/null"]