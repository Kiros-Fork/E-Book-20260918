prompt=>context=>harness=>loop=>graph engineering 分別是什麼？

這四個概念代表了當代 AI / LLM 應用開發 中不同層次的設計與架構思維，從單一提示詞逐步延伸到複雜的系統級工作流：

概念	核心定義	角色與譬如	典型應用場景
Prompt (提示詞)	交付給 LLM 的輸入指令與上下文資料	指令內容 (如「食譜文字」)	撰寫文章、問題解答、單次翻譯
Context (上下文)	LLM 在推理時能參照的總體資訊空間	記憶與背景資料庫 (如「整本食譜與廚房庫存」)	RAG 檢索增強、多輪對話歷史管理
Harness (測試/執行框架)	包裹並測試 Prompt/模型運行的基礎設施環境	試驗台與控制儀表板 (如「測試廚房」)	Prompt 評測 (Eval)、自動化測試、系統監控
Loop (循環/代理迴圈)	讓模型能自省、修正或重複執行的迭代機制	動態修正機制 (如「試吃後調整調味料」)	ReAct 代理人、自動除錯、多輪自我校正
Graph Engineering (圖形工程)	將 LLM 系統拆解為有向圖 (Graph) 的節點與邊進行複雜流程控制	整體工廠流水線圖 (如「整間餐廳的標準作業流程」)	LangGraph、多 Agent 協作系統、複雜分級審查系統
彼此之間的層級與遞進關係

Prompt & Context 是基礎：你將 Context 送入模型，而 Prompt 決定模型如何處理這些 Context。

Loop 引入動態能力：單次 Prompt 回應可能不足，Loop 讓 LLM 能根據結果不斷迭代修正。

Graph Engineering 提供結構化管理：當系統包含多個 Prompt、Loop 與工具呼叫時，Graph 將這些組件連成明確的状态機 (State Machine) 與條件分流。

Harness 則是品質把關：在整個開發過程中，Harness 用來評測 Prompt、Loop 與 Graph 的整體表現與穩定度。

請說明如何建立 LLM 應用中的 Harness (測試與評測框架) 與常見指標。

建立 LLM 應用的 Harness（測試與評測框架） 是將「隨機且不確定的 AI 輸出」轉化為「可預測、可量化產品」的核心工程。它的主要任務是自動化地執行測試資料集、記錄模型輸出、透過多元指標（Metrics）評分，並在 Prompt 或模型版本更動時進行迴歸測試 (Regression Testing)。

建立 Harness 的五大核心步驟

建構高質量的測試資料集 (Evaluation Dataset)

收集真實用戶查詢、邊界條件 (Edge Cases) 與對抗性範例 (Adversarial Prompts)。

包含輸入（Input/Context）與對應的黃金標準答案（Ground Truth/Golden Dataset）。

選擇與組合評測指標 (Evaluation Metrics Setup)

根據應用類型（如檢索問答 RAG、摘要、 Agent 執行）配置相應的自動化評分函數。

建構自動化執行與平行測試管線 (Test Pipeline Executor)

自動批量（Batch processing）呼叫 Target Prompt / Model，並記錄輸入、輸出、Token 消耗與延遲時間。

實施 LLM-as-a-Judge 與傳統評測機制

利用功能強大的 LLM（如 GPT-4o 或 Claude 3.5 Sonnet）作為裁判，對語意正確性、安全性或格式進行高層級打分。

整合 CI/CD 與迴歸分析診斷 (Regression & Benchmarking)

將 Harness 整合至 GitHub Actions 或 GitLab CI，當 Prompt 修改時自動運行，對比變更前後的得分差異（Diff）。

常見的 LLM 評測指標 (Evaluation Metrics)

分類	指標名稱	說明與適用場景
RAG 專用指標	Faithfulness (忠實度)	評估生成的回答是否完全基於檢索出的 Context，避免幻覺。
Answer Relevance (回答相關性)	評估生成的回答是否精準回應了用戶的原始提問。
Context Precision / Recall	評估檢索模組找出的 context 是否精確且覆蓋關鍵答案資訊。
生成品質與語意	Semantic Similarity	使用 Embedding 計算生成答案與 Ground Truth 的向量餘弦相似度。
LLM-as-a-Judge	使用詳細 Prompt 指引高階 LLM 針對「流暢度、邏輯性、語氣」打分 (1-5分)。
系統結構與安全	JSON / Schema Validity	針對 Function Calling 或結構化輸出，驗證 JSON 解析成功率。
Safety / Toxicity / Jailbreak	檢測輸出是否包含偏見、毒性言論，或容易被 Prompt Injection 破解。
系統效能	Latency & TTFT	測量端到端延遲與首個 Token 回應時間 (Time to First Token)。
Token Cost & Usage	計算單次測試與長期執行的 Token 消耗與成本。
熱門的現成 Harness 工具與框架

Ragas / TruLens：專為 RAG 應用設計的評測框架，提供完整的忠實度與相關性算子。

Promptfoo：輕量級 CLI 工具，專門用於 CI/CD 流程中對 Prompt、Model 與 RAG 進行安全與品質比對。

DeepEval：類似 Pytest 語法的 Python 評測庫，方便將 LLM 測試嵌入現有單元測試體系。

LangSmith / Phoenix / Braintrust：企業級的 LLM 可視化 Trace、評測與數據集管理平台。

請提供一個使用 Python 或 DeepEval 撰寫 LLM 自動化測試與評測的實際代碼範例。

使用 Python 與 DeepEval（目前極受歡迎的 LLM 單元測試框架）撰寫評測程式碼非常直觀。DeepEval 與 pytest 深度整合，讓你可以像寫一般軟體測試一樣，測試 LLM 回應的忠實度（Faithfulness）、回答相關性（Answer Relevancy） 與 JSON 結構規範。

