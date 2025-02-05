FROM debian

WORKDIR /usr/local/src/zsh

COPY . .

# dependencias do sistema
RUN apt-get update && \
    xargs apt-get install -y < ./requirements-cli && \
    rm -rf /var/lib/apt/lists/*

# cria usuario zsh
RUN useradd -m zsh

# instala o gerenciador de plugins antibody
RUN curl -sfL git.io/antibody | sh -s - -b /usr/local/bin

USER zsh

# instala o oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# instala o fuzzy-finder
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
RUN ~/.fzf/install

# configura o antibody
RUN echo "\n\n# antibody\n\nsource <(antibody init)\nantibody bundle < ~/.zsh-plugins" >> ~/.zshrc

# copia o arquivo de plugins
RUN cp ./zsh-plugins ~/.zsh-plugins

WORKDIR /home/zsh

CMD [ "/usr/bin/zsh" ]