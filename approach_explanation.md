# Approach Explanation: Persona-Driven Document Intelligence

## Problem Overview

The task is to build a generic system that extracts and prioritizes the most relevant sections from a collection of documents based on a defined *persona* and their *job-to-be-done*. This system must be adaptable across domains (e.g., research, business, travel), personas (e.g., student, analyst, planner), and task types (e.g., summarize, compare, plan).

---

## High-Level Approach

We developed an intelligent document analysis pipeline that:
1. Parses multiple PDFs and identifies candidate sections.
2. Scores each section based on semantic similarity to the persona and job context.
3. Ranks and selects top sections with diverse content coverage.
4. Extracts granular sub-section content for detailed analysis.
5. Outputs the results in a structured JSON format aligned with the challenge specification.

---

## Step-by-Step Breakdown

### 1. Section Extraction (PDF Parsing)
Using `PyMuPDF`, we iterate through each page of the input PDFs and identify potential section titles based on visual cues like font size and formatting. Once a heading is detected, we extract the following 2–3 paragraphs to serve as candidate content blocks for analysis.

### 2. Embedding & Semantic Matching
We use a lightweight transformer model (`all-MiniLM-L6-v2`, ~80MB) from `sentence-transformers` to generate embeddings:
- One embedding for the combined persona and task (e.g., *"Persona: Travel Planner. Task: Plan a trip of 4 days for 10 college friends."*).
- One for each candidate section’s content.

Using cosine similarity, we compute relevance scores between the persona query and each section.

### 3. Persona-Aware Boosting
To ensure alignment with the persona’s needs, we apply a small keyword-based boosting mechanism that favors content containing relevant travel-specific terms like “beach”, “nightlife”, “food”, or “activities”.

### 4. Section Ranking and Filtering
We sort all sections by similarity score and enforce document diversity in the top-K by selecting only one section per PDF. This ensures a well-rounded output covering various aspects relevant to the persona.

### 5. Output Formatting
The final output JSON includes:
- Metadata (documents, persona, task, timestamp)
- Top 5 extracted sections (title, source, page number, importance rank)
- Subsection analysis (cleaned paragraph content)

The format strictly follows the structure defined in `challenge1b_output.json`.

---

## Model & Execution Constraints

- The solution runs entirely on CPU within 60 seconds for 3–5 PDFs.
- Total model size < 100MB.
- No internet access is required at runtime.

---

## Generalizability

The system is domain-agnostic and easily adapts to any document type, persona, and job definition. Only the input persona/job description needs to be changed, and the system will automatically prioritize relevant sections accordingly using embeddings.

---

## Summary

This approach provides a scalable, lightweight, and general-purpose solution to the persona-driven document intelligence problem. It combines classical PDF parsing with semantic ranking using transformers, ensuring both interpretability and performance within constrained environments.