1. 安裝套件與設定
首先安裝 DeepEval 與 Pytest：

Bash
pip install deepeval pytest
設定你的 OpenAI API Key（DeepEval 預設會使用 GPT 模型作為評判裁判 LLM-as-a-Judge）：

Bash
export OPENAI_API_KEY="your-api-key"
2. 測試範例程式碼 (test_llm_app.py)
建立一個名為 test_llm_app.py 的檔案，內容如下：

Python
import pytest
from deepeval import assert_test
from deepeval.metrics import (
    AnswerRelevancyMetric,
    FaithfulnessMetric,
    JsonCorrectnessMetric,
)
from deepeval.test_case import LLMTestCase

# ==========================================
# 1. 模擬你的 LLM / RAG 應用程式邏輯
# ==========================================
def run_rag_pipeline(user_query: str) -> dict:
    """
    模擬 RAG 系統的運作流程：
    回傳：檢索到的 Context、LLM 生成的 Answer，以及結構化 JSON
    """
    # 模擬檢索出來的文檔背景資料
    retrieved_context = [
        "我們的退貨政策規定，消費者可在購買後 14 天內申請退款。",
        "退款申請必須附上原始購買發票，且商品需保持全新未拆封狀態。",
    ]

    # 模擬 LLM 產生的回答
    llm_answer = "您可以在購買後的 14 天內申請退款，但必須提供原始發票且商品必須全新未拆封。"

    return {
        "context": retrieved_context,
        "answer": llm_answer,
    }


# ==========================================
# 2. 撰寫 DeepEval 自動化測試案例
# ==========================================

def test_rag_faithfulness_and_relevancy():
    """
    測試 1：評測 RAG 的忠實度 (是否包含幻覺) 與回答相關性
    """
    input_query = "退貨需要準備什麼資料？幾天內要提出？"
    
    # 執行你的 RAG 系統
    result = run_rag_pipeline(input_query)

    # 建立 DeepEval 的測試用例 (LLMTestCase)
    test_case = LLMTestCase(
        input=input_query,
        actual_output=result["answer"],
        retrieval_context=result["context"]
    )

    # 定義評測指標 (Metrics)
    # 1. Faithfulness: 檢查 Answer 是否完全忠於 Retrieval Context (無幻覺)
    faithfulness_metric = FaithfulnessMetric(threshold=0.7)
    
    # 2. Answer Relevancy: 檢查 Answer 是否直接回答了 Input Query
    relevancy_metric = AnswerRelevancyMetric(threshold=0.7)

    # 執行斷言 (Assert)，若指標低於 threshold 則測試失敗
    assert_test(
        test_case=test_case,
        metrics=[faithfulness_metric, relevancy_metric]
    )


def test_json_output_schema():
    """
    測試 2：驗證 LLM 生成的 JSON 是否符合格式與語意規範
    """
    # 模擬 LLM 生成的 JSON 字串
    llm_json_output = '{"status": "success", "days_allowed": 14, "require_receipt": true}'
    
    test_case = LLMTestCase(
        input="請將退貨條件轉為 JSON 格式",
        actual_output=llm_json_output
    )

    # 驗證 JSON 格式正確性
    json_metric = JsonCorrectnessMetric()

    assert_test(test_case=test_case, metrics=[json_metric])
3. 執行測試與檢視報告
在命令列執行 deepeval test（或直接使用 pytest）：

Bash
deepeval test run test_llm_app.py
測試結果示範：

DeepEval 會自動呼叫裁判模型評分，並在 terminal 輸出漂亮的表格與細節：

Plaintext
PASSED test_llm_app.py::test_rag_faithfulness_and_relevancy
Metrics Summary:
  - FaithfulnessMetric: Score: 1.00 (Threshold: 0.70, Passed: True)
    Reason: The output directly cites conditions present in the context.
  - AnswerRelevancyMetric: Score: 0.95 (Threshold: 0.70, Passed: True)
    Reason: The response directly addresses both the required items and the timeline.

PASSED test_llm_app.py::test_json_output_schema
Metrics Summary:
  - JsonCorrectnessMetric: Score: 1.00 (Threshold: 0.50, Passed: True)
關鍵機制說明
LLMTestCase：DeepEval 的核心數據結構，收集 input（提問）、actual_output（模型回答）、retrieval_context（檢索內容）與 expected_output（黃金答案）。

threshold（閾值）：每個 Metric 分數介於 0.0∼1.0，當實際分數高於 threshold 時，assert_test 才會通過。

Reason（失敗或計分原因）：裁判 LLM 不僅給分，還會生成解釋，方便在 CI/CD 測試失敗時快速 Debug。

請說明如何將 DeepEval 的 LLM 自動化測試整合到 GitHub Actions CI/CD 流程中。

將 DeepEval 整合至 GitHub Actions CI/CD 流程中，可以在每次開發者提交代碼或發起 Pull Request (PR) 時，自動對 Prompt、模型配置或 RAG 邏輯進行迴歸測試。若評測分數未達門檻（例如 Faithfulness < 0.7），系統會自動阻擋 PR 合併，確保線上模型的品質。

1. 準備工作與 Secrets 設定
在整合至 CI/CD 之前，必須先將 LLM 裁判與應用程式需要的 API Key 託管至 GitHub：

進入你的 GitHub Repository -> Settings -> Secrets and variables -> Actions。

點擊 New repository secret，新增以下金鑰：

OPENAI_API_KEY：DeepEval 用來作為裁判模型（LLM-as-a-Judge）打分使用。

