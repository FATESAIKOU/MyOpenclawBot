# ========================================
# OpenClaw Dockerized Bot
# ========================================
FROM node:22-bookworm

# 安裝系統依賴
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 安裝 OpenClaw CLI
RUN npm install -g openclaw@latest

# 建立非 root 用戶的 home 目錄結構
RUN mkdir -p /home/node/.openclaw && \
    chown -R node:node /home/node

# 設定環境變數
ENV HOME=/home/node
ENV NODE_ENV=production

# 切換到非 root 用戶
USER node
WORKDIR /home/node

# 預設進入點
ENTRYPOINT ["openclaw"]
CMD ["--help"]
