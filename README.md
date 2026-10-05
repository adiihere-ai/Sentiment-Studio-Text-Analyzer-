# 🧠 Text Analyzer: Sentiment Studio

[![Python 3.12](https://img.shields.io/badge/Python-3.12-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=flat&logo=fastapi)](https://fastapi.tiangolo.com/)
[![Next.js 16](https://img.shields.io/badge/Next.js-16-black.svg?logo=next.js&logoColor=white)](https://nextjs.org/)
[![PyTorch 2.2.2](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Hugging Face](https://img.shields.io/badge/Hugging%20Face-FFD21E?style=flat&logo=huggingface&logoColor=black)](https://huggingface.co/)

**Sentiment Studio** is a robust, full-stack text analysis application leveraging PyTorch and Hugging Face Transformers. It provides a FastAPI service that serves four fine-tuned transformer models, paired with a modern Next.js workspace to analyze texts, run bulk batches, and explore model architectures.

---

## 🏗️ Architecture

The workspace is divided into two main components:

```text
sentiment-studio/
├── api/   FastAPI · Python 3.12 · uv · Ruff · pytest · torch 2.2.2 · transformers   → :8000
└── web/   Next.js 16 · React 19 · TypeScript 7 · Tailwind CSS 4 · shadcn/ui         → :3000
```

## 🤖 Supported Models

The system supports a variety of transformer models, which are downloaded automatically from the Hugging Face Hub on first use.

| Model | Hugging Face ID | Target Domain | Labels |
| :--- | :--- | :--- | :--- |
| **DistilBERT SST-2** *(default)* | `distilbert/distilbert-base-uncased-finetuned-sst-2-english` | General (reviews, comments) | Negative, Positive |
| **FinBERT** | `ProsusAI/finbert` | Financial news & reports | Negative, Neutral, Positive |
| **Twitter RoBERTa** | `cardiffnlp/twitter-roberta-base-sentiment-latest` | Social media & tweets | Negative, Neutral, Positive |
| **Multilingual BERT** | `nlptown/bert-base-multilingual-uncased-sentiment` | Multilingual product reviews | 1–5 stars |

*Note: The default model loads efficiently in the background during application startup.*

---

## 🚀 Quick Start

### 1. Start the API Server (Backend)
Open a terminal and run the following commands:
```bash
cd sentiment-studio/api
uv sync
cp .env.example .env            # Optional; defaults work seamlessly
uv run fastapi dev              # Runs on http://localhost:8000
```
*API documentation is available at `http://localhost:8000/docs`*

### 2. Start the Web Workspace (Frontend)
Open a second terminal and run:
```bash
cd sentiment-studio/web
npm install
npm run dev                     # Runs on http://localhost:3000
```
*The web application communicates seamlessly with `/api/*` via Next.js rewrites, bypassing CORS configuration requirements.*

---

## ✨ Features

### 🔍 Interactive Analysis
- **Deep Insights:** View prediction labels, confidence scores, and probability distributions for every class.
- **Word Influence:** Integrated Gradients explain model behavior (blue pushes toward positive, red toward negative).
- **Tokenization Inspector:** See raw token lists and truncation warnings for texts exceeding 512 tokens.
- **Quick Test:** `Ctrl/⌘ + Enter` provides instant feedback.

### 📦 Batch Processing
- **Bulk Uploads:** Paste text (one per line) or upload a `.CSV` / `.TXT` file (up to 1,000 rows).
- **Analytics Dashboard:** Review label distribution, average confidence, and identify low-confidence edge cases.
- **Data Export:** Filter results and export back to CSV containing full probability scores.

### 🛠️ Model Management
- **Dashboard:** Access model cards, runtime specs (device constraints, loading times), and switch models on the fly.
- **Proactive Loading:** Preload models ahead of time for uninterrupted processing.

### 🎨 User Experience
- **Persistent History:** Recent analyses are kept in your browser.
- **Responsive Design:** Supports fluid light/dark themes and a mobile-optimized layout.

---

## 🐳 Docker Deployment

To launch the entire stack via Docker:

```bash
cd sentiment-studio
cp api/.env.example api/.env      # Optional configuration
docker compose up --build         # Web → http://localhost:3000, API → http://localhost:8000
```
*Models are cached in Docker volumes to prevent redundant downloads. For NVIDIA GPU acceleration, uncomment the `deploy` block in `docker-compose.yml`.*

---

## 🧪 Quality & Testing

Ensure system integrity with our robust testing pipeline:

```bash
# Run backend tests, type checks, and formatting
cd sentiment-studio/api
uv run ruff check . && uv run ruff format --check . && uv run pytest

# Run frontend checks and builds
cd ../web
npm run typecheck && npm run build
```
*Our tests dynamically build lightweight transformer checkpoints to exercise actual PyTorch/Transformers paths without heavy downloads.*

---

## ⚙️ Configuration

Backend configurations are managed via environment variables. Refer to `api/.env.example` for comprehensive settings. Key variables include:

- `DEVICE`: Auto-detects CUDA/MPS/CPU.
- `DEFAULT_MODEL` / `ENABLED_MODELS`: Configure which models to load.
- `MAX_TEXT_CHARS` / `MAX_BATCH_ITEMS`: Safety limits for processing.
- `EXPLAIN_STEPS`: Precision control for Integrated Gradients.

---

*Built with ❤️ for text analysis and AI developers.*
