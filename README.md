Graph RAG for Incident Management using Neo4j Aura & LangChain
==============================================================

This repository demonstrates how to build an **LLM-powered Graph RAG pipeline** for **DevOps/SRE incident analysis**, using:

-   **Neo4j Aura** (graph database)

-   **LangChain**

-   **Groq LLMs**

-   **LLMGraphTransformer** for text → graph extraction

-   **GraphCypherQAChain** for natural-language querying

It shows how raw incident reports (from Jira/Slack) can be turned into a **knowledge graph**, enabling powerful graph-based retrieval and reasoning.

<img width="1429" height="674" alt="image" src="https://github.com/user-attachments/assets/0ba55ff7-9207-4d14-91ae-e7a51d9213e6" />

<img width="1180" height="614" alt="image" src="https://github.com/user-attachments/assets/c60a9e41-15ed-4ad5-acb2-e3c39b546a44" />


* * * * *

🧠 What is a Graph Database?
============================

A **graph database** is a database designed to store and query information in the form of:

-   **Nodes** (entities: services, incidents, engineers)

-   **Relationships** (incident → handled by → engineer)

-   **Properties** on both nodes and edges

Unlike relational databases (tables) or document stores (JSON), graph databases treat **relationships as first-class citizens**.

### Example graph structure
```
(Incident)-[:HAS_SEVERITY]->(Severity)
(Incident)-[:HANDLED_BY]->(Engineer)
(Service)-[:HAS_INCIDENT]->(Incident)
```
<img width="1021" height="486" alt="image" src="https://github.com/user-attachments/assets/ab7c6f63-3441-4e32-96d7-21c12c7069ba" />

A graph database like **Neo4j** is optimized to answer questions like:

-   "Which services had repeated P1 incidents?"

-   "Which engineers handled the most incidents this quarter?"

-   "What are recurring root causes across incidents?"

These are difficult questions for traditional SQL because they require **multi-hop relationship traversal**.

* * * * *

🚀 Why Use a Graph Database?
============================

Graph databases shine when your data forms **networks**, not tables.

✔ 1. Natural modeling of complex relationships
----------------------------------------------

Incidents are connected to:\
services → root cause → engineer → change event → severity → timelines

Trying to represent this in SQL leads to dozens of JOINs.

Graph DBs do this naturally with pattern matching.

* * * * *

✔ 2. Fast multi-hop queries
---------------------------

Typical SQL query:

> "Find the top 3 engineers who handled incidents in services that depend on payment-service."

SQL → Expensive, JOIN-heavy\
Graph → Simple:
```
MATCH (s:Service {name: 'payment-service'})<-[:DEPENDS_ON]-
      (other:Service)-[:HAS_INCIDENT]->(i:Incident)-[:HANDLED_BY]->(e:Engineer)
RETURN e, count(*) as incidents
ORDER BY incidents DESC LIMIT 3
```

* * * * *

✔ 3. Perfect foundation for LLM reasoning
-----------------------------------------

LLMs often hallucinate when asked fact-based questions.\
Graphs give them a **ground-truth, queryable knowledge base**.

* * * * *

✔ 4. Ideal for semi-structured, evolving data
---------------------------------------------

Incidents evolve:

-   new services

-   new relationships

-   new metadata

-   new severity types

Graph schemas are **flexible** and easy to extend.

* * * * *

🔍 What is Graph RAG?
=====================

**Graph RAG (Graph Retrieval-Augmented Generation)** is the next evolution of RAG.

Traditional RAG retrieves **text chunks** using embeddings.

Graph RAG retrieves **structured knowledge** using graph traversal and relationships.

* * * * *

✔ How Graph RAG works
---------------------

### **Ingestion**

1.  Take unstructured text (incident report, Slack message)

2.  Use LLM to extract:

    -   entities → nodes

    -   relationships → edges

3.  Store them in Neo4j

### **Query-time**

1.  User asks:\
    "Which services had P1 incidents handled by Alice?"

2.  LLM translates question → Cypher query

3.  Neo4j executes the query (zero hallucination)

4.  LLM summarizes the result to natural language

* * * * *

🆚 RAG vs Graph RAG
===================

| Feature | Traditional RAG | Graph RAG |
| --- | --- | --- |
| Retrieval | Semantic search (vector similarity) | Graph traversal (Cypher) |
| Data format | Documents / text chunks | Nodes, relationships, properties |
| Strengths | Good for long documents | Excellent for structured knowledge & relationships |
| Weaknesses | Easy to miss relational context; hallucination | Requires graph data but gives precise answers |
| Typical questions | "What does this document say about X?" | "How is X connected to Y?" "What pattern exists across Z?" |
| Best use cases | Q&A over documents | Incident analysis, knowledge graphs, fraud detection, dependency mapping |

**RAG ≠ Graph RAG.**\
Graph RAG is **retrieval based on relationships**, not similarity.

* * * * *

📊 Why Graph RAG for Incident Management?
=========================================

Incidents have complex, relational data:

-   Services

-   Engineers

-   Root causes

-   Deployments

-   Dependencies

-   Severity

-   Timelines

You can ask powerful questions like:

-   "Which services had multiple P1 incidents in the last month?"

-   "Which engineer handled incidents with the same root cause?"

-   "What service dependencies correlate with user-facing failures?"

These require **relational reasoning**, which plain text RAG cannot do.

* * * * *

🏗 Project Structure
====================
```
├── graph_rag_neo4j.ipynb   # Main notebook
├── README.md                             # This file
└── requirements.txt (optional)
```

* * * * *

🔥 Features Demonstrated
========================

**1\. Create a Neo4j Aura incident graph using Cypher**\
Services → Incidents → Engineers

**2\. Convert unstructured incident reports to graph objects**\
Using:

`LLMGraphTransformer`

**3\. Natural language → Cypher → Neo4j → Answer**\
Using:

`GraphCypherQAChain`

**4\. Comparison of RAG vs Graph RAG**

**5\. Full explanation for beginners**

* * * * *

🧩 Key Technologies
===================

-   **Neo4j Aura** (cloud graph DB)

-   **LangChain**

-   **Groq LLMs (LLaMA 3.3 / Gemma)**

-   **Python, Jupyter**

* * * * *

🎯 Suggested Use Cases Beyond Incidents
=======================================

-   CMDB automation

-   Service dependency mapping

-   Fraud detection

-   Security posture analysis

-   Knowledge management

-   Root cause pattern mining
