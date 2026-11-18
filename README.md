# Senior Living Email Triage

A staged machine learning project that reads sales and marketing emails from
senior living communities and classifies them by community type, with the goal
of extracting key details, routing them, and answering questions about them.

## The problem

Senior living professionals receive a high volume of promotional emails from
communities across the US and Canada. Sorting them by hand is slow and
inconsistent. This project builds a system that:

1. **Classifies** each email as Independent Living, Assisted Living,
   Memory Care, or Other/Mixed
2. **Extracts** details such as location, pricing, and offers
3. **Routes** emails to the right folder or person
4. **Answers questions** about the collection using retrieval-augmented generation

## Data

The dataset is **synthetic** (825 generated emails) and deliberately messy:
inconsistent label spellings, placeholder values, missing fields, duplicates,
US and Canadian formats, and uneven class sizes. No real emails or personal
information are used.

## Progress

| Stage | Focus | Status |
|-------|-------|--------|
| 1 | Data loading and cleaning | Done |
| 2 | Baseline models and evaluation | Planned |
| 3 | Neural network (PyTorch) | Planned |
| 4 | Embeddings and transformers | Planned |
| 5 | LLM comparison | Planned |
| 6 | RAG layer | Planned |
| 7 | API and deployment | Planned |

## Stage 1 summary

- Normalized 20 raw label variants into 4 classes
- Treated `N/A` placeholders as Other/Mixed after reviewing the emails
- Removed duplicate emails and set aside unlabeled rows
- Established a majority-class baseline for later models to beat
- Engineered `body_length`, `mentions_price`, and `month` features

## Project structure

```
data/raw/         original CSV (never edited)
data/processed/   cleaned data
notebooks/        one notebook per stage
```

## How to run

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Then open `notebooks/01_explore_and_clean.ipynb`.