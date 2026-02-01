# 🦞 MyOpenclawBot

基於 [OpenClaw](https://github.com/openclaw/openclaw) 的 Dockerized 個人 AI 助手。

## 📁 專案結構

```
MyOpenclawBot/
├── docker/                    # Docker 相關文件
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── docker-compose.onboard.yml
│
├── openclaw-data/             # OpenClaw 資料 (掛載 → ~/.openclaw)
│   ├── openclaw.json          # 主設定檔
│   ├── agents/                # Agent 設定 + sessions
│   ├── credentials/           # Chat 平台認證 (git 忽略)
│   ├── skills/                # 自製 Skills
│   ├── workspace/             # Agent 工作空間
│   └── ...
│
├── .env.example               # 環境變數範例
├── .env                       # 實際環境變數 (git 忽略)
└── .gitignore
```

## 🚀 快速開始

### 1. 設定環境變數

```bash
cp .env.example .env
# 編輯 .env，填入你的 API Keys
```

### 2. 初始化 (Onboard)

```bash
cd docker
docker compose -f docker-compose.onboard.yml run --rm openclaw-cli onboard
```

### 3. 啟動服務

```bash
docker compose up -d
```

### 4. 存取 Control UI

開啟瀏覽器前往 `http://localhost:18789`

## 📝 設定 Chat 平台

### Telegram

```bash
docker compose -f docker-compose.onboard.yml run --rm openclaw-cli channels add --channel telegram --token "YOUR_BOT_TOKEN"
```

### Discord

```bash
docker compose -f docker-compose.onboard.yml run --rm openclaw-cli channels add --channel discord --token "YOUR_BOT_TOKEN"
```

### WhatsApp (QR Code)

```bash
docker compose -f docker-compose.onboard.yml run --rm openclaw-cli channels login
```

## 🔧 自製 Skills

在 `openclaw-data/skills/` 下建立你的 Skill：

```
openclaw-data/skills/
└── my-skill/
    ├── SKILL.md          # Skill 定義
    ├── scripts/          # 腳本
    └── assets/           # 資源
```

## 📚 參考文件

- [OpenClaw 官方文件](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Docker 安裝指南](https://docs.openclaw.ai/install/docker)
