FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8
ENV ASDF_VERSION=v0.15.0

# Install all required packages and setup locale
RUN apt-get update && apt-get install -y \
    curl git zsh bash nano exa bat build-essential \
    libssl-dev ca-certificates gnupg dirmngr unzip wget \
    zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
    llvm libncurses5-dev libncursesw5-dev xz-utils \
    tk-dev libffi-dev liblzma-dev locales \
 && locale-gen en_US.UTF-8 \
 && ln -s /usr/bin/batcat /usr/local/bin/bat || true \
 && apt-get clean

# Install Oh My Zsh and plugins/themes
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-autosuggestions /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting /root/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/MichaelAquilina/zsh-you-should-use.git /root/.oh-my-zsh/custom/plugins/you-should-use && \
    git clone https://github.com/mrxcitement/zsh-bat.git /root/.oh-my-zsh/custom/plugins/zsh-bat && \
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /root/.oh-my-zsh/custom/themes/powerlevel10k

# Install asdf
RUN git clone https://github.com/asdf-vm/asdf.git /root/.asdf --branch ${ASDF_VERSION}

# Configure zsh and asdf
RUN echo 'typeset -g POWERLEVEL9K_INSTANT_PROMPT=off' >> /root/.zshrc && \
    echo '. /root/.asdf/asdf.sh' >> /root/.zshrc && \
    echo '. /root/.asdf/completions/asdf.bash' >> /root/.zshrc && \
    echo 'export PATH="/root/.asdf/bin:/root/.asdf/shims:$PATH"' >> /root/.zshrc && \
    echo 'alias ccat="bat"' >> /root/.zshrc

# Install Node.js and Python via asdf
RUN bash -c ". /root/.asdf/asdf.sh && \
    asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git && \
    asdf install nodejs 24.3.0 && \
    asdf global nodejs 24.3.0 && \
    asdf plugin add python && \
    asdf install python 3.10.0 && \
    asdf global python 3.10.0"

# Clone custom zsh config
RUN git clone https://github.com/Takusei/codeSetting.git /root/codeSetting && \
    echo 'source /root/codeSetting/zshrc' >> /root/.zshrc && \
    echo '[[ -f /root/codeSetting/.p10k.zsh ]] && source /root/codeSetting/.p10k.zsh' >> /root/.zshrc && \
    sed -i 's|/home/jaycen|/root|g' /root/codeSetting/zshrc || true

# Set timezone to JST (Asia/Tokyo)
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    echo "Asia/Tokyo" > /etc/timezone

WORKDIR /workspace
CMD ["zsh"]
