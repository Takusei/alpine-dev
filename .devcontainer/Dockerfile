FROM alpine

# Install required tools
RUN apk update && apk add --no-cache \
    bash \
    zsh \
    git \
    curl \
    nano \
    build-base \
    exa \
    bat

# Install Oh My Zsh (non-interactive)
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# Install Oh My Zsh plugins
RUN git clone https://github.com/zsh-users/zsh-autosuggestions            /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting       /root/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/MichaelAquilina/zsh-you-should-use.git  /root/.oh-my-zsh/custom/plugins/you-should-use && \
    git clone https://github.com/mrxcitement/zsh-bat.git                 /root/.oh-my-zsh/custom/plugins/zsh-bat

# Install Powerlevel10k theme
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /root/.oh-my-zsh/custom/themes/powerlevel10k

# Install asdf version manager
RUN git clone https://github.com/asdf-vm/asdf.git /root/.asdf --branch v0.18.0

# Add asdf to shell profile (zsh example)
RUN echo '. /root/.asdf/asdf.sh' >> /root/.zshrc && \
    echo '. /root/.asdf/completions/asdf.bash' >> /root/.zshrc

# Install example plugin and tool, e.g., Node.js
RUN /bin/bash -c "source /root/.asdf/asdf.sh && \
    asdf plugin-add nodejs https://github.com/asdf-vm/asdf-nodejs.git && \
    asdf install nodejs latest && \
    asdf global nodejs latest"

# Clone your custom zsh config
RUN git clone https://github.com/Takusei/codeSetting.git /root/codeSetting

# Source your custom zsh config
RUN printf '\nsource /root/codeSetting/zshrc\n' >> /root/.zshrc

RUN echo "alias ccat='bat'" >> /root/.zshrc

# Set working directory
WORKDIR /workspace

# Set zsh as the default shell
CMD ["zsh"]
