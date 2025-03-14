# Class 4: Implementing Retrieval-Augmented Generation (RAG)

## Assignment (15 points)

For this assignment, you need to submit:

1. **Vector Database Setup**:
   - Documentation of your vector database setup (e.g., Pinecone, Weaviate, or Supabase with pgvector)
   - Configuration details and connection setup
   - Schema design for storing embeddings
   - Evidence of successful connection and operations

2. **Document Embeddings Implementation**:
   - Code/workflow for generating document embeddings
   - Process for chunking documents and creating embeddings
   - Storage of embeddings in your vector database
   - Sample documents that were processed

3. **Query Processing and Retrieval**:
   - Implementation of query embedding and similarity search
   - Integration with your chatbot workflow
   - Evidence of successful retrieval with sample queries
   - Analysis of retrieval accuracy and relevance

## Submission Format

Create a folder with your name or team name in this directory and include:

```
class-4-rag-implementation/
└── your-name-or-team-name/
    ├── README.md (overview of your submission)
    ├── vector-database-setup.md (or .pdf)
    ├── document-embeddings.md (or .pdf)
    ├── query-processing.md (or .pdf)
    ├── sample-documents/ (folder containing sample knowledge base documents)
    ├── code/ (folder containing any custom code written)
    ├── workflow-exports/ (folder containing updated N8N workflow JSON files)
    └── screenshots/ (folder containing all testing screenshots)
```

## Grading Criteria

- Proper setup of vector database for RAG (5 points)
- Successful implementation of document embeddings (5 points)
- Effective query processing and retrieval with evidence of accuracy (5 points)

This assignment is critical for enhancing your chatbot's knowledge capabilities, so ensure your RAG implementation effectively retrieves relevant information.
