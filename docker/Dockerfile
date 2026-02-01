# ========================================
# OpenClaw Dockerized Bot
# ========================================
FROM node:22-bookworm

# 安裝系統依賴 (包含 Homebrew 所需)
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    build-essential \
    procps \
    file \
    sudo \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 給 node 用戶 sudo 權限 (Homebrew 安裝需要)
RUN echo "node ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 建立 home 目錄
RUN mkdir -p /home/node/.openclaw && \
    chown -R node:node /home/node

# 切換到 node 用戶安裝 Homebrew
USER node
WORKDIR /home/node

# 安裝 Homebrew (Linuxbrew)
RUN NONINTERACTIVE=1 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 設定 Homebrew 環境變數
ENV HOMEBREW_PREFIX="/home/linuxbrew/.linuxbrew"
ENV HOMEBREW_CELLAR="/home/linuxbrew/.linuxbrew/Cellar"
ENV HOMEBREW_REPOSITORY="/home/linuxbrew/.linuxbrew/Homebrew"
ENV PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:${PATH}"

# 驗證 brew 安裝
RUN brew --version

# 安裝 OpenClaw CLI (用 sudo 因為全域安裝需要 root)
RUN sudo npm install -g openclaw@latest

# 設定環境變數
ENV HOME=/home/node
ENV NODE_ENV=production

# 預設進入點
ENTRYPOINT ["openclaw"]
CMD ["--help"]
