# Cloud Agent base image: lint tooling + Docker (gitleaks CI parity).
# Cursor clones the repo at runtime — do not COPY the project here.
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        gnupg \
        fuse-overlayfs \
        iptables \
        shellcheck \
        sudo \
        zsh \
    && rm -rf /var/lib/apt/lists/*

# Docker CE (fuse-overlayfs storage driver for nested VM kernels)
RUN install -m 0755 -d /etc/apt/keyrings \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
        | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
    && chmod a+r /etc/apt/keyrings/docker.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
        https://download.docker.com/linux/ubuntu noble stable" \
        > /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        containerd.io \
        docker-ce \
        docker-ce-cli \
        docker-compose-plugin \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /etc/docker \
    && printf '%s\n' '{' '  "storage-driver": "fuse-overlayfs"' '}' > /etc/docker/daemon.json \
    && update-alternatives --set iptables /usr/sbin/iptables-legacy \
    && update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy

# Cloud Agent runs as ubuntu with passwordless sudo
RUN if id ubuntu >/dev/null 2>&1; then usermod -aG docker ubuntu || true; fi
