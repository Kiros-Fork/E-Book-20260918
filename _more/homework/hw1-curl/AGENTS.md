# AGENTS.md

## 專案概況

`rurl`：以 Rust 實作的 curl 替代工具，位於 `hw1/`。非同步 HTTP/HTTPS 客戶端，目前為 **Phase 1**（基礎 CLI + HTTP 請求）。

## 常用指令

```bash
# 建置與檢查
cargo build                          # 編譯
cargo run -- <url>                   # 執行
cargo clippy                         # lint（維持 0 warning）
cargo fmt                            # 格式化（提交前執行）

# 測試
./test.sh                            # 完整自動化測試（需網路，httpbin.org）
BASE=http://localhost:8080 ./test.sh # 覆寫測試伺服器
```

每次修改後務必依序執行：`cargo fmt` → `cargo clippy` → `cargo build` → 相關測試。要求維持 **clippy 與編譯零警告**。

## 專案結構

```
hw1/
├── Cargo.toml          # 依賴：clap, reqwest, tokio, anyhow
├── src/
│   ├── main.rs         # 入口：parse CLI → execute → output
│   ├── cli.rs          # clap 參數定義、HttpMethod、header 解析
│   ├── client.rs       # Response struct、client::execute() 核心請求邏輯
│   └── output.rs       # print_response / write_to_file
├── _doc/               # 版本文件（如 v0.1.md）
└── test.sh             # bash 整合測試
```

## 程式碼慣例

- Stdout 只輸出 response body；日誌/診斷一律到 **stderr**，勿污染 stdout。
- `cli::Cli::infer_method()`：提供 `-d` 且方法為 GET 時自動升級為 POST（仿 curl）。
- 自訂 header 一律經 `cli::parse_headers()` 以 `"Name: Value"` 格式解析，非法格式回報錯誤。
- `HttpMethod::from_str` 大小寫不敏感（`-X GET` 與 `-X get` 皆可）。
- 錯誤處理使用 `anyhow`；`main` 回傳 `anyhow::Result<()>`。
- 新功能請沿用現有 `Cli` / `client::execute` 結構，勿在 main 中塞邏輯。
- 新增 CLI 旗標須同步更新 `test.sh` 與 `_doc/`。

## 測試慣例

- `test.sh` 使用 `pass/fail` 計數，最後以 `[[ $FAIL -eq 0 ]]` 作為 exit code。
- 需要網路的測試依賴 httpbin.org，透過 `check_grep` / `check` / `check_fail` helper 斷言。
- 若 CI/離線環境無法連外網，可設 `BASE` 至本端 mock 伺服器。

## 驗收標準

- `cargo clippy` 與 `cargo build` 零警告
- `./test.sh` 全數 PASS（exit 0）
- 版本異動（`-X`、參數、行為）同步更新 `_doc/` 與 `test.sh`