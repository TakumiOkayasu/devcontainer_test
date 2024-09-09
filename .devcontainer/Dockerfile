# ベースイメージにDebian unstable-slimを使用
FROM debian:unstable-slim

# 基本的なパッケージのインストール
RUN apt-get update && apt-get install -y \
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

# ユーザーを作成
RUN groupadd --gid $USER_GID $USERNAME \
	&& useradd --uid $USER_UID --gid $USER_GID -m $USERNAME
RUN echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
	&& chmod 0440 /etc/sudoers.d/$USERNAME

# 必要な作業ディレクトリの作成
WORKDIR /workspace

# コンテナのデフォルトユーザー設定
USER $USERNAME