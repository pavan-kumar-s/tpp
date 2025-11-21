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
Thanks for pointing that out, we will update the notation used for the cosine similarity

**Questions-2:**
In BrowseNet, the retrieval over the knowledge graph during multi-hop questions follows a principled graph traversal guided by the decomposed query-subgraph. For non-initiator nodes in the query-subgraph, we consider all combinations of candidates retrieved from predecessor nodes and gather their neighbors as the candidate corpus for the next retrieval step. From this expanded candidate set, we score chunks semantically relative to the current subquery and select the top-k scoring subgraphs. This scoring and selection over multiple candidate subgraphs resembles a beam search that maintains a fixed-width set of best subgraph paths as retrieval proceeds along the query-subgraph in topological order. Thus, while the traversal is not beam search in the classic sequence generation sense, it analogously explores multiple hypotheses (candidate subgraphs) at each step, pruning them according to a scoring function to efficiently focus retrieval on promising reasoning paths.

**Action:** We will clarify this analogy to beam search in the paper and explicitly describe the candidate combination, scoring, and pruning process during subgraph retrieval to improve reader understanding.