(可選) CONFIDENT_AI_API_KEY：若有使用 DeepEval 的雲端平台（Confident AI）記錄測試歷史趨勢，可在此設定。

2. 建立 GitHub Actions 工作流 (Workflow)
在你的專案根目錄下建立檔案：.github/workflows/llm_eval.yml。

YAML
name: LLM Evaluation CI

# 觸發條件：當有人在 main 分支發起 PR 或直接 Push 時執行
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  evaluate-llm:
    runs-on: ubuntu-latest

    steps:
      # 1. 檢出專案程式碼
      - name: Checkout repository
        uses: actions/checkout@v4

      # 2. 設定 Python 環境
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
          cache: 'pip'

      # 3. 安裝依賴套件
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install deepeval pytest

      # 4. 執行 DeepEval 評測測試
      - name: Run DeepEval Tests
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          # CONFIDENT_AI_API_KEY: ${{ secrets.CONFIDENT_AI_API_KEY }} # 若使用雲端平台下記錄
        run: |
          # 執行專案中所有的 deepeval 測試檔案
          deepeval test run tests/test_llm_app.py
3. 高級應用：在 Pull Request 中自動回覆評測結果
你可以結合 GitHub Actions 的 PR Comment 功能，將評測分數直接渲染為 Markdown 表格並留言在 PR 頁面中，方便 Code Reviewer 審查：

YAML
      # 5. (可選) 匯出結果並自動在 PR 留言
      - name: Run Tests and Export Result
        if: github.event_name == 'pull_request'
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          # 執行測試並生成 JUnit XML 報告
          deepeval test run tests/test_llm_app.py --junit-xml=test-results.xml

      - name: Publish Test Report to PR
        uses: mikepenz/action-junit-report@v4
        if: always() # 即使測試失敗也發布報告
        with:
          report_paths: 'test-results.xml'
          check_name: 'LLM Eval Results'
最佳實踐建議
區分單元測試與端到端評測：

快速測試 (PR 層級)：只跑 10-20 筆代表性的黃金數據集 (Golden Dataset)，降低 API 費用與 CI 執行時間。

完整測試 (Nightly / Release 層級)：設置定期執行的 Cron Job（例如每天凌晨），對數百筆完整數據集進行全面的 Benchmarking。

設置 API Rate Limit 與重試：在 CI 中大批量呼叫 LLM 裁判時，可在 DeepEval 中調整 async_mode=True 或適當設置併發數，避免觸發 API Rate Limit。

能用 deepeval 寫個 loop engineering 的 coding agent 案例嗎？

Loop Engineering（迴圈工程 / 代理人自省修復）的核心理念在於：LLM 生成結果 → 工具或編譯器執行 → 捕獲錯誤 → 將錯誤丟回 LLM 進行自我修正 (Self-Correction)，直到通過驗證或達到上限。

以下是一個完整的代碼範例，結合 Python 的 exec() 執行環境與 DeepEval 來測試並驗證一個具備「自動除錯迴圈 (Debug Loop)」的 Coding Agent。

1. 安裝需求套件
Bash
pip install deepeval pytest openai
2. 測試範例程式碼 (test_coding_agent_loop.py)
建立名為 test_coding_agent_loop.py 的檔案：

Python
import sys
import io
import pytest
from openai import OpenAI
from deepeval import assert_test
from deepeval.metrics import GEval, ExactMatchMetric
from deepeval.test_case import LLMTestCase, LLMTestCaseParams

client = OpenAI()

# ==========================================
# 1. 安全程式碼執行器 (Code Execution Helper)
# ==========================================
def execute_python_code(code_str: str) -> tuple[bool, str]:
    """
    執行 Python 程式碼並擷取 stdout 或 Exception 錯誤訊息
    """
    buffer = io.StringIO()
    sys.stdout = buffer
    local_env = {}
    
    try:
        # 清除 Markdown 標籤以純化程式碼
        cleaned_code = code_str.replace("```python", "").replace("```", "").strip()
        exec(cleaned_code, local_env, local_env)
        sys.stdout = sys.__stdout__
        return True, buffer.getvalue().strip()
    except Exception as e:
        sys.stdout = sys.__stdout__
        return False, f"{type(e).__name__}: {str(e)}"


# ==========================================
# 2. Coding Agent with Loop Engineering
# ==========================================
def run_coding_agent_with_loop(prompt: str, max_iterations: int = 3) -> dict:
    """
    帶有 Self-Correction 迴圈的 Coding Agent：
    生成程式碼 -> 執行測試 -> 若出錯則帶入 Error 再次生成 (Loop)
    """
    messages = [
        {"role": "system", "content": "你是一個專業 Python 工程師。請只輸出可執行的 Python 程式碼，不要包含任何解釋。"},
        {"role": "user", "content": prompt}
    ]
    
    iterations_used = 0
    final_code = ""
    execution_output = ""
    success = False

    # 🔄 Loop Engineering 核心迴圈
    for i in range(max_iterations):
        iterations_used = i + 1
        
        # LLM 生成或修正程式碼
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=messages,
            temperature=0.1
        )
        final_code = response.choices[0].message.content
        
        # 嘗試執行程式碼
        is_ok, output = execute_python_code(final_code)
        execution_output = output

        if is_ok:
            success = True
            break  # 執行成功，跳出迴圈
        
        # ❌ 執行失敗：將錯誤訊息寫入 Context，觸發下一輪自省修正 (Self-Correction Loop)
        messages.append({"role": "assistant", "content": final_code})
        messages.append({
            "role": "user", 
            "content": f"程式碼執行失敗，請修正以下錯誤並重新生成純程式碼：\n{execution_output}"
        })

    return {
        "code": final_code,
        "output": execution_output,
        "success": success,
        "iterations": iterations_used
    }


