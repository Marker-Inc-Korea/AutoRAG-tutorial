# AutoRAG-template
Template for a new AutoRAG project


# Installation

```bash
pip install -r requirements.txt
```

Running the above command will automatically install AutoRAG.

First, set the `OPENAI_API_KEY` environment variable directly or create a `.env` file and input it there.

# RAG Evaluation Dataset Creation Tutorial

To use AutoRAG, you first need to create a RAG evaluation dataset. Follow the steps below to create and use the dataset yourself.

1. Check the original documents in `raw_docs`. In this tutorial, we will use three PDF documents.
2. Run `run_parse.py`. This file allows you to execute parsing methods specified in `config/parse.yaml` and compare the results.
```bash
python make_parse.py 
```
3. In the `parsed_raw` folder, you will find several parquet files within the trial folder (numbered folders). These are the parsed results. Load them using `pandas` to inspect them directly.
4. Execute `run_chunk.py` to perform chunking using various methods. You can check the chunking methods in `config/chunk.yaml`. You need to set the raw file at this point.
```bash
python run_chunk.py --raw_path ./parsed_raw/0/2.parquet
```
5. After execution, check the `chunked_corpus` folder for the various chunked files created using different chunking methods.
6. Now, run the `make_qa.py` file. You need to set the raw file used for chunk creation and the chunk file to be used. Choose an appropriate chunk file, and you can generate a QA dataset using other chunk files later. You don't need to generate questions again. Refer to the update_corpus feature explained later.
```bash
python make_qa.py --raw_path ./parsed_raw/0/5.parquet --chunk_path ./chunked_corpus/0/3.parquet --qa_size 5
```
7. Check the `generated_qa.parquet` and `generated_corpus.parquet` files created in the `data` folder.

# Running the Project
## Using main.py

1. Copy the `.env.template` file to create a `.env` file and save it. Be sure to input your OpenAI API key in this file.
2. Run main.py as shown below to start AutoRAG.
```bash
python3 main.py --config ./config/tutorial.yaml
```
3. Once the benchmark folder is created, you can check the results there.

## Using CLI

1. Create a `benchmark` folder.
2. Set the `OPENAI_API_KEY` as an environment variable. `export OPENAI_API_KEY=sk-xxxx`
3. Execute the CLI command below to start AutoRAG optimization.
```bash
autorag evaluate --qa_data_path ./data/qa.parquet --corpus_data_path ./data/corpus.parquet \
  --config ./config/tutorial.yaml --project_dir ./benchmark
```
To run with the dataset created in the dataset tutorial, replace `corpus.parquet` with `corpus_new.parquet` and `qa.parquet` with `qa_new.parquet`.
4. Once the benchmark folder is created, you can check the results there.

## Running the Dashboard

Run the command below to load the dashboard. You can easily review the results through the dashboard.

```bash
autorag dashboard --trial_dir ./benchmark/0
```

## Running Streamlit

Run Streamlit to directly use the optimized RAG. Execute the command below.

```bash
autorag run_web --trial_path ./benchmark/0
```

## Update Corpus

This feature allows you to generate new QA files based on chunk corpora using the same raw file.

Try it as shown below.

```python
from autorag.data.qa.schema import Raw, Corpus, QA

raw = Raw(initial_raw_df)
corpus = Corpus(initial_corpus_df, raw)
qa = QA(initial_qa_df, corpus)

new_qa = qa.update_corpus(Corpus(new_corpus_df, raw))
```
