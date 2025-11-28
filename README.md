# Knowledge Graph RAG with Local LLM
I have updated former knowledge-graph-rag with new langchain version using Neo4j and Azure service.

```
pip install -r requirements.txt
```

# The pipeline

pipeline.py -> main script to run the pipeline.

1) It extracts text from PDFs in the `files` folder.
2) Sends the text to the local LLM to extract entities and relationships.
* To use a I needed to build a custom chat_prompt, as pointed out in this [StackOverflow topic](https://stackoverflow.com/questions/78521181/llmgraphtransformer-convert-to-graph-documentsdocuments-attributeerror-str).
* I chose to also build my own Pydantic class and examples, instead of using the library's default, to align the model to the crime-related theme.
3) Inserts into the Neo4J database the extracted entities and relationships.

After running the pipeline script, check out the Neo4J database at `http://localhost:7474/browser/`:
```
MATCH (n)-[r]->(m)
RETURN n, r, m
```

You should see all the entities and relationships extracted from the PDFs.

Results using Llama3-8B model:

![result](./files/pipeline_result.png)


# The Graph RAG

graph_rag.py -> main script to run the Graph RAG Q&A.

1) It queries the Neo4J database with a natural language question.
2) It returns the answer in natural language based on the result of the query.

> Right now you need to write the questions using the same words as the entities and relationships in the database. I'm working on a way to make the questions more flexible...

Results using Llama3-8B model:

![result](./files/graph_rag_result.png)
