# ベースイメージにDebian unstable-slimを使用
FROM debian:unstable-slim

# 基本的なパッケージのインストールと不要なファイルの削除
RUN apt-get update && apt-get install -y --no-install-recommends \
    bash \
    curl \
    git \
    sudo \
    build-essential \
    ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# ユーザー情報設定
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# ユーザーの作成とsudo権限の付与
RUN groupadd --gid $USER_GID $USERNAME \
	&& useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
	&& echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
	&& chmod 0440 /etc/sudoers.d/$USERNAME

# 作業ディレクトリの設定
WORKDIR /workspace

# gitの補完スクリプトをダウンロードし、シェルに追加
RUN curl -o /usr/share/bash-completion/completions/git-prompt.sh https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh \
	&& curl -o /usr/share/bash-completion/completions/git-completion.bash https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash

# ユーザーのホームディレクトリで~/.bashrcを直接変更
RUN printf "source /usr/share/bash-completion/completions/git-completion.bash\n" >> /home/$USERNAME/.bashrc \
	&& printf "source /usr/share/bash-completion/completions/git-prompt.sh\n" >> /home/$USERNAME/.bashrc \
	&& printf "GIT_PS1_SHOWDIRTYSTATE=true\n" >> /home/$USERNAME/.bashrc \
	&& printf "GIT_PS1_SHOWUNTRACKEDFILES=true\n" >> /home/$USERNAME/.bashrc \
	&& printf "GIT_PS1_SHOWUPSTREAM=auto\n" >> /home/$USERNAME/.bashrc \
	&& printf "export PS1=\"\\[\\033[01;32m\\]\\u@\\h\\[\\033[01;33m\\] \\w \\[\\033[01;31m\\]\$(__git_ps1 \"(%s)\") \\n\\[\\033[01;34m\\]\\\\$ \\[\\033[00m\\]\"\n" >> /home/$USERNAME/.bashrc \
	&& chmod a+x /usr/share/bash-completion/completions/git*

# デフォルトユーザーの設定
USER $USERNAME
