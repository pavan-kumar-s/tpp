# Reviewer 5j2x:

**Weakness-1:**
The presentation of this paper requires significant improvement. For example, the overall structure of the main content needs to be reorganized. Some key experiments, such as the ablation study, should be included in the main text rather than the appendix. Although the authors have added content in Section 3, its readability needs further polishing.

**Weakness-2:**
Some critical analysis are missing. For example, while BrowseNet shows improvements in knowledge retrieval, its performance on question answering drops, which is not adequately discussed in the paper.

**Comments Suggestions And Typos-1:**
The reliability of the Isomorphic accuracy metric needs to be further clarified.

**Comments Suggestions And Typos-2:**
Could you provide further clarification on how the datasets are partitioned in the experiments?

**Comments Suggestions And Typos-3:**
BrowseNet shows improvement in knowledge retrieval; however, why does its performance on question answering decrease? Please elaborate on the case of 2WikiMQA.

**Comments Suggestions And Typos-4:**
Further analysis is needed to explain why BrowseNet does not show significant improvement in overall question answering performance.

**Comments Suggestions And Typos-5:**
The implementation details are not provided in this submission, making it difficult to evaluate the backbone model used in BrowseNet.


# Reviewer BHPP: 
We thank the reviewer for their valuable feedback. We will revise the manuscript to incorporate all reviewer comments in the camera-ready submission.

**Weakness-1:**
W1: The results show that the graph constructed by the new method has certain improvements in its own performance and retrieval performance. Is it necessary to consider the difference in construction cost compared with the traditional method?



**Weakness-2:**
W2: More experiments needed (For example, using different LLMs).

We thank the reviewer for the suggestion. For the browseNet pipeline, the LLMs have been used at three distinct places: Keyword generation, Query decomposition, and Answer generation. In the manuscript, the effect of distinct LLMs on keyword generation has been summarized in the Tables-5,6,10 in terms of Graph density, Edge accuracy, Retrieval recall, and No. of entities. Also, LLM contribution on query decomposition has been summarised in Tables-4,6 in terms of isomorphic accuracy and retrieval recall. Hereby, we summarize the results for the usage of distinct LLMs in answer generation in terms of exact match (EM) and F1-score (F1).

|    LLM    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| gpt-4o-mini  |  62.20  | 77.69   | 63.90   | 74.50  | 41.60  | 54.08    | 55.90   |  68.76  |
| gpt-3.5-turbo  | 58.80  | 73.81  | 47.70   | 59.57   | 37.40   | 49.77  |47.97    | 61.05   |
| gpt-4.1-mini | 63.20   | **79.21**   | 64.50   | 74.43   | 42.70   | 55.07   | 56.80   | 69.57   |
| deepseek-chat-v3   |62.20   | 78.91   | **66.10**   | **75.86**   |**43.50**   | **56.25**   | **57.27**   | **70.34**   |
| gemini-2.0-flash   | **63.40**   | 78.00  | 62.10   | 70.30   | 38.10   | 47.37   | 54.53   | 65.22   |

The results presented in the manuscipt uses gpt-4o-mini. The same input and prompt has been given to all the above mentioned LLMs. 


**Comments Suggestions And Typos-1:**
D1: As a compound noun, the correct hyphenation for "work of art" should be "work-of-art" rather than "work_of_art".


**Comments Suggestions And Typos-2:**
D2: It is inappropriate to have both "Equation + numeral" and "Eq. + numeral" following "As shown in". It is preferable to unify the format. Additionally, the numerals in both cases are not enclosed in parentheses.





# Reviewer 6bMr: 
We thank the reviewer for their valuable feedback. We will revise the manuscript accordingly to address all reviewer comments in the camera-ready submission.

**Weakness-1:**
Benchmarks are not multi-document QA. HotpotQA, 2WikiMQA and MuSiQue require reasoning across paragraphs drawn from Wikipedia, but all evidence ultimately resides in one monolithic source. To claim multi-document QA the system should be evaluated on datasets where answers require synthesising separate primary documents. For example, FanOutQA, WikiHowQA, Narrative QA, MultiDoc2Dial, VisDoMBench, etc.


We thank the reviewer for this valuable comment. We agree that datasets such as FanOutQA and MultiDoc2Dial better reflect scenarios where answers must be synthesized from truly distinct primary documents. In our work, we followed the terminology used in prior literature [1–3], where HotpotQA, 2WikiMQA, and MuSiQue have been widely referred to as multi-document QA benchmarks, primarily due to their requirement for reasoning across multiple passages, albeit sourced from a single repository like Wikipedia. We will clarify this distinction more explicitly in the revised version to avoid any potential confusion.