# ==========================================
# 3. 使用 DeepEval 評測 Loop Coding Agent
# ==========================================

def test_coding_agent_loop_recovery():
    """
    測試 Coding Agent 是否能透過 Loop 機制正確修正寫錯的程式碼
    """
    # 故意給予一個容易寫錯的 Prompt（例如需要處理邊界條件或型別問題）
    user_prompt = """
請寫一個函數 `sum_numbers(input_list)`，計算傳入列表中所有「數字類型」的總和。
列表中可能混合字串格式的數字（如 "10"）以及非數字（如 "hello"）。
範例輸入：[1, "2", "abc", 3.5] -> 應回傳：6.5
最後請呼叫此函數並印出 print(sum_numbers([1, "2", "abc", 3.5]))
    """

    # 執行 Coding Agent (允許最多 3 次迴圈迭代)
    result = run_coding_agent_with_loop(user_prompt, max_iterations=3)

    # 建立 DeepEval 測試案例
    test_case = LLMTestCase(
        input=user_prompt,
        actual_output=result["output"],  # 以最終程式碼的執行結果作為 output
        expected_output="6.5"            # 預期的執行輸出
    )

    # 評測指標 1: 精確匹配結果 (Exact Match)
    exact_match_metric = ExactMatchMetric(threshold=1.0)

    # 評測指標 2: 使用 G-Eval 自訂對 Loop 效率與結果的評分
    loop_efficiency_metric = GEval(
        name="Coding Agent Loop Efficiency",
        criteria="評估 Coding Agent 是否成功解決問題，且迴圈迭代次數合理（儘量在 3 次內完成）。",
        evaluation_params=[LLMTestCaseParams.INPUT, LLMTestCaseParams.ACTUAL_OUTPUT],
        threshold=0.8
    )

    # 斷言：驗證 Agent 執行輸出是否正確
    assert_test(
        test_case=test_case,
        metrics=[exact_match_metric, loop_efficiency_metric]
    )

    # 補充斷言：驗證 Python 語法是否成功通過執行
    assert result["success"] is True, f"Agent 在 {result['iterations']} 次迴圈後仍無法修正錯誤！"
3. 執行測試與觀測 Loop 效果
在終端機執行：

Bash
deepeval test run test_coding_agent_loop.py
💡 這個案例體現的 Loop Engineering 關鍵要素
反饋機制 (Feedback Mechanism)：execute_python_code 擔任環境檢查者（Environment Guard），捕獲 Python 執行期錯誤（Runtime Exception）。

狀態記憶 (State & Context Update)：將對應的歷史錯誤訊息 Append 回 messages 中，提供 LLM 精確的除錯訊號。

終止條件 (Termination Condition)：當 is_ok == True（執行成功）或達到 max_iterations（最大次數限制）時跳出，防止無限迴圈消耗 Token。

DeepEval 斷言：DeepEval 不僅評估最終輸出（6.5），還可以斷言 result["success"] 與迭代次數，確保 Agent 具備實質的修復能力。

請改寫此案例，讓 Agent 在 Loop 中自動撰寫與執行 Pytest 單元測試來進行校正。

將單純的 exec() 執行器升級為 「TDD (測試驅動開發) 雙層迴圈」，可以讓 Agent 具備更強的自主驗證能力。

在這個架構中，Agent 不僅要寫出目標程式碼，還需要自動生成 Pytest 單元測試。迴圈將透過 pytest.main() 在記憶體中動態執行測試，若測試失敗（AssertionError 或 Exception），Agent 會根據 Pytest 的詳細 failure report 進行自我校正。

測試與校正流程架構
[使用者需求]
    │
    ▼
┌──────────────────────────────────────────────┐
│  LLM Agent 生成：                            │
│  1. 目標函數 (solution.py)                   │
│  2. Pytest 單元測試 (test_solution.py)        │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│  單元測試執行器 (Pytest Memory Executor)       │
└──────────────────────┬───────────────────────┘
                       │
             ┌─────────┴─────────┐
             │ 是否全數通過？     │
             └────┬─────────┬────┘
               No │         │ Yes
                  │         ▼
                  │    ┌─────────────────────────┐
                  │    │ 通過驗證！               │
                  │    │ 交付程式碼與報告        │
                  │    └─────────────────────────┘
                  ▼
┌──────────────────────────────────────────────┐
│ 擷取 Pytest Failure Report                   │
│ 帶回 LLM Context 進行自我修復 (Self-Correct) │
└──────────────────────────────────────────────┘
完整範例程式碼 (test_tdd_coding_agent.py)
Python
import sys
import tempfile
import pytest
from openai import OpenAI
from deepeval import assert_test
from deepeval.metrics import GEval, ExactMatchMetric
from deepeval.test_case import LLMTestCase, LLMTestCaseParams

client = OpenAI()

