# Crypto-Insight Engine: An LLM-Powered Data Extraction Pipeline
[View the interactive workflow diagram here](https://honcyeung.github.io/Extract-Crypto-White-Paper-Information-with-LLM/)

This project is a Python-based data engineering tool designed to ingest cryptocurrency whitepapers in PDF format, process them using a Large Language Model (LLM), and extract structured, high-quality data.
The output is specifically formatted to be ready for ingestion by an automated ETL/ELT orchestrator like Azure Data Factory (ADF) for loading into a NoSQL database. This project demonstrates best practices in data modeling, pipeline-friendly data formats, and creating configurable, production-ready code.

## Key Features

- Structured Data Extraction: Leverages LangChain and Pydantic to transform unstructured text from PDFs into a strictly-enforced, structured schema.
- Data Contract Enforcement: Utilizes Pydantic models as a "data contract" to ensure all extracted data is consistent, validated, and self-documenting.
- Pipeline-Ready Output: Generates data in the JSON format, which is highly efficient for streaming and batch processing in big data systems.
- Configurable & Reusable: The main notebook is parameterized using argparse, allowing it to be executed as a reusable tool within an automated workflow.
- Robust Processing: Implements comprehensive error handling and logging to ensure reliability.

## Tech Stack

- Language: Python 3.9+
- LLM Orchestration: LangChain
- Data Validation & Schemas: Pydantic
- PDF Processing: pdfplumber
- Orchestration Target: Azure Data Factory (ADF)
- Storage Target: NoSQL Databases (e.g., Azure Cosmos DB, MongoDB)

## Proposed ADF Pipeline Integration

This project is designed as a local data processing stage that produces artifacts ready for a cloud-based ETL pipeline. The output of this script is designed to be seamlessly picked up by an orchestrator like ADF.

### Pipeline Diagram:
```text
[Local Machine]                            [Azure Cloud]
+--------------------------+           +--------------------------------+
| 1. Run Python File       |           | 3. ADF Pipeline Triggered      |
|    (main.ipynb)          |           |    (e.g., on a schedule)       |
+--------------------------+           +--------------------------------+
              |                                         |
              v                                         v
+--------------------------+           +--------------------------------+
| 2. Generates output.json |  upload-> | 4. Azure Blob Storage          |
|    (Pipeline-Ready Data) |           |    (Landing Zone)              |
+--------------------------+           +--------------------------------+
                                                        |
                                                        v
                                       +--------------------------------+
                                       | 5. ADF Copy Activity           |
                                       |    (Parses .json)              |
                                       +--------------------------------+
                                                        |
                                                        v
                                       +--------------------------------+
                                       | 6. Azure Cosmos DB             |
                                       |    (NoSQL Storage)             |
                                       +--------------------------------+
```
