FROM alpine:latest

ENV HOME=/home
ENV XDG_CONFIG_HOME=/home/.config/

ENV LANG="en_US.UTF-8" LANGUAGE="en_US:ja" LC_ALL="en_US.UTF-8"

RUN apk update && apk upgrade
RUN apk add --no-cache \
    build-base \
    curl \
    gcc \
    git \
    neovim \
    nodejs \
    python3-dev \
    py3-pip
    
RUN pip3 install --upgrade pip pynvim

WORKDIR /home

RUN git clone https://github.com/tamago3keran/dotfiles.git
RUN mv dotfiles/.config .
RUN curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
RUN sh ./installer.sh ~/.cache/dein
RUN nvim +:UpdateRemotePlugins +:qa

#Change user mode
ARG UID=1000
RUN adduser -u ${UID} -S docker
RUN addgroup -g ${UID} docker
RUN chown -R docker:docker /home

USER ${UID}

#Make work directory
WORKDIR /home/working

ENTRYPOINT ["nvim"]

#How to use
#Run the following command line:
#docker run --rm -it -u "$(id -u $(whoami)):$(id -g $(whoami))" -v $(pwd):/home/working keith1994/neovim:latest [FILE_NAME]
