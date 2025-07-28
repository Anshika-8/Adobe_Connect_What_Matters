# Adobe_Connect_What_Matters (Round 1B)
# Persona-Driven Document Intelligence

##  Challenge Theme
**“Connect What Matters — For the User Who Matters”**

Build a smart system that extracts and ranks the most relevant content from a set of documents based on a specific persona and a concrete job-to-be-done.

---

##  Problem Statement
Given:
- A **collection of related PDF documents** (3–10)
- A defined **persona** (e.g., Travel Planner, Researcher, Student)
- A specific **job-to-be-done** (e.g., Plan a trip, Prepare a review, Study for exams)

Your task is to:
- Extract the most relevant sections from the documents
- Prioritize and rank them using semantic similarity
- Format the result in a structured JSON format

##  Sample Use Case
**Persona:** Travel Planner  
**Job:** Plan a trip of 4 days for a group of 10 college friends.  
**Documents:** PDFs about cities, activities, restaurants, culture, tips, etc.

### Input
- PDFs in `sample_pdfs/`
- Persona and job defined in `main.py`

###  Output
A JSON file (`output.json`) with:
1. Metadata (documents, persona, job, timestamp)
2. Top relevant sections (title, page number, rank)
3. Sub-section analysis (refined text content)

## Components and Libraries

### `PyMuPDF (fitz)`
Used for parsing PDFs, extracting page content, section headers, font sizes, and layout-based heuristics.

### `sentence-transformers`
Used to generate semantic embeddings of the user query and document sections.

Model used:
- `all-MiniLM-L6-v2`         (Size < 100MB, CPU-friendly)

###  cosine_similarity
From `sentence_transformers.util`, used to compute the semantic similarity between the task and each section.

###  Heuristics
- Font size and alignment to identify section headings
- Boost keywords (`cities`, `activities`, `fun`, etc.) to prioritize task-relevant terms

# Test Case Exapmles
Persona	              Job	                                                               Documents
Travel Planner	      Plan a trip for 10 college friends	                               6 PDFs on South of France
Researcher          	Literature review on Graph Neural Networks for Drug Discovery    	 4 Research Papers
Investment Analyst	  Analyze R&D and revenue trends across tech competitors	           3 Annual Reports
Chemistry Student	    Study Organic Chemistry reaction kinetics                          5 Textbook Chapters
