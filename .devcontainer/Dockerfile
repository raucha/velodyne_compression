# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.209.6/containers/ubuntu/.devcontainer/base.Dockerfile

ARG ROS_VER=kinetic
FROM ros:${ROS_VER}-ros-base

ARG ROS_VER

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>


ARG USERNAME=vscode
ARG USER_UID=1005
ARG USER_GID=$USER_UID

# # Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# # ********************************************************
# # * Anything else you want to do like clean up goes here *
# # ********************************************************

### Additional packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    wget \
    iputils-ping \
    ros-${ROS_VER}-tf \
    clang-format \
    ros-${ROS_VER}-pcl-ros \
    ros-${ROS_VER}-velodyne \
    ros-${ROS_VER}-cv-bridge \
    libopencv-dev \
    ros-${ROS_VER}-perception \
    && rm -rf /var/lib/apt/lists/*

#######
### zsh
#######
ENV USER ${USERNAME}
ENV HOME /home/${USERNAME}
RUN apt-get update && apt-get install -y --no-install-recommends \
    zsh \
    && rm -rf /var/lib/apt/lists/*
# 環境変数の設定
ENV SHELL /usr/bin/zsh
# ログインシェルを指定(アカウント情報の作成) 
RUN sed -i.bak "s|$HOME:|$HOME:$SHELL|" /etc/passwd
# 以降のRUN/CMDを実行するユーザーasdf
USER $USER
# preztoをクローンしてくる
RUN git clone --recursive \
    https://github.com/sorin-ionescu/prezto.git \
    $HOME/.zprezto

# preztoの設定ファイルのシンボリックリンクを
# ホームディレクトリに張る
RUN ln -s $HOME/.zprezto/runcoms/zlogin    $HOME/.zlogin \
    && ln -s $HOME/.zprezto/runcoms/zlogout   $HOME/.zlogout \
    && ln -s $HOME/.zprezto/runcoms/zpreztorc $HOME/.zpreztorc \
    && ln -s $HOME/.zprezto/runcoms/zprofile  $HOME/.zprofile \
    && ln -s $HOME/.zprezto/runcoms/zshenv    $HOME/.zshenv \
    && ln -s $HOME/.zprezto/runcoms/zshrc     $HOME/.zshrc
# # zplug
# RUN curl -sL --proto-redir -all,https https://raw.githubusercontent.com/zplug/installer/master/installer.zsh | zsh

# fzf
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && ~/.fzf/install
RUN sudo chsh -s /usr/bin/zsh

# edit ~/.zshrc
RUN echo "source /opt/ros/${ROS_VER}/setup.zsh" >> ~/.zshrc
RUN echo "source /workspaces/devel/setup.zsh" >> ~/.zshrc

########
### /zsh
########


# edit ~/.bashrc
RUN echo "source /opt/ros/${ROS_VER}/setup.bash" >> ~/.bashrc
RUN echo "source /workspaces/devel/setup.bash" >> ~/.bashrc

# # [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME