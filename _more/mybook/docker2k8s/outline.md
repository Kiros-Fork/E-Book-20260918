# 《從 Docker 到 Kubernetes：Rust 與多樣化 Web 架構（WebSocket / SSR / MPA）實戰》大綱

## 書籍定位

面向**中高階開發者、DevOps 與架構師**的實戰書：以前端多樣架構（WebSocket 長連線、SSR、MPA/SPA）為需求牽引，以 Rust 為後端主線，從 Docker 一路做到 Kubernetes 上的 GitOps、服務網格與 LLM 推論。

## 目標讀者

- 有 Docker 或後端基礎，想系統化上 K8s 的中高階工程師
- 負責全端架構選型的架構師（SSR vs SPA vs MPA、WS vs SSE）
- 要把 Rust 後端與長連線服務維運起來的 DevOps / SRE

## 稀缺性：為什麼是 WS 長連線 + SSR/MPA

市面 Docker/K8s 書多以無狀態 REST + SPA 為例，長連線與 SSR 常被略過。但實務痛點恰在於此：WS 閒置被網格砍斷、SSR 首屏與快取難調、MPA/SPA 部署物不同。本書以這三者為主線，每章都回答「容器化與編排時有何不同」。

## 結構規劃：四部分

| 部分 | 章 | 標題 | 核心內容 |
|------|----|------|----------|
| 一、Docker 基礎與全端容器化 | 1–3 | Docker 上手、多階段建置、Compose | Rust cargo-chef 快取、SSR/SPA 映像檔差異、Compose 一鍵全端 |
| 二、Rust 與多樣化 Web 架構 | 4–7 | Axum、WebSocket、SSR、MPA/SPA | WS 心跳廣播壓測、SSR Hydration 邊界、MPA/SPA 選型 |
| 三、上 Kubernetes | 8–11 | 工作負載、設定狀態、擴展、除錯 | Ingress 三路路由、Stateful DB/Redis、WS 連線數 HPA |
| 四、GitOps、網格、可觀測與 AI | 12–14 | GHA、Helm、ArgoCD、Istio/Cilium、OTel、vLLM/SSE | matrix 建鏡像、GitOps 同步、mTLS+熔斷、trace 經 WS 傳播、SSE 串流 LLM |

## 寫作策略：SPA 入門 → WS/SSR 進階

1. **SPA 入門**：先用最熟悉的 SPA + REST 打通 Docker→K8s 全鏈路，建立信心。
2. **WS/SSR 進階**：再引入長連線（心跳、廣播、熔斷）與 SSR（首屏、快取），對照 SPA 說明差異。
3. **SRE 收束**：最後以 GitOps、網格、可觀測、LLM 串流收尾，每章綁一個可跑的 YAML/程式骨架。

## 寫作風格

1. 每節 `# X.Y` 開頭、`##` 分節，以問題場景開場
2. 表格做對照（選型、參數、指令），mermaid 畫架構與流程，程式碼給可跑骨架
3. 每節結尾「本章小結 + 想一想 3 題」（README、outline、序言除外）
4. 中英術語對照，首次出現即定義；YAML 與 Rust 片段保持最小可運行
