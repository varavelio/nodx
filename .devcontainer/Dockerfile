FROM denoland/deno:debian-2.9.5

# Set environment variables
ENV \
  DEBIAN_FRONTEND="noninteractive" \
  PIP_BREAK_SYSTEM_PACKAGES=1

RUN set -e && \
  # Update and install system dependencies
  apt-get update -qq && \
  apt-get install -yqq --no-install-recommends \
  ca-certificates wget curl zip unzip p7zip-full tzdata git tree ripgrep \
  python3 python3-pip && \
  # Install tools from npm
  deno install --global --allow-all --allow-scripts --quiet npm:@go-task/cli@latest && \
  # Git config
  git config --global --add safe.directory '*' && \
  # Cleanup
  rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Just to keep the container running
CMD ["sleep", "infinity"]
