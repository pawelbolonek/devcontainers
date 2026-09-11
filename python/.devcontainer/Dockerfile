# debian 13
FROM debian:trixie-slim

ARG ZSH_IN_DOCKER_VERSION=v1.2.1
ARG ZSH_AUTOSUGGESTIONS_VERSION=v0.7.1
ARG ZSH_FAST_SYNTAX_HIGHLIGHTING_VERSION=v1.56
ARG ZSH_AUTOCOMPLETE=25.03.19
#https://www.python.org/downloads/release/python-3147/
ARG PYTHON_VERSION=3.14.7
#https://github.com/astral-sh/uv/releases
ARG UV_VERSION=0.12.11
#https://github.com/nvm-sh/nvm
ARG NVM_VERION=v0.40.7
#https://nodejs.org/en/about/previous-releases
ARG NODE_VERSION=26.8.1
#https://www.npmjs.com/package/@anthropic-ai/claude-code?activeTab=versions
ARG CLAUDE_CODE_VERSION=2.1.266
https://github.com/Fission-AI/OpenSpec/releases
ARG OPEN_SPEC_VERSION=v1.13.0
USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
  ca-certificates \
  aptitude \
  iproute2 \
  iputils-ping \
  less \
  git \
  lazygit \
  git-delta \
  sudo \
  fzf \
  zsh \
  unzip \
  aggregate \
  jq \
  vim \
  mcedit \
  wget \
  curl \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

## COMMON SHELL CONFIGURATION
# trixie comes with zsh 5.9
# Default powerline10k theme
#interesting tutorial https://gist.github.com/n1snt/2cccc8aa5f7b645a7628d3512c70deb6
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/${ZSH_IN_DOCKER_VERSION}/zsh-in-docker.sh)" -- \
  -p git \
  -p fzf \
  -p z \
  -p zsh-autosuggestions \
  -p fast-syntax-highlighting \
  -p zsh-autocomplete \
  -a "HISTFILE=/persistedhistory/.zsh_history" \
  -x
SHELL ["/usr/bin/zsh", "-i", "-c"]
RUN git clone --branch ${ZSH_AUTOSUGGESTIONS_VERSION} --depth 1 https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH}/custom/plugins/zsh-autosuggestions
RUN git clone --branch ${ZSH_FAST_SYNTAX_HIGHLIGHTING_VERSION} --depth 1 https://github.com/zdharma-continuum/fast-syntax-highlighting.git ${ZSH}/custom/plugins/fast-syntax-highlighting
RUN git clone --branch ${ZSH_AUTOCOMPLETE} --depth 1 https://github.com/marlonrichert/zsh-autocomplete.git ${ZSH}/custom/plugins/zsh-autocomplete

RUN usermod --shell /usr/bin/zsh root

# PYTHON DEVELOPMENT
RUN bash -c "$(curl --fail --show-error --silent --location https://astral.sh/uv/${UV_VERSION}/install.sh)" \
    && source $HOME/.local/bin/env

RUN uv python install ${PYTHON_VERSION} --default
ENV PATH="$PATH:/root/.local/bin"

RUN bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/nvm-sh/nvm/${NVM_VERION}/install.sh)"  \
    && source "$HOME/.nvm/nvm.sh"
RUN nvm install ${NODE_VERSION}

RUN npm install -g @anthropic-ai/claude-code@${CLAUDE_CODE_VERSION}
RUN npm install -g @fission-ai/openspec@${OPEN_SPEC_VERSION}
