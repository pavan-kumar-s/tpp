# Reviewer Jw6M:
We thank the reviewer for their valuable feedback. Our response to the comments are as follows

**Weakness-1:**
The graph constructed probably should not be called a "knowledge graph" -- usually in the context of information extraction a KG has entities as nodes and predicates as edges. The proposal here is a graph on chunks of text (which I agree that is a better format for downstream RAG than a traditional 3-tuple KG)

We recognize that a traditional knowledge graph involves entities as nodes and predicates as edges, while our graph is constructed over chunks of text, which suits retrieval-augmented generation tasks better. We initially followed prior work using the term "knowledge graph" [1][2], but upon reflection, we agree the term could be clearer. We will revise the manuscript to replace "knowledge graph" with "graph of chunks" wherever appropriate

**Action:**
We will revise the manuscript to replace all instances of "knowledge graph" with "graph of chunks" to improve clarity and accuracy.

**References:**
1) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI conference on artificial intelligence (Vol. 38, No. 17, pp. 19206-19214).
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).

**Weakness-2:**
The context is gathered with one shot -- maybe one could execute retrieval multiple times as the LLM executes the query subgraph it generates? As in a beam-search like process, the retrieved context could evolve as the LLM tries to solve the question (as in the style of https://aclanthology.org/2023.acl-long.557/)?

The core idea of BrowseNet is to minimize the number of LLM interactions during retrieval by leveraging a graph-of-chunks structure. Unlike IRCoT, which interleaves retrieval and reasoning by updating the query at each step over the entire corpus, BrowseNet restricts the retrieval candidates to neighbors in the graph. This focus reduces retrieval complexity and enables a one-shot retrieval approach by splitting the query, thus limiting LLM interaction to a single retrieval step. In contrast, IRCoT’s number of LLM interactions depends on query complexity and can be higher.

**Action:** We will clarify this distinction in the manuscript to highlight BrowseNet’s aim of efficient retrieval via one-shot interaction and graph-based candidate reduction, contrasting it with IRCoT’s iterative retrieval process.

**Weakness-3:**
A snippet subgraph of the constructed graph would be illuminating-- I suggest the authors present one in the final version.

**Action**
We will include an illustrative snippet subgraph in the final version of the paper, demonstrating the structure of the graph-of-chunks and how it relates to the decomposed query-subgraph. This visual aid will clarify the relationships between chunks and exemplify the retrieval process

**Questions-1:**
L195: $cos$ has a single argument: to denote cosine similarity, please write $\cos \angle (x, y)$.

Thanks for pointing that out, we will update the notation used for the cosine similarity

**Questions-2:**
L202-L220: Please clarify this search process on how this is related to beam search.

In BrowseNet, the retrieval over the knowledge graph during multi-hop questions follows a principled graph traversal guided by the decomposed query-subgraph. For non-initiator nodes in the query-subgraph, we consider all combinations of candidates retrieved from predecessor nodes and gather their neighbors as the candidate corpus for the next retrieval step. From this expanded candidate set, we score chunks semantically relative to the current subquery and select the top-k scoring subgraphs. This scoring and selection over multiple candidate subgraphs resembles a beam search that maintains a fixed-width set of best subgraph paths as retrieval proceeds along the query-subgraph in topological order. Thus, while the traversal is not beam search in the classic sequence generation sense, it analogously explores multiple hypotheses (candidate subgraphs) at each step, pruning them according to a scoring function to efficiently focus retrieval on promising reasoning paths.

**Action:** We will clarify this analogy to beam search in the paper and explicitly describe the candidate combination, scoring, and pruning process during subgraph retrieval to improve reader understanding.


# Reviewer Jw6M:
We thank the reviewer for their valuable feedback. Our response to the comments are as follows

**Weakness-1:**
The core idea focuses on context retrieval, but the novelty is limited since query decomposition and graph-based iterative retrieval have been explored previously; comparison to similar methods (e.g., SiReRAG, ArchRAG, GraphRAG) is missing.

We agree with the reviewer that the idea of query decomposition and graph-based iterative retrieval were explored previously, the novelty here is the pipeline that brings together the different these different aspects. On that note, we have compared with the existing methods, SiReRAG and GraphRAG as suggested by the reviewer. For the case of ArchRAG, the code has not been open-sourced yet by the authors, the anonymous link provided (https://anonymous.4open.science/r/H-CAR-AG-1E0B/) seems to have expired. Also, we have mailed the authors of the paper and we will benchmark our results against it once we have the codes. The results are given in the below table.

|    Method (K=5)  | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| Browsenet (gpt-4o-mini) |  62.20  | 77.69   | 63.90   | 74.50  | 41.60  | 54.08    | 55.90   |  68.76  |
| SiReRAG (gpt-4o-mini)  | 48.30  | 63.17  | 41.30   | 48.05   | 26.00   | 39.59  |   |    |
| GraphRAG (gpt-4o-mini) | 51.40   | 67.60   | 45.70   | 61.00   | 27.00   | 42.00   | 41.37   | 56.87   |

The answer generation model used for all the methods are gpt-4o-mini. Also, top-5 chunks were given for answer generation. In the main paper of SiReRAG, top-20 chunks were provided, hence for comparison with BrowseNet, we have implimented again with top-20 chunks and the results for that are shown below.

|    Method (K=20)   | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| Browsenet (gpt-4o-mini) | 62.20  | 78.36   | 68.2   | 78.47  | 44.50  | 57.77    |    |    |
| SiReRAG (gpt-4o-mini)  | 52.80  | 69.17  | 46.10   | 54.82   | 29.80   | 44.96  |   |    |


**Weakness-2:**
Scalability to large, real-world corpora and real-time use cases is not clearly addressed, as experiments rely on controlled corpora with gold evidence and distractors.

**Weakness-3:**
Fairness of comparison is unclear, particularly whether the same backbone models (generation, NER, embeddings) were used across baselines, which may confound reported improvements.

**Weakness-4:**
The current framework is optimized for structured multi-hop reasoning and may not directly generalize to open-domain retrieval or tasks with unstructured context dependencies.

BrowseNet is designed to be robust to varying query structures. When a query cannot be decomposed into subqueries, retrieval relies solely on initiator nodes, effectively performing a semantic search over the entire knowledge graph corpus. Additionally, during retrieval for each subquery, similarity scores are also computed between chunks and the original multi-hop query, allowing the system to maintain relevance to the full query context. We agree that BrowseNet requires a structured context for optimal performance, and that processing documents to induce such structure is beyond the current scope. However, BrowseNet only requires keyword prediction for constructing the graph-of-chunks, so converting from unstructured to structured context should be feasible. Exploring how performance improves by leveraging such structural preprocessing remains an interesting direction for future work.

**Action**: We will clarify BrowseNet’s robustness to query decomposition failure, its semantic search fallback, and the limitation regarding structured context construction in the manuscript, while noting the opportunity to investigate performance gains from unstructured-to-structured context conversion.

**Weakness-5:**
The approach depends on LLM-based query decomposition, which may introduce structural or semantic errors when sub-queries are misgenerated, leading to cascading retrieval failures.

We agree that the retrieval quality heavily depends on the correctness of the subgraph generated by LLM-based query decomposition. Structural or semantic errors in subquery generation can indeed lead to cascading failures during retrieval. A promising research direction is to explore generating multiple alternative decompositions of the query, either through prompting strategies or temperature variations in the LLM. Simultaneously searching along multiple decomposition paths could enhance robustness and reduce error propagation.