# ==========================================
# 1. 記憶體/臨時檔案中的 Pytest 執行器
# ==========================================
def run_pytest_on_code(source_code: str, test_code: str) -> tuple[bool, str]:
    """
    將生成的程式碼與單元測試寫入臨時目錄，並執行 Pytest 捕獲詳細報告
    """
    with tempfile.TemporaryDirectory() as temp_dir:
        source_path = f"{temp_dir}/solution.py"
        test_path = f"{temp_dir}/test_solution.py"

        # 寫入目標程式碼與測試程式碼
        with open(source_path, "w", encoding="utf-8") as f:
            f.write(source_code)
            
        with open(test_path, "w", encoding="utf-8") as f:
            # 自動導入 solution.py
            f.write("from solution import *\n" + test_code)

        # 執行 Pytest 並擷取標準輸出 (Captured Output)
        # -q: 簡潔輸出, --tb=short: 擷取短版的 Failure Traceback
        class PytestResultCollector:
            def __init__(self):
                self.output = []
            def write(self, text):
                self.output.append(text)
            def flush(self):
                pass

        collector = PytestResultCollector()
        original_stdout = sys.stdout
        sys.stdout = collector
        
        # 執行 pytest
        exit_code = pytest.main([test_path, "-q", "--tb=short"])
        
        sys.stdout = original_stdout
        pytest_output = "".join(collector.output)

        # exit_code == 0 代表所有 test 都 PASSED
        is_success = (exit_code == 0)
        return is_success, pytest_output


# ==========================================
# 2. TDD Loop Coding Agent
# ==========================================
def run_tdd_agent_loop(task_description: str, max_iterations: int = 3) -> dict:
    """
    結合 Pytest 自我驗證的 Coding Agent 迴圈
    """
    system_prompt = """
你是一個 TDD (測試驅動開發) 導向的 Python 專家。
請根據使用者的需求，同時生成「目標程式碼」與「Pytest 單元測試」。

請嚴格遵守以下格式輸出（使用標籤隔開）：

<CODE>
# 這裡放實作的 Python 程式碼 / 函數
</CODE>

<TEST>
# 這裡放以 test_ 開頭的 Pytest 測試函數
# 注意：直接呼叫目標函數，不需要寫 import
</TEST>
"""

    messages = [
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": task_description}
    ]

    final_source_code = ""
    final_test_code = ""
    last_report = ""
    success = False

    for iteration in range(1, max_iterations + 1):
        # 1. LLM 生成程式碼與測試
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=messages,
            temperature=0.1
        )
        content = response.choices[0].message.content

        # 解析 <CODE> 與 <TEST> 區塊
        try:
            source_code = content.split("<CODE>")[1].split("</CODE>")[0].strip()
            test_code = content.split("<TEST>")[1].split("</TEST>")[0].strip()
        except IndexError:
            # 若 LLM 未按格式輸出，視為格式錯誤並重新要求
            messages.append({"role": "assistant", "content": content})
            messages.append({"role": "user", "content": "格式錯誤！請嚴格按照 <CODE>...</CODE> 與 <TEST>...</TEST> 標籤輸出。"})
            continue

        # 移除 markdown code block 標籤
        source_code = source_code.replace("```python", "").replace("```", "").strip()
        test_code = test_code.replace("```python", "").replace("```", "").strip()

        # 2. 執行 Pytest 測試
        passed, report = run_pytest_on_code(source_code, test_code)
        
        final_source_code = source_code
        final_test_code = test_code
        last_report = report

        if passed:
            success = True
            break  # 測試全數通過，跳出迴圈

        # 3. 測試失敗：將完整 Pytest 失敗報告寫回 Context，引導 Agent 自我修復
        messages.append({"role": "assistant", "content": content})
        messages.append({
            "role": "user",
            "content": f"Pytest 單元測試失敗！以下是測試報告：\n\n{report}\n\n請根據錯誤訊息重新檢視你的實作或測試邏輯，並修正後再次輸出 <CODE> 與 <TEST>。"
        })

    return {
        "source_code": final_source_code,
        "test_code": final_test_code,
        "pytest_report": last_report,
        "success": success,
        "iterations": iteration
    }


# ==========================================
# 3. 使用 DeepEval 評測 Agent 表現
# ==========================================
def test_agent_tdd_self_correction():
    """
    評測 TDD Agent 是否能透過自動測試迴圈解決問題
    """
    user_task = """
請實作一個函數 `calculate_discount(prices, discount_rate, min_spend)`：
1. `prices`: 購物車商品價格列表（包含整數與浮點數）。
2. `discount_rate`: 折扣率（如 0.2 代表 20% off / 8折）。
3. `min_spend`: 最低消費門檻。若價格總和未達門檻，不予折扣。
4. 回傳：最終應付金額（四捨五入至小數點後兩位）。
5. 特殊條件：若 prices 包含無效價格（小於等于 0 的數字）或非數字型別，應拋出 ValueError。

請提供包含多個測試案例 (包含邊界條件與異常處理) 的 Pytest 測試。
"""

    # 執行 Agent 進行 TDD 迴圈
    result = run_tdd_agent_loop(user_task, max_iterations=3)

    # 建立 DeepEval 測試案例
    test_case = LLMTestCase(
        input=user_task,
        actual_output=f"Success: {result['success']}\nIterations: {result['iterations']}\nReport:\n{result['pytest_report']}"
    )

    # 評測指標：評估產出的單元測試完整性與 TDD 自我修復效率
    tdd_quality_metric = GEval(
        name="TDD Correction Quality",
        criteria="評估 Agent 是否成功產出高質量的單元測試，且目標程式碼全數通過了這些測試。",
        evaluation_params=[LLMTestCaseParams.INPUT, LLMTestCaseParams.ACTUAL_OUTPUT],
        threshold=0.8
    )

    # 執行 DeepEval 斷言
    assert_test(test_case=test_case, metrics=[tdd_quality_metric])

    # 系統級斷言：確保 Pytest 全數通過
    assert result["success"] is True, f"Agent 經過 {result['iterations']} 次迭代後仍無法通過單元測試！"
執行與結果呈現
當你運行 deepeval test run test_tdd_coding_agent.py 時，如果 Agent 在第一輪寫出的測試覆蓋到了潛在 Bug（例如忘記對 ValueError 進行斷言，或是四捨五入邏輯錯誤），Pytest 報告會明確指出：

