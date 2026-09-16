# 從 Docker 到 Kubernetes：Rust 與多樣化 Web 架構（WebSocket / SSR / MPA）實戰

## 第 1 部分：多樣化前端與通訊協定的 Docker 容器化

- 一、雲原生世代與容器技術革命
   - [1.1 虛擬機 vs 容器：從 Namespace 與 Cgroups 理解隔離本質](1.1.md)
   - [1.2 技術棧選型：Rust 高併發核心 × 多樣化 Web 前端](1.2.md)
   - [1.3 開發環境：Docker Desktop、Podman 與 CLI 工具鏈](1.3.md)

- 二、Rust 後端服務的極致容器化（Multi-Stage Builds）
   - [2.1 Rust 編譯特性與容器化挑戰](2.1.md)
   - [2.2 多階段構建到 Alpine 與 Distroless](2.2.md)
   - [2.3 musl 靜態編譯：把 Rust 壓進 20 MB 以內](2.3.md)
   - [2.4 cargo-chef 加速 CI/CD：三段式 Dockerfile](2.4.md)

- 三、多樣化 Web 架構的容器化策略
   - [3.1 SPA 模式：React + Vite 建置、Nginx 託管](3.1.md)
   - [3.2 SSR / Fullstack 模式：Next.js standalone、靜態抽離與記憶體考量](3.2.md)
   - [3.3 MPA / 樣板引擎：Rust + Tera + HTMX，一個二進位檔即全站](3.3.md)
   - [3.4 環境變數注入：Build-time vs Run-time，前端三模式全解](3.4.md)

- 四、多容器開發與本地編排（Docker Compose）
   - [4.1 Compose 語法：Services、Networks、Volumes 一次搞定](4.1.md)
   - [4.2 本地全棧：Rust Server + 雙前端 + PostgreSQL + Redis](4.2.md)
   - [4.3 容器間通訊、連接池與 Healthcheck：從能跑到穩跑](4.3.md)
   - [4.4 Compose Watch：Rust 與前端 Hot Reload，一存即同步](4.4.md)

## 第 2 部分：長連線與混合架構的 12-Factor 雲原生改造

- 五、WebSocket 與長連線雲原生挑戰
   - [5.1 短連線與長連線：在 K8s 裡完全是兩種生物](5.1.md)
   - [5.2 WebSocket 狀態抽離與廣播：從單機記憶體到跨 Pod 同步](5.2.md)
   - [5.3 負載均衡與 Sticky Sessions：WS 為什麼需要粘性](5.3.md)

- 六、全棧應用的無狀態化與 Session 抽離
   - [6.1 SPA 無狀態化：把 Session 趕出伺服器記憶體](6.1.md)
   - [6.2 SSR / MPA 的伺服器端 Session：Redis 分散式 Session Store](6.2.md)
   - [6.3 檔案上傳解耦：Presigned URL 直傳物件儲存](6.3.md)

- 七、長連線環境下的生命週期管理與健康檢測
   - [7.1 WS 優雅停機：SIGTERM 來時體面說再見](7.1.md)
   - [7.2 健康檢查設計：別讓長連線害死你的 Pod](7.2.md)
   - [7.3 雲原生可觀測性：讓每條 WS 都有跡可循](7.3.md)

## 第 3 部分：Kubernetes 編排與長連線流量治理

- 八、單機 Kubernetes 開發環境：Kind
   - [8.1 Kind 的 DinD 原理：把 Kubernetes 裝進 Docker](8.1.md)
   - [8.2 Kind YAML 實戰：1 Control-Plane + 2 Workers](8.2.md)
   - [8.3 kind load docker-image：把本機映像送進叢集](8.3.md)
   - [8.4 Kind Port Mapping 與 NGINX Ingress 安裝](8.4.md)

- 九、K8s 核心資源管理與部署
   - [9.1 Deployment 部署三種前端架構：Rust Backend、SSR、SPA](9.1.md)
   - [9.2 ConfigMap 與 Secret：把 run-time 配置動態注入](9.2.md)
   - [9.3 StatefulSet + PV/PVC：託管 PostgreSQL 與 Redis](9.3.md)

- 十、K8s 網絡、長連線流量路由與 Ingress 實戰
   - [10.1 Service 模型：ClusterIP、NodePort、LoadBalancer](10.1.md)
   - [10.2 Ingress 超時與 WebSocket 支持（核心節）](10.2.md)
   - [10.3 CORS、HTTP/2 與 HTTP/3 QUIC 實踐](10.3.md)

- 十一、長連線架構下的資源調度與自動擴縮容
   - [11.1 Requests / Limits 與 QoS：別讓 SSR 吃掉整台 Worker](11.1.md)
   - [11.2 WebSocket 擴縮容瓶頸：基於 Active 連線數的 Custom Metrics HPA](11.2.md)
   - [11.3 Rolling Update 與長連線平滑遷移](11.3.md)

## 第 4 部分：高級主題與生產環境落地

- 十二、GitOps 與自動化部署流水線（CI/CD）
   - [12.1 GitHub Actions 自動建置 Rust 與多前端鏡像並推送至 Registry](12.1.md)
   - [12.2 Helm 模組化打包全端：Frontend + Backend + Redis + DB](12.2.md)
   - [12.3 ArgoCD GitOps 自動同步](12.3.md)

- 十三、服務網格與長連線可觀測性
   - [13.1 Istio / Cilium 在微服務 + 長連線下的選型](13.1.md)
   - [13.2 mTLS 加密與 WebSocket 流量治理：熔斷與限流](13.2.md)
   - [13.3 OpenTelemetry 全端追蹤：從前端 HTTP/WS 觸發點到 Rust DB Query](13.3.md)

- 十四、AI / ML 雲原生算力擴充與長連線 Streaming
   - [14.1 在 Kubernetes 上部署 vLLM 推論服務](14.1.md)
   - [14.2 前端 + Rust Gateway SSE 串流 + LLM Pod 架構](14.2.md)

## 附錄

- [序言：為什麼是 WebSocket + SSR/MPA](0.0.md)
- [本書大綱與寫作策略](outline.md)
