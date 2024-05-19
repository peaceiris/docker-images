FROM ubuntu:24.04

# cf. https://github.com/Homebrew/brew/blob/91b58f0cc49c51e4b7fdbae80aff8c3820e4cd65/Dockerfile

SHELL ["/bin/bash", "-eo", "pipefail", "-c"]

ENV DEBIAN_FRONTEND=noninteractive

# hadolint ignore=DL3008
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    curl \
    file \
    git \
    locales \
    procps \
    tzdata && \
    apt-get autoclean && \
    apt-get clean && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

RUN sed -i -E 's/^(USERGROUPS_ENAB\s+)yes$/\1no/' /etc/login.defs && \
    localedef -i en_US -f UTF-8 en_US.UTF-8 && \
    useradd --create-home --shell /bin/bash --user-group linuxbrew && \
    echo 'linuxbrew ALL=(ALL) NOPASSWD:ALL' >>/etc/sudoers && \
    su - linuxbrew -c 'mkdir ~/.linuxbrew'
USER linuxbrew
COPY --chown=linuxbrew:linuxbrew . /home/linuxbrew/.linuxbrew/Homebrew
ENV PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:${PATH}" \
    XDG_CACHE_HOME=/home/linuxbrew/.cache
WORKDIR /home/linuxbrew

RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
RUN test -d ~/.linuxbrew && eval "$(~/.linuxbrew/bin/brew shellenv)" && \
    test -d /home/linuxbrew/.linuxbrew && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)" && \
    echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.bashrc

RUN brew update && \
    brew upgrade && \
    brew install cmake && \
    brew cleanup
