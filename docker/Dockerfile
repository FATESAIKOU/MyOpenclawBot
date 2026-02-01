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

# 建立 .openclaw 目錄
RUN mkdir -p /root/.openclaw

# 建立 linuxbrew 用戶安裝 Homebrew (Homebrew 不允許 root 安裝)
RUN useradd -m -s /bin/bash linuxbrew && \
    echo "linuxbrew ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 切換到 linuxbrew 用戶安裝 Homebrew
USER linuxbrew
WORKDIR /home/linuxbrew

# 安裝 Homebrew (Linuxbrew)
RUN NONINTERACTIVE=1 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 切回 root
USER root
WORKDIR /root

# 設定 Homebrew 環境變數 (給所有用戶使用)
ENV HOMEBREW_PREFIX="/home/linuxbrew/.linuxbrew"
ENV HOMEBREW_CELLAR="/home/linuxbrew/.linuxbrew/Cellar"
ENV HOMEBREW_REPOSITORY="/home/linuxbrew/.linuxbrew/Homebrew"
ENV PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:${PATH}"

# 驗證 brew 安裝
RUN brew --version

# 安裝 OpenClaw CLI
RUN npm install -g openclaw@latest

# 設定環境變數
ENV HOME=/root
ENV NODE_ENV=production

# 預設進入點
ENTRYPOINT ["openclaw"]
CMD ["--help"]
