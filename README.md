# ragatouille

Retrieval-augmented generation needs a way to find passages that answer a query. ragatouille makes ColBERT retrieval in Python easier to use, with document indexing, search, and training-data preparation for late-interaction models.

## Features

- Index document text with a pretrained ColBERT model for use in a RAG pipeline.
- Search an index for passages relevant to a query.
- Prepare retrieval training data from pairs, labelled pairs, or triplets with `RAGTrainer.prepare_training_data()`.
- Remove duplicate training examples and mine hard negatives during data preparation.
- Reuse data-processing and negative-mining components outside the main model and trainer interfaces.

## Installation

Install the Python package:

```bash
python -m pip install ragatouille
```

The donor project lists Python 3.9–3.11. Native Windows is unsupported; some users have reported success with WSL2.

For a standalone release archive, replace `OWNER` below with this project's GitHub owner. This command assumes the release publishes an asset named `ragatouille-v0.1.0.tar.gz`; the supplied project information does not confirm that an archive has been published.

```bash
curl -fL -o ragatouille-v0.1.0.tar.gz https://github.com/OWNER/ragatouille/releases/download/v0.1.0/ragatouille-v0.1.0.tar.gz
```

## Usage

You can start with a pretrained model without training your own. Save this example as `search.py`:

```python
from ragatouille import RAGPretrainedModel


def main():
    model = RAGPretrainedModel.from_pretrained("colbert-ir/colbertv2.0")

    documents = [
        "ColBERT is a late-interaction model for document retrieval.",
        "RAG pipelines retrieve passages to provide context for generation.",
        "Hard negatives are non-relevant examples that resemble relevant ones.",
    ]

    model.index(collection=documents, index_name="example-docs")
    results = model.search(query="What are hard negatives?", k=2)

    for result in results:
        print(result)


if __name__ == "__main__":
    main()
```

Run it with:

```bash
python search.py
```

**Keep the `if __name__ == "__main__"` guard when running scripts.** Indexing creates a local index, and the pretrained model must be downloaded on first use.

For training-data preparation, pass query–passage pairs to the trainer:

```python
from ragatouille import RAGTrainer


if __name__ == "__main__":
    trainer = RAGTrainer()
    trainer.prepare_training_data(
        raw_data=[
            ("What is late interaction?", "Late interaction compares token-level representations."),
            ("What does a retriever do?", "A retriever finds documents relevant to a query."),
        ]
    )
```

This prepares training data; it does not train a model. Use a representative dataset for actual fine-tuning.

## License

Licensed under Apache-2.0. This project is based on [AnswerDotAI/RAGatouille](https://github.com/AnswerDotAI/RAGatouille). Retain applicable upstream copyright and attribution notices, include the license when redistributing, and mark modified files as required by that license.

## Related projects

Other repositories supplied as projects from the same author:

- [khuram-bti/taskomask](https://github.com/khuram-bti/taskomask): A task-management application.
- [037L/docker-airflow](https://github.com/037L/docker-airflow): Docker packaging for Apache Airflow.
- [maxwildcat/vulcan-sql](https://github.com/maxwildcat/vulcan-sql): A tool for building data APIs from SQL.

