# Disease Knowledge Books

This folder contains disease-specific medical reference content used by the **Disease Knowledge Brain** to improve diagnosis accuracy.

## Structure

- **`pdf/`** — PDF files for each disease (viewable)
- **`pdf/aiims/`** — AIIMS-style study material from StatPearls (NCBI)
- **`knowledge_index.json`** — Text index used by the brain for RAG-style retrieval

## Sources

1. **Wikipedia** (default): `python scripts/download_disease_books.py`
2. **AIIMS/StatPearls**: `python scripts/download_disease_books.py --source aiims -n 40`

StatPearls is a free NIH/NCBI resource used in Indian medical colleges including AIIMS.

## Regenerating Content

```bash
cd backend
python scripts/download_disease_books.py              # Wikipedia
python scripts/download_disease_books.py --source aiims -n 40   # AIIMS-style
```

## Testing Accuracy

```bash
cd backend
REGRESSION_SAMPLE_SIZE=20 PYTHONUNBUFFERED=1 python -m tests.regression_test_medgemma
```
