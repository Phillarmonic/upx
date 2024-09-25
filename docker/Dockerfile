# Start from Debian Bookworm
FROM debian:bookworm

# Set up architecture and version variables
ARG TARGETARCH
ARG UPX_VERSION=4.2.4

# Update packages and install necessary tools
RUN set -ex && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    zip unzip \
    build-essential \
    apt-transport-https \
    nano curl wget && \
    # Install UPX based on architecture and version
    if [ "$TARGETARCH" = "amd64" ]; then \
        echo "Installing UPX for amd64, version $UPX_VERSION"; \
        curl -L https://github.com/upx/upx/releases/download/v$UPX_VERSION/upx-$UPX_VERSION-amd64_linux.tar.xz -o upx.tar.xz && \
        tar -xf upx.tar.xz && \
        mv upx-$UPX_VERSION-amd64_linux/upx /usr/local/bin/ && \
        rm -rf upx.tar.xz upx-$UPX_VERSION-amd64_linux; \
    elif [ "$TARGETARCH" = "arm64" ]; then \
        echo "Installing UPX for arm64, version $UPX_VERSION"; \
        curl -L https://github.com/upx/upx/releases/download/v$UPX_VERSION/upx-$UPX_VERSION-arm64_linux.tar.xz -o upx.tar.xz && \
        tar -xf upx.tar.xz && \
        mv upx-$UPX_VERSION-arm64_linux/upx /usr/local/bin/ && \
        rm -rf upx.tar.xz upx-$UPX_VERSION-arm64_linux; \
    else \
        echo "Unsupported architecture: $TARGETARCH"; \
        exit 1; \
    fi && \
    # Clean up cache to reduce image size
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create the /app directory
RUN mkdir -p /app

# Set /app as the working directory
WORKDIR /app
