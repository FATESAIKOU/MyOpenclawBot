## 目標

我需要你幫我建立一個內建 openclaw 的 Dockerfile

## 背景

- 我剛接觸 openclaw
- 對於 openclaw 那個可以介接多語言模型，多個Chat平台，記憶外部化，背景執行 特別有感
- 但 openclaw 本身似乎沒有提供一個完整的 Dockerize 的封裝

## 執行步驟

1. 根據 openclaw 官方文件，規劃整體專案結構
    - 外掛資料的資料夾怎麼擺(LLM Keys, Chat Keys(or session), Memory, Additional Manual Skill, ... 還有其他最好外掛的東西的話都請你提案)
    - openclaw container 的 Dockerfile 位置
    - openclaw container 的 Docker-Compose file 位置 & 內部的掛載設定
2. 根據規劃的專案結構建立資料夾, .gitignore, .gitkeep 等
3. 撰寫 Dockerfile / Docker-compose for configuration - 用以配置 LLM Keys / Chat Keys 或各種(其實就是跑 openclaw onboard 然後把結果好好吐到 mount 的外部資料夾)
4. 使用 2 撰寫的東西嘗試讓使用者初始化 context
5. 撰寫 Dockerfile / Docker-compose for execution - 用以真實上線 提供服務

※ 注意 你每一步都需要經過我 Review
※ 你不得同時進行多步
※ 為避免問題 所有步驟你能測都要進行測試 聽說至少可以透過 openclaw 指令假裝發起 chat

## 限制條件 & 需求

- 希望可以把 「LLM的設定&Key」、「Chat平台的Session設定」、「記憶」都做到外掛到Container外
- Container 內部直接包含 「openclaw 本體」、「openclaw 幾乎全權限的設定(root)」、「、可選的全部預設 Skill」
- 允許「自製的Skill」集合外掛給Container