Plaintext
FAILED test_solution.py::test_invalid_price - Failed: DID NOT RAISE <class 'ValueError'>
Agent 在下一輪會讀取到這個失敗訊息，自動將代碼修復為 if p <= 0: raise ValueError(...)，並再次執行 Pytest 直到輸出完全為：

Plaintext
1 passed in 0.02s
請說明如何使用 LangGraph 將這個包含 Pytest 執行的 TDD Agent 改寫為明確的 Graph 狀態機 (Nodes & Edges)。

將 TDD Coding Agent 改寫為 LangGraph 狀態機，是將 Loop Engineering 提升至 Graph Engineering 的典型示範。透過將「程式碼生成」、「Pytest 測試執行」與「錯誤修復」拆解為獨立的 節點 (Nodes)，並使用 條件邊 (Conditional Edges) 決定流程跳轉，能讓原本隱晦的 for 迴圈轉變為透明、可追蹤且易於維護的狀態機。

LangGraph 狀態機架構圖
                 [ Start ]
                     │
                     ▼
             ┌───────────────┐
             │ generate_code │ (Node 1: 生成程式碼與測試)
             └───────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │  run_pytest   │ (Node 2: 執行 Pytest 沙盒)
             └───────┬───────┘
                     │
                     ▼
         < check_test_result > (Conditional Edge: 驗證測試結果)
            /                 \
  (測試失敗 & 未達上限)     (測試成功 OR 達到上限)
          /                     \
         ▼                       ▼
┌─────────────────┐          [ End ]
│ self_correction │
└────────┬────────┘
         │
         └───────► (回到 run_pytest)
1. 安裝需求套件
Bash
pip install langgraph langchain-openai pytest deepeval
2. 完整 LangGraph 程式碼實作 (langgraph_tdd_agent.py)
Python
import sys
import tempfile
import pytest
from typing import TypedDict, List, Literal
from langchain_openai import ChatOpenAI
from langchain_core.messages import BaseMessage, HumanMessage, AIMessage, SystemMessage
from langgraph.graph import StateGraph, END

# ==========================================
# 1. 定義 Agent 狀態 (Graph State)
# ==========================================
class AgentState(TypedDict):
    messages: List[BaseMessage]   # 對話歷史與 Prompt Context
    source_code: str              # 生成的目標程式碼
    test_code: str                # 生成的 Pytest 單元測試
    pytest_report: str            # Pytest 執行報告
    success: bool                 # 是否全數通過測試
    iteration: int                # 當前重試次數
    max_iterations: int           # 最大重試次數上限


# ==========================================
# 2. Pytest 執行沙盒工具 (Sandbox Helper)
# ==========================================
def execute_pytest_in_sandbox(source_code: str, test_code: str) -> tuple[bool, str]:
    """在臨時目錄中寫入檔案並執行 pytest"""
    with tempfile.TemporaryDirectory() as temp_dir:
        source_path = f"{temp_dir}/solution.py"
        test_path = f"{temp_dir}/test_solution.py"

        with open(source_path, "w", encoding="utf-8") as f:
            f.write(source_code)
        with open(test_path, "w", encoding="utf-8") as f:
            f.write("from solution import *\n" + test_code)

        class PytestResultCollector:
            def __init__(self):
                self.output = []
            def write(self, text):
                self.output.append(text)
            def flush(self):
                pass

        collector = PytestResultCollector()
        original_stdout = sys.stdout
        sys.stdout = collector
        
        exit_code = pytest.main([test_path, "-q", "--tb=short"])
        
        sys.stdout = original_stdout
        return (exit_code == 0), "".join(collector.output)


# ==========================================
# 3. 定義 Graph 節點 (Nodes)
# ==========================================
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.1)

SYSTEM_PROMPT = """你是一個 TDD 導向的 Python 專家。請根據需求生成「目標程式碼」與「Pytest 單元測試」。
請嚴格使用以下標籤格式輸出：
<CODE>
# Python 實作程式碼
</CODE>
<TEST>
# Pytest 測試程式碼 (直接呼叫目標函數，無需寫 import)
</TEST>
"""

def parse_code_and_test(content: str) -> tuple[str, str]:
    """解析 LLM 輸出的 <CODE> 與 <TEST> 區塊"""
    try:
        source_code = content.split("<CODE>")[1].split("</CODE>")[0].strip()
        test_code = content.split("<TEST>")[1].split("</TEST>")[0].strip()
        source_code = source_code.replace("```python", "").replace("```", "").strip()
        test_code = test_code.replace("```python", "").replace("```", "").strip()
        return source_code, test_code
    except IndexError:
        return "", ""

def generate_code_node(state: AgentState) -> dict:
    """節點 1：初始程式碼與測試生成"""
    messages = [SystemMessage(content=SYSTEM_PROMPT)] + state["messages"]
    response = llm.invoke(messages)
    source_code, test_code = parse_code_and_test(response.content)
    
    return {
        "source_code": source_code,
        "test_code": test_code,
        "messages": [AIMessage(content=response.content)],
        "iteration": 1
    }

def run_pytest_node(state: AgentState) -> dict:
    """節點 2：執行 Pytest 測試"""
    passed, report = execute_pytest_in_sandbox(state["source_code"], state["test_code"])
    return {
        "success": passed,
        "pytest_report": report
    }

