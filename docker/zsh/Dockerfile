# argumentos via cli 
ARG IMG=debian

FROM $IMG
#FROM debian

ARG USER=zsh

USER root

WORKDIR /usr/local/src/zsh

COPY . .

# dependencias do sistema
RUN apt-get update && \
    xargs apt-get install -y < ./requirements-cli && \
    rm -rf /var/lib/apt/lists/*

# cria usuario zsh
RUN if [ "$USER" = "zsh" ]; then useradd -m zsh && echo "zsh:zsh" | chpasswd; else echo ""; fi


# instala o gerenciador de plugins antibody
RUN curl -sfL git.io/antibody | sh -s - -b /usr/local/bin

# USER zsh
USER $USER

# instala o oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# instala o fuzzy-finder
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
RUN ~/.fzf/install

# configura o antibody
RUN echo "\n\n# antibody\n\nsource <(antibody init)\nantibody bundle < ~/.zsh-plugins\n\n" >> ~/.zshrc

# copia o arquivo de plugins
RUN cp ./zsh-plugins ~/.zsh-plugins

# copia configuração do pk10
RUN echo "\n\n# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.\n\n[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh\n\n" >> ~/.zshrc
RUN cp -r ./p10k.zsh ~/.p10k.zsh

# adiciona as configurações do zshrc
RUN echo "\n\n" >> ~/.zshrc
RUN cat ./zshrc >> ~/.zshrc

WORKDIR /home/$USER

CMD [ "/usr/bin/zsh" ]