# Jupyterのベースイメージを使用
FROM jupyter/base-notebook

# jupyter_server_config.pyを作成または編集
RUN echo "c.MappingKernelManager.cull_idle_timeout = 600  # 10 minutes in seconds" >> ~/.jupyter/jupyter_server_config.py \
    && echo "c.MappingKernelManager.cull_interval = 60  # Check every minute" >> ~/.jupyter/jupyter_server_config.py \
    && echo "c.MappingKernelManager.cull_connected = True  # Even if a browser tab is open" >> ~/.jupyter/jupyter_server_config.py

USER root

# 必要なパッケージのインストール
RUN apt-get update && apt-get install -y build-essential curl git

# RubyのJupyterカーネルをインストール
RUN apt-get install -y ruby ruby-dev
RUN gem install ffi-rzmq
RUN gem install iruby --pre
RUN iruby register --force

# Node.jsのインストール
RUN curl -fsSL https://deb.nodesource.com/setup_current.x | bash - \
    && apt-get install -y nodejs

# TypeScriptのJupyterカーネル（tslab）をインストール
RUN npm install -g tslab \
    && tslab install

# xeus-clingのインストール
RUN conda install -c conda-forge xeus-cling

# Go and GoNB Libraries
ENV GO_VERSION=1.22.0
ENV GONB_VERSION="v0.9.6"
ENV GOROOT=/usr/local/go
ENV GOPATH=/opt/go
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin
RUN mkdir ${GOPATH} && chown ${NB_USER}:users ${GOPATH}
WORKDIR /usr/local
RUN wget --quiet --output-document=- "https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz" | tar -xz \
    && go version

# IJavaのインストール
RUN apt-get update && apt-get install -y default-jdk
RUN wget https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip \
    && unzip ijava-1.3.0.zip -d /tmp/ijava \
    && cd /tmp/ijava \
    && python install.py --sys-prefix

# Rustのインストール
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/home/${NB_USER}/.cargo/bin:${PATH}"

# RustのJupyterカーネル（evcxr_jupyter）をインストール
RUN cargo install evcxr_jupyter \
    && evcxr_jupyter --install

# 所有権の変更
RUN chown -R 1000 /home/${NB_USER}

USER ${NB_USER}

WORKDIR ${HOME}
RUN export GOPROXY=direct && \
    go install "github.com/janpfeifer/gonb@latest" && \
    go install golang.org/x/tools/cmd/goimports@latest && \
    go install golang.org/x/tools/gopls@latest && \
    gonb --install