def self_correction_node(state: AgentState) -> dict:
    """節點 3：修復程式碼 (Self-Correction)"""
    feedback_prompt = f"""Pytest 單元測試失敗！測試報告如下：
\n{state['pytest_report']}\n
請根據此錯誤訊息重新檢視實作與測試邏輯，修正後再次輸出 <CODE> 與 <TEST>。"""

    updated_messages = state["messages"] + [HumanMessage(content=feedback_prompt)]
    messages = [SystemMessage(content=SYSTEM_PROMPT)] + updated_messages
    
    response = llm.invoke(messages)
    source_code, test_code = parse_code_and_test(response.content)

    return {
        "source_code": source_code,
        "test_code": test_code,
        "messages": updated_messages + [AIMessage(content=response.content)],
        "iteration": state["iteration"] + 1
    }


# ==========================================
# 4. 定義條件邊 (Conditional Edge)
# ==========================================
def should_continue(state: AgentState) -> Literal["self_correction", "__end__"]:
    """判斷是否進入自我修復節點，或是結束 Graph"""
    if state["success"]:
        print("✅ 單元測試全數通過，完成 Task！")
        return END
    
    if state["iteration"] >= state["max_iterations"]:
        print(f"⚠️ 已達到最大重試次數 ({state['max_iterations']} 次)，結束 Graph。")
        return END
    
    print(f"❌ 測試失敗 (第 {state['iteration']} 次)，進入 Self-Correction 節點...")
    return "self_correction"


# ==========================================
# 5. 組裝與編譯 LangGraph (StateGraph)
# ==========================================
def build_tdd_agent_graph():
    workflow = StateGraph(AgentState)

    # 新增三個核心節點
    workflow.add_node("generate_code", generate_code_node)
    workflow.add_node("run_pytest", run_pytest_node)
    workflow.add_node("self_correction", self_correction_node)

    # 設定起始節點與固定邊 (Edges)
    workflow.set_entry_point("generate_code")
    workflow.add_edge("generate_code", "run_pytest")
    
    # 新增條件邊 (Conditional Edge)
    workflow.add_conditional_edges(
        "run_pytest",
        should_continue,
        {
            "self_correction": "self_correction",
            END: END
        }
    )
    
    # 自我修復後回到 run_pytest 再次驗證 (形成 Loop)
    workflow.add_edge("self_correction", "run_pytest")

    return workflow.compile()


# ==========================================
# 6. 執行測試示範
# ==========================================
if __name__ == "__main__":
    app = build_tdd_agent_graph()

    initial_input = {
        "messages": [HumanMessage(content="""
請寫一個函數 `is_valid_email(email: str) -> bool`：
1. 驗證是否為合法 Email。
2. 必須包含 `@` 與域名點號 `.`。
3. 如果輸入不是字串，需拋出 TypeError。
並撰寫包含邊界測試的 Pytest。
        """)],
        "max_iterations": 3,
        "iteration": 0,
        "success": False
    }

    # 執行 Graph 狀態機
    final_state = app.invoke(initial_input)

    print("\n======== 最終 Graph 執行結果 ========")
    print(f"成功狀態: {final_state['success']}")
    print(f"總迭代次數: {final_state['iteration']}")
    print(f"最終程式碼:\n{final_state['source_code']}")
重構為 Graph 狀態機的核心優勢
職責明確 (Decoupled Nodes)：generate_code 專注生成，run_pytest 專注環境測試，self_correction 專注除錯，避免單一 Prompt 承載過多混亂邏輯。

靈活控制流程 (Control Flow)：可隨時透過 should_continue 插入其他邏輯（例如：連續失敗兩次後切換為能力更強的 GPT-4o 裁判，或加入人工審查 Human-in-the-loop）。

可追蹤性 (Observability)：結合 LangSmith 後，能在介面上清晰看到狀態機在各個 Node 與 Edge 之間的跳轉軌跡、傳遞的 AgentState 狀態變化與耗時。

請展示如何撰寫一個 DeepEval 測試案例，對整個 LangGraph TDD Agent 狀態機進行端到端評測。

對 LangGraph 狀態機進行 端到端（End-to-End, E2E）評測 時，我們需要驗證的目標包含兩個層次：

系統行為與結果驗證 (System Determinism)：Graph 最終是否順利收斂？Pytest 測試是否通過？總迭代次數是否在合理範圍內？

生成品質語意評測 (LLM-as-a-Judge)：使用 DeepEval 的 GEval 對產出的程式碼可讀性、註解品質以及 Pytest 測試案例的覆蓋度進行打分。

端到端評測架構 (test_langgraph_e2e.py)
你可以直接建立一個獨立的評測檔案，將 LangGraph 狀態機封裝為被測試物件，並結合 DeepEval 的 LLMTestCase 與 GEval 進行評測。

Python
import pytest
from deepeval import assert_test
from deepeval.metrics import GEval
from deepeval.test_case import LLMTestCase, LLMTestCaseParams
from langchain_core.messages import HumanMessage

# 匯入上一階段建立的 LangGraph 狀態機編譯對象
from langgraph_tdd_agent import build_tdd_agent_graph


# ==========================================
# 1. 準備端到端 (E2E) 測試資料集
# ==========================================
E2E_TEST_DATASET = [
    {
        "name": "複雜邏輯：購物車折扣計算器",
        "task": """
請實作一個函數 `calculate_cart_total(items, discount_code=None)`：
1. `items`: 包含商品字典的列表，例如 `[{"price": 100, "quantity": 2}, ...]`。
2. `discount_code`: 
   - "SAVE10": 總金額扣減 10%。
   - "50OFF": 總金額滿 200 時扣減 50。
3. 邊界條件：若商品價格或數量 <= 0，拋出 `ValueError`。若無效的折扣碼，拋出 `KeyError`。
4. 請提供包含邊界測試的 Pytest。
""",
        "max_iterations": 3
    },
    {
        "name": "資料結構處理：深層字典合併",
        "task": """
請實作一個函數 `deep_merge_dicts(dict1, dict2)`：
1. 遞迴合併兩個字典。若相同的 key 對應的值都是字典，應繼續向下合併。
2. 若相同的 key 對應的值非字典，以 `dict2` 的值覆蓋 `dict1`。
3. 若傳入非字典型別，拋出 `TypeError`。
4. 請提供包含遞迴邊界測試的 Pytest。
""",
        "max_iterations": 3
    }
]


