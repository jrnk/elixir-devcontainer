# Elixir Development Container
# Based on official Elixir image from hexpm
#
ARG ELIXIR_VERSION=1.18.3
ARG OTP_VERSION=27.3.4
ARG DEBIAN_VERSION=bookworm-20250428-slim
ARG NODE_VERSION=20

FROM docker.io/hexpm/elixir:${ELIXIR_VERSION}-erlang-${OTP_VERSION}-debian-${DEBIAN_VERSION}

# Install development dependencies
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    curl \
    wget \
    inotify-tools \
    postgresql-client \
    sudo \
    vim \
    locales \
    ca-certificates \
    gnupg \
  && rm -rf /var/lib/apt/lists/*

# Install Node.js for Phoenix assets
ARG NODE_VERSION
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
  && apt-get install -y nodejs \
  && rm -rf /var/lib/apt/lists/*

# Set the locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen \
  && locale-gen

ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Create non-root user for development
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
  && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
  && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
  && chmod 0440 /etc/sudoers.d/$USERNAME

# Set working directory
WORKDIR /workspace

# Switch to non-root user
USER $USERNAME

# Install Hex and Rebar for the user
RUN mix local.hex --force \
  && mix local.rebar --force

# Install Phoenix if needed
RUN mix archive.install hex phx_new --force

# Set development environment
ENV MIX_ENV=dev

# Expose Phoenix default port
EXPOSE 4000

# Keep container running
CMD ["tail", "-f", "/dev/null"]