**References:**
1) Yoon, C., Lee, T., Hwang, H., Jeong, M., & Kang, J. (2024). Compact: Compressing retrieved documents actively for question answering. arXiv preprint arXiv:2407.09014.
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).
3) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI Conference on Artificial Intelligence (Vol. 38, No. 17, pp. 19206-19214).
Results are compared only with multi-hop-within-corpus retrievers (HippoRAG-2, KAG, etc.). Standard multi-document QA baselines are absent, such as Visconde, KGP, KGP’s variant (e.g., Curiousllm), etc.

**Weakness-2:**
Results are compared only with multi-hop-within-corpus retrievers (HippoRAG-2, KAG, etc.). Standard multi-document QA baselines are absent, such as Visconde, KGP, KGP’s variant (e.g., Curiousllm), etc.
################################################


**Weakness-3:**
Evaluation run on small 1000 subsets rather than full test sets.

We appreciate the reviewer’s observation. To manage the computational cost of large-scale experiments, we followed the common practice adopted in prior works [1–5] and evaluated our methods on a randomly sampled subset of 1,000 questions from each validation set. Notably, the state-of-the-art baselines we compare against HippoRAG [1,2] and KAG [4] also use the same subset of the dataset. We will clarify this detail in the revised version to avoid any ambiguity.

**References**
1) Gutiérrez, B. J., Shu, Y., Gu, Y., Yasunaga, M., & Su, Y. (2024, January). Hipporag: Neurobiologically inspired long-term memory for large language models. In The Thirty-eighth Annual Conference on Neural Information Processing Systems.
2) Gutiérrez, B. J., Shu, Y., Qi, W., Zhou, S., & Su, Y. (2025). From rag to memory: Non-parametric continual learning for large language models. arXiv preprint arXiv:2502.14802.
3) Trivedi, H., Balasubramanian, N., Khot, T., & Sabharwal, A. (2022). Interleaving retrieval with chain-of-thought reasoning for knowledge-intensive multi-step questions. arXiv preprint arXiv:2212.10509.
4) Liang, L., Bo, Z., Gui, Z., Zhu, Z., Zhong, L., Zhao, P., ... & Chen, H. (2025, May). Kag: Boosting llms in professional domains via knowledge augmented generation. In Companion Proceedings of the ACM on Web Conference 2025 (pp. 334-343).
Related work lacks multi-doc related and efficient RAG related research.
5) Press, O., Zhang, M., Min, S., Schmidt, L., Smith, N. A., & Lewis, M. (2022). Measuring and narrowing the compositionality gap in language models. arXiv preprint arXiv:2210.03350.

**Weakness-4:**
Related work lacks multi-doc related and efficient RAG related research.

#########################################################

**Weakness-5:**
While the paper claims lower LLM cost, there is no concrete token / dollar comparison against iterative baselines.

#########################################################

**Weakness-6:**
BrowseNet extends earlier KG-enhanced RAGs (GraphRAG, HippoRAG) mainly with query-specific traversal, which limits its novelty.

We thank the reviewer for highlighting the importance of distinguishing our approach from existing KG-enhanced RAG methods. While BrowseNet builds upon the general idea of incorporating knowledge graphs into retrieval-augmented generation, it introduces several key innovations that set it apart from both GraphRAG and HippoRAG:

**GraphRAG Differences:**
1) Graph Construction: GraphRAG constructs hierarchical community-based graphs and generates summaries at the community level. In contrast, BrowseNet builds a flat, lexically-connected graph without requiring hierarchical clustering, allowing for finer-grained and dynamic retrieval.
2) Indexing Efficiency: GraphRAG involves LLM-based entity and relationship extraction during indexing, which is computationally expensive. BrowseNet relies on lightweight NER and similarity-based linking, offering a more efficient pipeline.
3) Retrieval Strategy: GraphRAG uses precomputed community summaries for global retrieval. BrowseNet, however, dynamically constructs query-specific subgraphs and performs targeted multi-hop traversal at query time.

**HippoRAG Differences:**
1) Graph Schema: HippoRAG employs schemaless KGs using OpenIE triples and synonymy-aware retrieval encoders. BrowseNet, in contrast, uses structured lexical relationships and leverages ColBERTv2-based linking for more precise entity disambiguation.
2) Traversal Mechanism: HippoRAG performs single-step Personalized PageRank traversal, while BrowseNet executes multi-step beam search with topological constraints, enabling deeper and more targeted reasoning.
3) Pipeline Simplicity: HippoRAG requires both NER and relation extraction, whereas BrowseNet simplifies the pipeline by requiring only NER, reducing annotation and inference complexity.

