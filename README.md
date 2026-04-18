# meddiagnose-knowledge

Disease knowledge base for the MedDiagnose platform. Contains medical reference PDFs, knowledge graphs, and indexed content used for RAG-style diagnosis improvement.

## Contents

```
pdf/                                  # Disease-specific reference PDFs (Wikipedia/medical sources)
  achalasia.pdf
  acute_coronary_syndrome.pdf
  adhd.pdf
  ...
aiims_syllabus/
  Syllabus - md ms mds mha.pdf       # AIIMS postgraduate medical syllabus
  syllabus_text.txt                   # Extracted text from syllabus PDF
knowledge_index.json                  # Indexed text chunks from Wikipedia disease content
knowledge_index_aiims.json            # Indexed text chunks from AIIMS/StatPearls content
knowledge_index_aiims_syllabus.json   # Structured AIIMS curriculum index
knowledge_graph.json                  # Disease-symptom-treatment graph
```

## How It Works

The **Disease Knowledge Brain** in the backend loads these indexes at startup and uses them for RAG (Retrieval-Augmented Generation):

1. Patient describes symptoms
2. Knowledge brain scores relevance of each disease entry against the symptoms
3. Top matching disease references are injected into the MedGemma prompt
4. AI diagnosis is grounded in medical reference content, improving accuracy

## Building the Knowledge Base

Use the scripts in [meddiagnose-api](https://github.com/AngadBindra46/meddiagnose-api):

```bash
cd meddiagnose-api

# 1. Download disease content from Wikipedia
python scripts/download_disease_books.py -n 80

# 2. Download AIIMS/StatPearls content
python scripts/download_disease_books.py --source aiims -n 80

# 3. Download AIIMS syllabus
python scripts/download_aiims_syllabus.py

# 4. Build the knowledge graph (merges all sources)
python scripts/build_knowledge_graph.py --merge-aiims
```

## Knowledge Graph Format

`knowledge_graph.json` contains structured relationships:

```json
{
  "diseases": {
    "Diabetes Mellitus Type 2": {
      "symptoms": ["polyuria", "polydipsia", "fatigue", "blurred vision"],
      "treatments": ["Metformin", "lifestyle modification", "insulin therapy"],
      "causes": ["insulin resistance", "genetic predisposition", "obesity"],
      "description": "..."
    }
  }
}
```

## Sources

1. **Wikipedia** (default): General disease summaries
2. **AIIMS/StatPearls**: Clinical reference material from NIH/NCBI, used in Indian medical colleges

## Related Repos

- [meddiagnose-api](https://github.com/AngadBindra46/meddiagnose-api) -- Backend API (consumes this knowledge base)
- [meddiagnose-ml](https://github.com/AngadBindra46/meddiagnose-ml) -- ML inference library
