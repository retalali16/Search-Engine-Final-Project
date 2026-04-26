# 🔍 NLP Semantic Search Engine — BBC News

A full end-to-end semantic search engine built on a live-scraped BBC News corpus, featuring TF-IDF and BM25 ranking, BERT-based query expansion with relevance feedback, and a Gradio web interface.

---

## 📌 Project Overview

This project implements a complete information retrieval pipeline across three phases:

| Phase | Components |
|-------|-----------|
| Phase 1 | Data Collection, Preprocessing, Inverted Index |
| Phase 2 | Query Processing, TF-IDF & BM25 Ranking |
| Phase 3 | Query Expansion (Sentence-BERT), Gradio UI, Evaluation |

---

## 🗂️ Project Structure

```
Search-Engine-Final-Project/
│
├── basic_search_engine.ipynb        # Main notebook (all phases)
├── bbc_scraped_news.csv             # Raw scraped BBC articles
├── preprocessed_news.csv            # Cleaned & lemmatized corpus
└── README.md
```

---

## 🚀 Phases

### Phase 1 — Data Collection, Preprocessing & Indexing

**1. Data Collection — Web Scraping**
- Scraped up to 20 live BBC News articles using **Selenium** with headless Chrome.
- Extracted full article text from `<p>` tags; saved URLs and content to `bbc_scraped_news.csv`.

**2. Preprocessing Pipeline**
- Lowercasing
- Punctuation & special character removal (`re`)
- Tokenization (`nltk.word_tokenize`)
- Stopword removal (`nltk.corpus.stopwords`)
- Lemmatization (`WordNetLemmatizer`)
- Output saved to `preprocessed_news.csv`

**3. Inverted Index**
- Built a `defaultdict`-based inverted index mapping each unique term → list of `(doc_id, term_frequency)` pairs.
- Used for fast document lookup during query processing.

---

### Phase 2 — Query Processing & Ranking

**TF-IDF Ranking**
- Query tokens are matched against the inverted index.
- Scores computed as: `TF × IDF` where IDF uses smoothed log formula.
- Documents ranked by descending cumulative TF-IDF score.

**BM25 Ranking**
- Implemented Okapi BM25 with `k1=1.5`, `b=0.75`.
- Accounts for document length normalization relative to average corpus length.
- More robust than TF-IDF for variable-length documents.

---

### Phase 3 — Query Expansion, UI & Evaluation

**Query Expansion with Sentence-BERT**
- Uses `all-MiniLM-L6-v2` (Sentence Transformers) to encode queries and corpus documents.
- **Relevance Feedback:** Top-5 most similar documents retrieved via cosine similarity.
- Frequent terms extracted from top documents (top-k frequency).
- Semantically similar terms identified via embedding cosine similarity.
- Final expanded query = original tokens + frequent terms + similar terms.

**Gradio Web Interface**
- Interactive search box accepting free-text queries.
- Displays: expanded query, search time, precision, recall, and top-5 results with URLs and scores.

**Evaluation**
- Precision and Recall computed per query against retrieved document set.
- Search latency measured end-to-end (scraping excluded).

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| Web Scraping | Selenium, ChromeDriver, webdriver-manager |
| NLP & Preprocessing | NLTK (tokenization, stopwords, lemmatization) |
| Ranking | TF-IDF (custom), BM25 (custom) |
| Query Expansion | Sentence-Transformers (`all-MiniLM-L6-v2`), scikit-learn cosine similarity |
| UI | Gradio |
| Data Handling | Pandas, NumPy |
| Language | Python 3 |

---

## ⚙️ Setup & Installation

```bash
# Clone the repo
git clone https://github.com/retalali16/Search-Engine-Final-Project.git
cd Search-Engine-Final-Project

# Install dependencies
pip install selenium webdriver-manager gradio sentence-transformers scikit-learn nltk pandas numpy

# Download NLTK data (run once)
python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords'); nltk.download('wordnet'); nltk.download('punkt_tab')"
```

> **Note:** Selenium requires Google Chrome installed. On Colab, the notebook includes setup commands for headless Chrome.

---

## 🧪 Usage

Open `basic_search_engine.ipynb` in Jupyter or Google Colab and run cells sequentially:

1. **Scraping** — Populates `bbc_scraped_news.csv`
2. **Preprocessing** — Generates `preprocessed_news.csv`
3. **Index + Ranking** — Builds inverted index; runs TF-IDF and BM25 on sample queries
4. **Query Expansion** — Tests expanded queries with Sentence-BERT
5. **Gradio UI** — Launches the interactive search interface (generates a public share link on Colab)

**Example query:**
```
Enter your search query: climate change
→ Expanded Query: climate change environment policy global ...
→ Result 1: https://www.bbc.com/news/... | Score: 4.27
```

---

## 📊 Evaluation Results

| Metric | Value |
|--------|-------|
| Retrieval | Inverted index + TF-IDF / BM25 |
| Query Expansion | Sentence-BERT relevance feedback |
| Precision | Computed per query |
| Recall | Computed per query |
| Search Latency | Measured per query (seconds) |

---

## 📄 Report

A PDF report covering each phase of implementation was submitted alongside the source code as part of the course requirements.

---

## 👩‍💻 Author

**Retal Ashraf Ali**
- 📧 s-retal.ali@zewailcity.edu.eg
- 💼 [LinkedIn](https://linkedin.com/in/retal-ashraf-ali-a42454311)
- 🐙 [GitHub](https://github.com/retalali16)
