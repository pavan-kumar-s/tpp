# Reviewer Jw6M:
We thank the reviewer for their valuable feedback. Our response to the comments are as follows

**Weakness-1:**
The graph constructed probably should not be called a "knowledge graph" -- usually in the context of information extraction a KG has entities as nodes and predicates as edges. The proposal here is a graph on chunks of text (which I agree that is a better format for downstream RAG than a traditional 3-tuple KG)

We recognize that a traditional knowledge graph involves entities as nodes and predicates as edges, while our graph is constructed over chunks of text, which suits retrieval-augmented generation tasks better. We initially followed prior work using the term "knowledge graph" [1][2], but upon reflection, we agree the term could be clearer. We will revise the manuscript to replace "knowledge graph" with "graph of chunks" wherever appropriate


**Action**
We will revise the manuscript to replace all instances of "knowledge graph" with "graph of chunks" to improve clarity and accuracy.

References:
1) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI conference on artificial intelligence (Vol. 38, No. 17, pp. 19206-19214).
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).
**Action:** 
