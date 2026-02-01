# 🦞 MyOpenclawBot

基於 [OpenClaw](https://github.com/nickclaw/openclaw) 的 Dockerized 個人 AI 助手。

## 📁 專案結構

```
MyOpenclawBot/
├── docker/                    # Docker 相關文件
│   ├── Dockerfile             # OpenClaw 映像檔 (含 Homebrew)
│   ├── docker-compose.yml     # 正式執行用
│   └── docker-compose.onboard.yml  # 初始化設定用
│
├── openclaw-data/             # OpenClaw 資料 (掛載 → ~/.openclaw)
│   ├── openclaw.json          # 主設定檔 (含敏感 token，git 忽略)
│   ├── agents/                # Agent 設定 + OAuth tokens (git 忽略)
│   ├── credentials/           # Chat 平台認證 (git 忽略)
│   ├── skills/                # 自製 Skills
│   ├── workspace/             # Agent 工作空間 + 記憶
│   └── ...
│
├── .env.example               # 環境變數範例
└── .gitignore
```

---

## 🚀 快速開始

### 前置需求

- Docker & Docker Compose
- Telegram Bot Token（從 [@BotFather](https://t.me/BotFather) 取得）
- OpenAI 帳號（用於 OAuth 登入）

---

## 1️⃣ 生成設定檔 (Onboard)

首次使用需執行 `onboard` 互動式精靈，產生 `openclaw-data/` 下的所有設定。

### 步驟

```bash
# 1. 建立 Docker 映像檔
cd docker
docker-compose -f docker-compose.onboard.yml build

# 2. 執行 onboard 精靈
docker-compose -f docker-compose.onboard.yml run --rm openclaw-cli onboard
```

### Onboard 過程中會設定

| 項目 | 說明 |
|------|------|
| **LLM Provider** | 選擇 `openai-codex` → OAuth 登入（瀏覽器開啟 URL，貼回 redirect URL） |
| **Chat Platform** | 選擇 `Telegram`，輸入 Bot Token |
| **Skills** | 選擇要安裝的技能（可跳過） |

完成後會在 `openclaw-data/` 產生：
- `openclaw.json` - 主設定檔
- `agents/main/agent/auth-profiles.json` - OAuth tokens
- `workspace/*.md` - Agent 工作空間模板

---

## 2️⃣ 啟動服務

設定完成後，使用正式的 docker-compose 啟動 Gateway：

```bash
cd docker

# 啟動 (背景執行)
docker-compose up -d

# 查看狀態
docker-compose ps

# 查看日誌
docker-compose logs -f

# 停止服務
docker-compose down
```

### 驗證啟動成功

日誌應顯示：
```
[gateway] listening on ws://127.0.0.1:18789
[telegram] [default] starting provider
```

---

## 3️⃣ Telegram 用戶授權

Bot 啟動後，**每個用戶首次使用都需要授權**。

### 流程

1. **用戶**：在 Telegram 對 bot 發送 `/start`

2. **Bot 回覆**：
   ```
   OpenClaw: access not configured.
   Your Telegram user id: XXXXXXXXX
   Pairing code: XXXXXX
   Ask the bot owner to approve with:
   openclaw pairing approve telegram <code>
   ```

3. **Bot 管理員**：執行授權指令
   ```bash
   cd docker
   docker-compose exec openclaw-gateway openclaw pairing approve telegram <PAIRING_CODE>
   ```
   
   例如：
   ```bash
   docker-compose exec openclaw-gateway openclaw pairing approve telegram ABC123
   ```

4. **完成**：用戶現在可以正常使用 bot

### 查看待授權請求

```bash
docker-compose exec openclaw-gateway openclaw pairing list --channel telegram
```

---

## 🔧 其他管理指令

```bash
# 進入容器執行 openclaw 指令
docker-compose exec openclaw-gateway openclaw <command>

# 健康檢查
docker-compose exec openclaw-gateway openclaw doctor

# 互動式設定精靈
docker-compose exec openclaw-gateway openclaw configure

# 查看/修改設定
docker-compose exec openclaw-gateway openclaw config get <key>
docker-compose exec openclaw-gateway openclaw config set <key> <value>
```

---

## ⚠️ 安全注意事項

以下檔案包含敏感資訊，**已被 `.gitignore` 排除**：

| 檔案/目錄 | 內容 |
|-----------|------|
| `openclaw-data/openclaw.json` | Gateway token, Telegram bot token |
| `openclaw-data/agents/` | OAuth access/refresh tokens, email |
| `openclaw-data/credentials/` | Chat 平台 session |
| `openclaw-data/identity/` | 裝置身份 |

**請勿將這些檔案提交到 Git！**

---

## 📚 參考文件

- [OpenClaw GitHub](https://github.com/nickclaw/openclaw)
- [Telegram BotFather](https://t.me/BotFather)
