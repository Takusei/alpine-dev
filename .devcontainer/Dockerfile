FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && apt-get install -y \
    curl \
    git \
    zsh \
    bash \
    nano \
    exa \
    bat \
    build-essential \
    libssl-dev \
    ca-certificates \
    gnupg \
    dirmngr \
    python3 \
    unzip \
    wget \
    && apt-get clean

# Symlink batcat if needed (Ubuntu uses batcat instead of bat)
RUN ln -s /usr/bin/batcat /usr/local/bin/bat || true

# Install Oh My Zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# Install Zsh plugins
RUN git clone https://github.com/zsh-users/zsh-autosuggestions /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting /root/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/MichaelAquilina/zsh-you-should-use.git /root/.oh-my-zsh/custom/plugins/you-should-use && \
    git clone https://github.com/mrxcitement/zsh-bat.git /root/.oh-my-zsh/custom/plugins/zsh-bat

# Install Powerlevel10k
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /root/.oh-my-zsh/custom/themes/powerlevel10k

# Install asdf (latest stable recommended)
ENV ASDF_VERSION=v0.15.0
RUN git clone https://github.com/asdf-vm/asdf.git /root/.asdf --branch ${ASDF_VERSION}

# Configure asdf and PATH
RUN echo 'typeset -g POWERLEVEL9K_INSTANT_PROMPT=off' >> /root/.zshrc && \
    echo '. /root/.asdf/asdf.sh' >> /root/.zshrc && \
    echo '. /root/.asdf/completions/asdf.bash' >> /root/.zshrc && \
    echo 'export PATH="/root/.asdf/bin:/root/.asdf/shims:$PATH"' >> /root/.zshrc

# Install Node.js via asdf
RUN bash -c ". /root/.asdf/asdf.sh && \
    asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git && \
    asdf install nodejs latest && \
    asdf global nodejs latest"

# Set alias
RUN echo 'alias ccat="bat"' >> /root/.zshrc

# Clone custom Zsh config & load it
RUN git clone https://github.com/Takusei/codeSetting.git /root/codeSetting && \
    echo 'source /root/codeSetting/zshrc' >> /root/.zshrc && \
    echo '[[ -f /root/codeSetting/.p10k.zsh ]] && source /root/codeSetting/.p10k.zsh' >> /root/.zshrc && \
    sed -i 's|/home/jaycen|/root|g' /root/codeSetting/zshrc || true


# Set working directory
WORKDIR /workspace

# Default shell
CMD ["zsh"]
