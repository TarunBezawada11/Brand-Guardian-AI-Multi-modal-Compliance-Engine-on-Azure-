# Brand Guardian AI

A multi-modal compliance engine that audits video content against brand guidelines using AI.

## The Problem

Brands need to ensure their video content complies with advertising regulations and internal guidelines. Doing this manually is slow, inconsistent, and doesn't scale. A single video can contain spoken claims, on-screen text, and visual elements that all need to be checked against compliance rules.

## The Approach

Brand Guardian takes a YouTube video URL, downloads and processes it through Azure Video Indexer to extract transcripts (speech-to-text), OCR text, and metadata. These are passed through a RAG retrieval layer that embeds queries and runs similarity search against a knowledge base of compliance rules stored in Azure AI Search. The retrieved rules are then sent to a reasoning layer (GPT-4o) that evaluates the video content against each rule and outputs structured compliance results — pass/fail status, violations by severity and category, and a summary audit report.

The entire pipeline is orchestrated using LangGraph, with LangSmith for tracing and Azure Application Insights for observability.

## Architecture

![Brand Guardian Architecture](https://github.com/Brand_Guardian/ComplianceQAPipeline/BrandArch.jpeg)

## Tech Stack

- **Backend:** Python, FastAPI
- **Orchestration:** LangGraph, LangChain
- **Cloud:** Azure (Blob Storage, Video Indexer, AI Search, OpenAI)
- **Models:** GPT-4o, text-embedding-3-small
- **Retrieval:** RAG with Azure AI Search (Vector Store)
- **Observability:** Azure Application Insights, LangSmith Tracing
- **Deployment:** Docker

## How It Works

1. **Input** - YouTube URL and Video ID
2. **Ingestion** - Video downloaded via yt-dlp, uploaded to Azure Video Indexer
3. **Extraction** - Transcript, OCR text, and metadata extracted
4. **Retrieval** - Compliance rules retrieved via vector similarity search (k=3)
5. **Reasoning** - GPT-4o evaluates content against retrieved rules
6. **Output** - Pass/Fail status, violations, and audit report in markdown
