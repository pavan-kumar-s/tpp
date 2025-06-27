# Reviewer 6bMr: 
We thank the reviewer for the useful comments. We will revise the manuscript that incorporates all reviewers’ comments while submitting for Camera-ready papers

**Weakness-1:**
Benchmarks are not multi-document QA. HotpotQA, 2WikiMQA and MuSiQue require reasoning across paragraphs drawn from Wikipedia, but all evidence ultimately resides in one monolithic source. To claim multi-document QA the system should be evaluated on datasets where answers require synthesising separate primary documents. For example, FanOutQA, WikiHowQA, Narrative QA, MultiDoc2Dial, VisDoMBench, etc.


We thank the reviewer for this valuable comment. We agree that datasets such as FanOutQA and MultiDoc2Dial better reflect scenarios where answers must be synthesized from truly distinct primary documents. In our work, we followed the terminology used in prior literature [1–3], where HotpotQA, 2WikiMQA, and MuSiQue have been widely referred to as multi-document QA benchmarks, primarily due to their requirement for reasoning across multiple passages, albeit sourced from a single repository like Wikipedia. We will clarify this distinction more explicitly in the revised version to avoid any potential confusion.

**References:**
1) Yoon, C., Lee, T., Hwang, H., Jeong, M., & Kang, J. (2024). Compact: Compressing retrieved documents actively for question answering. arXiv preprint arXiv:2407.09014.
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).
3) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI Conference on Artificial Intelligence (Vol. 38, No. 17, pp. 19206-19214).
Results are compared only with multi-hop-within-corpus retrievers (HippoRAG-2, KAG, etc.). Standard multi-document QA baselines are absent, such as Visconde, KGP, KGP’s variant (e.g., Curiousllm), etc.

**Weakness-2:**
Evaluation run on small 1000 subsets rather than full test sets.

We appreciate the reviewer’s observation. To manage the computational cost of large-scale experiments, we followed the common practice adopted in prior works [1–5] and evaluated our methods on a randomly sampled subset of 1,000 questions from each validation set. Notably, the state-of-the-art baselines we compare against HippoRAG [1,2] and KAG [4] also use the same subset of the dataset. We will clarify this detail in the revised version to avoid any ambiguity.

**References**
1) Gutiérrez, B. J., Shu, Y., Gu, Y., Yasunaga, M., & Su, Y. (2024, January). Hipporag: Neurobiologically inspired long-term memory for large language models. In The Thirty-eighth Annual Conference on Neural Information Processing Systems.
2) Gutiérrez, B. J., Shu, Y., Qi, W., Zhou, S., & Su, Y. (2025). From rag to memory: Non-parametric continual learning for large language models. arXiv preprint arXiv:2502.14802.
3) Trivedi, H., Balasubramanian, N., Khot, T., & Sabharwal, A. (2022). Interleaving retrieval with chain-of-thought reasoning for knowledge-intensive multi-step questions. arXiv preprint arXiv:2212.10509.
4) Liang, L., Bo, Z., Gui, Z., Zhu, Z., Zhong, L., Zhao, P., ... & Chen, H. (2025, May). Kag: Boosting llms in professional domains via knowledge augmented generation. In Companion Proceedings of the ACM on Web Conference 2025 (pp. 334-343).
Related work lacks multi-doc related and efficient RAG related research.
5) Press, O., Zhang, M., Min, S., Schmidt, L., Smith, N. A., & Lewis, M. (2022). Measuring and narrowing the compositionality gap in language models. arXiv preprint arXiv:2210.03350.

**Weakness-3:**
While the paper claims lower LLM cost, there is no concrete token / dollar comparison against iterative baselines.

**Weakness-4:**
BrowseNet extends earlier KG-enhanced RAGs (GraphRAG, HippoRAG) mainly with query-specific traversal, which limits its novelty.

**GraphRAG Differences:**
1) GraphRAG constructs hierarchical community structures and generates community summaries, while BrowseNet builds lexically-connected graphs without hierarchical clustering
2) GraphRAG requires extensive LLM processing during indexing for entity and relationship extraction, whereas BrowseNet uses lightweight NER and similarity-based linking
3) GraphRAG's retrieval relies on community summaries for global search, while BrowseNet performs dynamic subgraph traversal

**HippoRAG Differences:**
1) HippoRAG uses schemaless knowledge graphs with Open Information Extraction and synonymy linking via retrieval encoders, while BrowseNet employs structured lexical relationships with ColBERTv2-based entity linking
2) HippoRAG's retrieval mechanism centers on Personalized PageRank for single-step traversal, whereas BrowseNet implements multi-step beam search with topological ordering
3) HippoRAG requires both NER and relation extraction, while BrowseNet needs only NER, reducing complexity
