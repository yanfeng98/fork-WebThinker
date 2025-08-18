
<h1 align="center"> 🌐 WebThinker: Empowering Large Reasoning Models with Deep Research Capability</a></h1>

## 📣 Latest News

- **[May 30, 2025]**: 🔍 WebThinker now supports **[Google Serper API](https://serper.dev/)** for web search! Important: **[Bing Search API](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api)** will be retired in August 2025.

## 💡 Overview

**WebThinker** is a deep research framework fully powered by large reasoning models (LRMs). WebThinker enables LRMs to **autonomously search**, **deeply explore web pages**, and **draft research reports**, all within their thinking process.

### 📊 Overall Performance

<p align="center">
  <img src="figures/performance.png" width="100%" />
</p>

### ✨ The WebThinker Framework

![Model Comparison](figures/framework.png)

**Key Features:**
- We introduce a **Deep Web Explorer** that empowers LRMs to search, navigate pages by clicking interactive elements (like links or buttons), and extract relevant information. Based on initial search results, the LRM can initiate follow-up searches and traverse deeper links until it collects all relevant information.
- For scientific reporting, our **Autonomous Think-Search-and-Draft** strategy integrates real-time knowledge seeking with report creation. We equip LRMs with three specialized tools: (1) drafting content for specific chapters, (2) checking the current report, and (3) editing the report—ensuring reports remain comprehensive, coherent, and adaptive to new insights.

## 🔧 Installation

###  Environment Setup
```bash
# Create conda environment
conda create -n webthinker python=3.11
conda activate webthinker

# Install requirements
pip install -r requirements.txt
```

## 🏃 Quick Start

### Pre-preparation

#### Model Serving
Before running WebThinker, ensure your reasoning model and auxiliary model are served using vLLM. In our experiments, we use QwQ-32B as the reasoning model and Qwen-32B-Instruct as the auxiliary model. You can also explore other instruction-tuned models as your auxiliary model, which will be used in webpage reading, report writting/editting, evaluation, etc. For detailed instructions on model serving, see [here](https://docs.vllm.ai/en/stable/serving/distributed_serving.html).

#### Web Parser Client
For better web crawling performance, we recommend setting up a web parser client in `scripts/search/bing_search.py` using [Crawl4AI](https://github.com/unclecode/crawl4ai). This will help handle JavaScript-rendered content and provide more reliable webpage extraction.

### Problem Solving Mode

1. If you would like to ask a single question, run the following command:
```bash
python scripts/run_web_thinker.py \
    --single_question "What is OpenAI Deep Research?" \
    --search_engine "serper" \
    --serper_api_key "YOUR_GOOGLE_SERPER_API" \
    --api_base_url "YOUR_API_BASE_URL" \
    --model_name "QwQ-32B" \
    --aux_api_base_url "YOUR_AUX_API_BASE_URL" \
    --aux_model_name "Qwen2.5-32B-Instruct" \
    --tokenizer_path "PATH_TO_YOUR_TOKENIZER" \
    --aux_tokenizer_path "PATH_TO_YOUR_AUX_TOKENIZER"
```

2. If you would like to run results on benchmarks, run the following command:
```bash
python scripts/run_web_thinker.py \
    --dataset_name gaia \
    --split dev \
    --concurrent_limit 32 \
    --max_search_limit 15 \
    --search_engine "serper" \
    --serper_api_key "YOUR_GOOGLE_SERPER_API" \
    --api_base_url "YOUR_API_BASE_URL" \
    --model_name "QwQ-32B" \
    --aux_api_base_url "YOUR_AUX_API_BASE_URL" \
    --aux_model_name "Qwen2.5-32B-Instruct" \
    --tokenizer_path "PATH_TO_YOUR_TOKENIZER" \
    --aux_tokenizer_path "PATH_TO_YOUR_AUX_TOKENIZER"
```

### Report Generation Mode

1. If you would like to ask a single question, run the following command:
```bash
python scripts/run_web_thinker_report.py \
    --single_question "What are the models of OpenAI and what are the differences?" \
    --search_engine "serper" \
    --serper_api_key "YOUR_GOOGLE_SERPER_API" \
    --api_base_url "YOUR_API_BASE_URL" \
    --model_name "QwQ-32B" \
    --aux_api_base_url "YOUR_AUX_API_BASE_URL" \
    --aux_model_name "Qwen2.5-32B-Instruct" \
    --tokenizer_path "PATH_TO_YOUR_TOKENIZER" \
    --aux_tokenizer_path "PATH_TO_YOUR_AUX_TOKENIZER"
```

2. If you would like to run results on benchmarks, run the following command:
```bash
python scripts/run_web_thinker_report.py \
    --dataset_name glaive \
    --split test \
    --concurrent_limit 32 \
    --search_engine "serper" \
    --serper_api_key "YOUR_GOOGLE_SERPER_API" \
    --api_base_url "YOUR_API_BASE_URL" \
    --model_name "QwQ-32B" \
    --aux_api_base_url "YOUR_AUX_API_BASE_URL" \
    --aux_model_name "Qwen2.5-32B-Instruct" \
    --tokenizer_path "PATH_TO_YOUR_TOKENIZER" \
    --aux_tokenizer_path "PATH_TO_YOUR_AUX_TOKENIZER"
```

**Parameters Explanation:**
- `--dataset_name`: Name of the dataset to use (glaive).
- `--split`: Data split to run (test).
- `--single_question`: The question you want to ask when running in single question mode.
- `--concurrent_limit`: Maximum number of concurrent requests.
- `--max_search_limit`: Maximum number of search queries per reasoning session.
- `--search_engine`: Search engine to use (bing or serper). Default: bing.
- `--serper_api_key`: Your Google Serper API key (not required when using Bing).
- `--bing_subscription_key`: Your Bing Search API subscription key (not required when using Serper).
- `--api_base_url`: Base URL for the main model API.
- `--model_name`: Name of the main model to use.
- `--aux_api_base_url`: Base URL for the auxiliary model API.
- `--aux_model_name`: Name of the auxiliary model to use.

### Run Demo

```bash
cd demo
streamlit run_demo.py
```

**Note:** Before running, it is necessary to configure the relevant parameters in `demo/settings.py`.

### Benchmarks

The benchmarks we utilize are categorized into two types:
- **Complex Reasoning Benchmarks:**
    - **PhD-level Science QA:** [GPQA](https://arxiv.org/abs/2311.12022) (198 questions)
    - **General AI Assistant:** [GAIA](https://arxiv.org/abs/2311.12983) (103 questions)
    - **Web Exploration:** [WebWalkerQA](https://arxiv.org/abs/2501.07572) (680 questions)
    - **Extremely Difficult Reasoning Problems:** [Humanity's Last Exam (HLE)](https://arxiv.org/abs/2501.14249) (500 questions)
- **Scientific Report Evaluation:**
    - **General Open-ended Reasoning Problem:** [Reasoning-v1-20m](https://huggingface.co/datasets/glaiveai/reasoning-v1-20m) (30 questions)

All the pre-processed data is available in the `./data/` directory. For GAIA, HLE and Reasoning-v1-20m, we sampled a text-only subset of questions to efficiently conduct our evaluation.


### Evaluation

Our model inference scripts will automatically save the model's input and output texts for evaluation.

#### Problem Solving Evaluation

You can use the following command to evaluate the model's problem solving performance:

```bash
python scripts/evaluate/evaluate.py \
    --output_path "YOUR_OUTPUT_PATH" \
    --task math \
    --use_llm \
    --api_base_url "YOUR_AUX_API_BASE_URL" \
    --model_name "Qwen2.5-72B-Instruct" \
    --extract_answer
```
**Parameters Explanation:**
- `--output_path`: Path to the model's outputs for evaluation.
- `--task`: Task name. You can always set it to math (suitable for any QA task), unless it is a code task, then set it to code.
- `--use_llm`: Whether to use the LLM to evaluate the model's performance.
- `--api_base_url`: Base URL for the LLM API.
- `--model_name`: Model name for LLM evaluation.
- `--extract_answer`: Whether to extract the answer from the model's output, otherwise it will use the last few lines of the model's output as the final answer. Only used when `--use_llm` is set to `True`.

#### Report Generation Evaluation

We employ [DeepSeek-R1](https://api-docs.deepseek.com/) and [GPT-4o](https://platform.openai.com/docs/models/gpt-4o) to perform *listwise evaluation* for comparison of reports generated by different models. You can evaluate the reports using:

```bash
python scripts/evaluate/evaluate_report.py \
    --api-base-url "YOUR_API_BASE_URL" \
    --api-key "YOUR_API_KEY" \
    --models "YOUR_MODEL_NAME" \
    --model-to-test-dir "YOUR_MODEL_OUTPUT_DIRECTORY"
```
**Parameters Explanation:**
- `--api-base-url`: Base URL for the LLM API (e.g., "https://openrouter.ai/api/v1" or "https://api.openai.com/v1").
- `--api-key`: Your API key for the LLM service.
- `--models`: A list of model names (e.g., "deepseek/deepseek-r1", "openai/gpt-4o") to be used for evaluating the reports. The script will iterate through these models to get evaluations.
- `--model-to-test-dir`: Path to the directory where the generated reports (markdown files) from your model are stored.


📊 **Report Comparison Available**:

We've included the complete set of 30 test reports generated by **WebThinker**, **Grok3 DeeperSearch** and **Gemini2.0 Deep Research** in the `./outputs/` directory for your reference and comparison.
