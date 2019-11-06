FROM centos:latest

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN yum -y update
RUN yum -y install sudo
RUN yum -y install git

# Add user "web" as non-root user
RUN useradd -ms /bin/bash web

# Set sudoer for "web"
RUN echo 'web ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

ENV NVM_DIR /home/web/.nvm
ENV NODE_VERSION v13.0.1

# Set permission for nvm directory
RUN mkdir -p $NVM_DIR && chown -R web:1000 $NVM_DIR

# Switch to user "web" from now
USER web

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash

# nvm
RUN echo 'export NVM_DIR="$HOME/.nvm"'                                       >> "$HOME/.bashrc"
RUN echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> "$HOME/.bashrc"
RUN echo '[ -s "$NVM_DIR/bash_completion" ] && . "$NVM_DIR/bash_completion" # This loads nvm bash_completion' >> "$HOME/.bashrc"

# nodejs and tools
RUN bash -c 'source $HOME/.nvm/nvm.sh \
    && nvm --version \
    && nvm install $NODE_VERSION \
    && npm install http-server -g'

# Set WORKDIR to web directory
WORKDIR /home/web

ENTRYPOINT ["/bin/bash"]