# ==========================================
# 2. 定義 DeepEval 評測指標 (Metrics)
# ==========================================

# 指標 1: 評估程式碼與單元測試的質量與完整度
code_quality_metric = GEval(
    name="Code & Test Quality",
    criteria="""
評估 Agent 產出的 Python 實作程式碼與 Pytest 測試案例：
1. 實作程式碼是否結構清晰、遵守 Python 最佳實踐且有適當處理例外狀況。
2. 單元測試是否包含邊界條件測試 (Edge Cases) 與異常測試 (Exception handling)。
""",
    evaluation_params=[LLMTestCaseParams.INPUT, LLMTestCaseParams.ACTUAL_OUTPUT],
    threshold=0.8
)

# 指標 2: 評估 Graph 自我修正與收斂效率
graph_efficiency_metric = GEval(
    name="Graph Convergence Efficiency",
    criteria="""
評估 LangGraph 狀態機的運行效率：
1. 狀態機是否成功通過了 Pytest 測試 (success == True)。
2. 狀態機是否在合理的重試次數內完成 (儘量在 2 次內收斂，不應盲目陷入重試)。
""",
    evaluation_params=[LLMTestCaseParams.INPUT, LLMTestCaseParams.ACTUAL_OUTPUT],
    threshold=0.75
)


# ==========================================
# 3. 撰寫 DeepEval 端到端測試案例
# ==========================================

@pytest.mark.parametrize("test_case_data", E2E_TEST_DATASET, ids=[d["name"] for d in E2E_TEST_DATASET])
def test_langgraph_tdd_agent_e2e(test_case_data):
    """
    對 LangGraph TDD Agent 進行端到端 (E2E) 狀態機評測
    """
    # 1. 初始化 LangGraph 狀態機
    app = build_tdd_agent_graph()
    
    initial_state = {
        "messages": [HumanMessage(content=test_case_data["task"])],
        "max_iterations": test_case_data["max_iterations"],
        "iteration": 0,
        "success": False
    }

    # 2. 執行 Graph 狀態機 (端到端運行)
    final_state = app.invoke(initial_state)

    # 3. 整理實際輸出資訊以供 DeepEval 評測
    formatted_actual_output = f"""
=== 狀態機執行結果 ===
- 成功狀態 (Success): {final_state['success']}
- 總迭代次數 (Iterations): {final_state['iteration']} / {final_state['max_iterations']}

=== 生成的實作程式碼 ===
{final_state['source_code']}

=== 生成的 Pytest 測試 ===
{final_state['test_code']}

=== 最終 Pytest 執行報告 ===
{final_state['pytest_report']}
"""

    # 4. 建構 DeepEval 測試用例
    test_case = LLMTestCase(
        input=test_case_data["task"],
        actual_output=formatted_actual_output
    )

    # 5. 執行 DeepEval 評測斷言 (語意與品質打分)
    assert_test(
        test_case=test_case,
        metrics=[code_quality_metric, graph_efficiency_metric]
    )

    # 6. 硬性系統級斷言 (System Assertions)
    assert final_state["success"] is True, f"Graph 執行失敗：經過 {final_state['iteration']} 次迭代後仍未通過單元測試。"
    assert final_state["source_code"].strip() != "", "Graph 產出的目標程式碼不可為空。"
執行端到端評測
透過 CLI 執行 DeepEval：

Bash
deepeval test run test_langgraph_e2e.py
測試結果輸出示範：

Plaintext
PASSED test_langgraph_e2e.py::test_langgraph_tdd_agent_e2e[複雜邏輯：購物車折扣計算器]
Metrics Summary:
  - Code & Test Quality: Score: 0.90 (Threshold: 0.80, Passed: True)
    Reason: The generated code correctly raises ValueError and KeyError for edge cases, and pytest includes comprehensive test functions for each scenario.
  - Graph Convergence Efficiency: Score: 1.00 (Threshold: 0.75, Passed: True)
    Reason: The graph successfully converged in iteration 1 with zero test failures.

PASSED test_langgraph_e2e.py::test_langgraph_tdd_agent_e2e[資料結構處理：深層字典合併]
Metrics Summary:
  - Code & Test Quality: Score: 0.85 (Threshold: 0.80, Passed: True)
  - Graph Convergence Efficiency: Score: 0.80 (Threshold: 0.75, Passed: True)
    Reason: The agent failed pytest on iteration 1 due to TypeError handling, but self-corrected and passed on iteration 2.
端到端評測的三個關鍵價值
驗證動態自癒能力 (Self-Healing Validation)：如第二個測試案例所示，LLM 可能在第一輪寫錯，但 DeepEval 能評測 Graph 是否能透過 self_correction 節點正確修復並收斂。

防範重構造成的迴歸 (Regression Protection)：未來如果你更換了 Agent 內部的 Prompt、調整了 Node 的繪製方式，或是升級 LLM 模型，這套 E2E 測試能第一時間告訴你 Graph 的整體通關率是否下降。

可量化的架構優化：你可以輕鬆比較「無 Graph 的單次 Prompt」vs「帶有 Pytest Loop 的 LangGraph」在 Code Quality 分數上的巨大提升。

