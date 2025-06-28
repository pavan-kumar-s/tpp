# Reviewer 5j2x:
We thank the reviewer for their valuable feedback. We will revise the manuscript to incorporate all reviewer comments in the camera-ready submission.

**Weakness-1:**
The presentation of this paper requires significant improvement. For example, the overall structure of the main content needs to be reorganized. Some key experiments, such as the ablation study, should be included in the main text rather than the appendix. Although the authors have added content in Section 3, its readability needs further polishing.

We thank the reviewer for the constructive feedback regarding the presentation and organization of the paper. In the revised version, we will take the following steps to improve clarity and structure:
Reorganization of content: We will revisit the flow of the main sections to ensure a more coherent narrative and better separation between the method, implementation details, and evaluation results.
Ablation studies: We agree that the ablation studies provide important insights. Accordingly, we will move the most relevant ablation results from the appendix into the main text to highlight their significance.
We believe these revisions will significantly improve the presentation of our work.

**Weakness-2:**
Some critical analysis are missing. For example, while BrowseNet shows improvements in knowledge retrieval, its performance on question answering drops, which is not adequately discussed in the paper.

We thank the reviewer for this valuable observation. This feedback helped us investigate the gap between improved retrieval quality and answer generation performance more carefully.

Upon further analysis, we found that the quality of the final answer is significantly influenced by the choice of the LLM used in the answer generation stage of the pipeline. In the original submission, we used gpt-4o-mini for all QA outputs. To better understand this impact, we evaluated the system using different LLMs for the answer generation module, keeping the retrieval and decomposition stages fixed. The results are presented below:

|    LLM    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| gpt-4o-mini  |  62.20  | 77.69   | 63.90   | 74.50  | 41.60  | 54.08    | 55.90   |  68.76  |
| gpt-3.5-turbo  | 58.80  | 73.81  | 47.70   | 59.57   | 37.40   | 49.77  |47.97    | 61.05   |
| gpt-4.1-mini | 63.20   | **79.21**   | 64.50   | 74.43   | 42.70   | 55.07   | 56.80   | 69.57   |
| deepseek-chat-v3   |62.20   | 78.91   | **66.10**   | **75.86**   |**43.50**   | **56.25**   | **57.27**   | **70.34**   |
| gemini-2.0-flash   | **63.40**   | 78.00  | 62.10   | 70.30   | 38.10   | 47.37   | 54.53   | 65.22   |

These results indicate that while the retrieval and reasoning components provide high-quality evidence, the final QA performance is sensitive to the language model used. We observed nearly a 10-point difference in average F1 score between the best and least effective LLMs. We will include this analysis in the revised version to provide a more complete picture of the QA performance and clarify that the observed drop in some results was partly due to the LLM selection rather than limitations in retrieval quality.


**Comments Suggestions And Typos-1:**
The reliability of the Isomorphic accuracy metric needs to be further clarified.

We acknowledge that isomorphic accuracy, while helpful, does not fully capture the semantic correctness of a decomposed query. However, it serves as a useful proxy metric for evaluating the structural alignment between the predicted and reference query graphs. We would like to clarify that isomorphic accuracy primarily reflects how closely the structure of the decomposed subqueries matches the ground truth in terms of nodes and relation linkage. It is a strict, binary metric-even a single edge mismatch results in a score of zero, which makes it a conservative measure of structural accuracy. There can be cases where the predicted subquery is structurally similar to the gold standard but differs in semantics, or vice versa.

**Comments Suggestions And Typos-2:**
Could you provide further clarification on how the datasets are partitioned in the experiments.

We thank the reviewer for the opportunity to clarify this aspect of our experimental setup. To evaluate the multi-hop QA pipeline, we follow the standard practice used in prior works [1–5]. Specifically, we randomly sample 1,000 questions from the validation set of each of the three benchmark datasets: HotpotQA, 2WikiMultiHopQA, and MuSiQue. For each sampled question, we include both its supporting passages (which contain the information necessary to answer the question) and distractor passages (which are similar in content but do not contain the correct answer) to construct the corpus used for retrieval. This results in a corpus of: 9,221 passages for HotpotQA, 6,119 passages for 2WikiMultiHopQA, and 11,656 passages for MuSiQue. These statistics are reported in Table 1 of the main manuscript. We will revise the manuscript to make this setup more explicit and transparent in the experimental section.

**References**
1) Gutiérrez, B. J., Shu, Y., Gu, Y., Yasunaga, M., & Su, Y. (2024, January). Hipporag: Neurobiologically inspired long-term memory for large language models. In The Thirty-eighth Annual Conference on Neural Information Processing Systems.
2) Gutiérrez, B. J., Shu, Y., Qi, W., Zhou, S., & Su, Y. (2025). From rag to memory: Non-parametric continual learning for large language models. arXiv preprint arXiv:2502.14802.
3) Trivedi, H., Balasubramanian, N., Khot, T., & Sabharwal, A. (2022). Interleaving retrieval with chain-of-thought reasoning for knowledge-intensive multi-step questions. arXiv preprint arXiv:2212.10509.
4) Liang, L., Bo, Z., Gui, Z., Zhu, Z., Zhong, L., Zhao, P., ... & Chen, H. (2025, May). Kag: Boosting llms in professional domains via knowledge augmented generation. In Companion Proceedings of the ACM on Web Conference 2025 (pp. 334-343).
Related work lacks multi-doc related and efficient RAG related research.
5) Press, O., Zhang, M., Min, S., Schmidt, L., Smith, N. A., & Lewis, M. (2022). Measuring and narrowing the compositionality gap in language models. arXiv preprint arXiv:2210.03350.


**Comments Suggestions And Typos-3:**
BrowseNet shows improvement in knowledge retrieval; however, why does its performance on question answering decrease? Please elaborate on the case of 2WikiMQA.

We thank the reviewer for this insightful comment. To better understand the observed gap between retrieval quality and question answering performance, particularly in the case of 2WikiMQA, we conducted a detailed error analysis. After observing how QA performance varies with different LLMs (as shown in our updated experiments), we specifically analyzed the subset of questions where the retrieval recall was high, but the final answer accuracy was low. In particular: 103 questions had perfect retrieval recall (Recall = 1) but received an F1 score of 0; 88 additional questions had non-zero recall and still received an F1 score of 0.

Upon manually inspecting the generated answers for these questions, we found that in many cases, the answers were semantically correct but did not exactly match the ground truth string, resulting in a zero F1 score due to strict matching. For example:
Question: Which country is Aleksander Koniecpolski’s father from? Ground Truth: Polish-Lithuanian; Generated Answer: Poland

Question: What nationality is the performer of the song "When the Stars Go Blue"? Ground Truth: America; Generated Answer: American.

In both cases, the model provides reasonable and arguably correct answers, but they do not match the gold answer string verbatim. This highlights a limitation of using strict exact match and F1 as evaluation metrics for open-ended QA, particularly when synonyms, paraphrasing, or slight factual reinterpretations occur.

**Comments Suggestions And Typos-4:**
Further analysis is needed to explain why BrowseNet does not show significant improvement in overall question answering performance.

As discussed in our previous responses, we conducted additional analysis to investigate this issue. Specifically, we examined the impact of the LLM used in the answer generation stage, analyzed cases with high retrieval recall but low answer accuracy, and identified instances where semantically correct answers were penalized due to strict string matching. These insights provide a clearer explanation for the observed gap between improved retrieval and QA performance. We will summarize this analysis in the revised manuscript to ensure it is presented.

**Comments Suggestions And Typos-5:**
The implementation details are not provided in this submission, making it difficult to evaluate the backbone model used in BrowseNet.

The following are the implementation details of the BrowseNet pipeline. 

Named Entity Recognition (NER):
We use the GliNER model for named entity recognition during the graph construction phase. All experiments use the model’s default configuration, including a similarity threshold of 0.5 for linking extracted entities.

Chunk Embeddings:
We employ the NV-Embed-v2 model to get the embeddings. The output embedding dimensionality is set to the model’s default value of 4096.

Language Models (LLMs):
BrowseNet utilizes large language models at three stages: keyword generation (optional), query decomposition, and answer generation. All LLMs are queried using a temperature of 0 to ensure deterministic responses. Also, gpt-4o-mini is used for answer generation in the reported results.

Hardware and Environment:
All experiments were conducted on a server with an NVIDIA A100 GPU and 512 GB of RAM. Model inference and graph operations were implemented using Python with standard libraries (e.g., PyTorch, Hugging Face Transformers, NetworkX).


# Reviewer BHPP: 
We thank the reviewer for their valuable feedback. We will revise the manuscript to incorporate all reviewer comments in the camera-ready submission.

**Weakness-1:**
W1: The results show that the graph constructed by the new method has certain improvements in its own performance and retrieval performance. Is it necessary to consider the difference in construction cost compared with the traditional method?

We appreciate the reviewer’s thoughtful observation. One of the primary motivations behind BrowseNet is precisely to reduce the construction cost and complexity associated with existing knowledge graph-enhanced RAG systems, without compromising retrieval quality.

In particular, the following design choices contribute to BrowseNet’s cost-efficiency:

1) Minimal LLM Usage During Indexing:
As stated in the manuscript, "our best-performing model does not require any LLM-generated text during the offline indexing phase, making it both cost-efficient and less prone to noise." Unlike traditional approaches that rely heavily on large language models for entity or relation generation, BrowseNet only uses lightweight Named Entity Recognition (NER) via GLiNER during indexing. No LLM is used to generate intermediate graph structures or summaries during this stage.

2) Simplified Knowledge Graph Construction:
Traditional methods such as HippoRAG and KAG involve both Named Entity Recognition (NER) and Relation Extraction (RE) pipelines, often requiring fine-tuned models or LLM APIs. This substantially increases computational cost and monetary cost (especially with LLM-based APIs). In contrast, BrowseNet requires only NER, reducing the overall construction pipeline complexity by approximately 50%. This design significantly lowers both preprocessing time and infrastructure requirements, making BrowseNet more scalable and deployable in real-world settings.

We will revise the manuscript to make these cost-related benefits more explicit in the comparison with prior graph-based retrieval systems.

**Weakness-2:**
W2: More experiments needed (For example, using different LLMs).

We thank the reviewer for the thoughtful suggestion.
In the BrowseNet pipeline, LLMs are employed at three distinct stages: (1) keyword generation, (2) query decomposition, and (3) answer generation.
In the manuscript, we have already presented the impact of using different LLMs for keyword generation in Tables 5, 6, and 10, measured in terms of graph density, edge accuracy, retrieval recall, and number of entities. Similarly, the impact of LLMs on query decomposition is reported in Tables 4 and 6 through metrics such as isomorphic accuracy and retrieval recall.

Below, we now summarize the impact of various LLMs on answer generation, evaluated using Exact Match (EM) and F1-score across three benchmark datasets (HotpotQA, 2WikiMQA, MuSiQue):

|    LLM    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| gpt-4o-mini  |  62.20  | 77.69   | 63.90   | 74.50  | 41.60  | 54.08    | 55.90   |  68.76  |
| gpt-3.5-turbo  | 58.80  | 73.81  | 47.70   | 59.57   | 37.40   | 49.77  |47.97    | 61.05   |
| gpt-4.1-mini | 63.20   | **79.21**   | 64.50   | 74.43   | 42.70   | 55.07   | 56.80   | 69.57   |
| deepseek-chat-v3   |62.20   | 78.91   | **66.10**   | **75.86**   |**43.50**   | **56.25**   | **57.27**   | **70.34**   |
| gemini-2.0-flash   | **63.40**   | 78.00  | 62.10   | 70.30   | 38.10   | 47.37   | 54.53   | 65.22   |

The results reported in the manuscript are based on gpt-4o-mini. For fairness, all LLMs above were evaluated using the same input and prompt format.

From the table, it is evident that the choice of LLM significantly impacts answer generation performance with up to a 10% difference in F1-score across models. This reinforces the importance of selecting the appropriate LLM for downstream QA tasks.
These results will be incorporated into the revised version of the paper to provide a more comprehensive evaluation of the impact of different LLMs on answer generation.


**Comments Suggestions And Typos-1:**
D1: As a compound noun, the correct hyphenation for "work of art" should be "work-of-art" rather than "work_of_art".

We thank the reviewer for pointing this out. We will correct the phrasing and use the appropriate hyphenated form “work-of-art” in the revised manuscript.

**Comments Suggestions And Typos-2:**
D2: It is inappropriate to have both "Equation + numeral" and "Eq. + numeral" following "As shown in". It is preferable to unify the format. Additionally, the numerals in both cases are not enclosed in parentheses.

We thank the reviewer for highlighting this inconsistency. We will revise the manuscript to ensure a consistent format throughout. Specifically, we will standardize the usage to “Eq. (n)” and ensure that all equation references include the numeral enclosed in parentheses, as per standard conventions.





# Reviewer 6bMr: 
We thank the reviewer for their valuable feedback. We will revise the manuscript accordingly to address all reviewer comments in the camera-ready submission.

**Weakness-1:**
Benchmarks are not multi-document QA. HotpotQA, 2WikiMQA and MuSiQue require reasoning across paragraphs drawn from Wikipedia, but all evidence ultimately resides in one monolithic source. To claim multi-document QA the system should be evaluated on datasets where answers require synthesising separate primary documents. For example, FanOutQA, WikiHowQA, Narrative QA, MultiDoc2Dial, VisDoMBench, etc.


We thank the reviewer for this valuable comment. We agree that datasets such as FanOutQA and MultiDoc2Dial better reflect scenarios where answers must be synthesized from truly distinct primary documents. In our work, we followed the terminology used in prior literature [1–3], where HotpotQA, 2WikiMQA, and MuSiQue have been widely referred to as multi-document QA benchmarks, primarily due to their requirement for reasoning across multiple passages, albeit sourced from a single repository like Wikipedia. We will clarify this distinction more explicitly in the revised version to avoid any potential confusion.

**References:**
1) Yoon, C., Lee, T., Hwang, H., Jeong, M., & Kang, J. (2024). Compact: Compressing retrieved documents actively for question answering. arXiv preprint arXiv:2407.09014.
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).
3) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI Conference on Artificial Intelligence (Vol. 38, No. 17, pp. 19206-19214).

**Weakness-2:**
Results are compared only with multi-hop-within-corpus retrievers (HippoRAG-2, KAG, etc.). Standard multi-document QA baselines are absent, such as Visconde, KGP, KGP’s variant (e.g., Curiousllm), etc.

We appreciate the reviewer’s suggestion to include comparisons with standard multi-document QA baselines. Our current evaluation is focused on methods designed for multi-hop reasoning within a single corpus, such as HippoRAG-2 and KAG, in order to maintain consistency with the properties of the datasets used (HotpotQA, 2WikiMQA, MuSiQue) and to ensure a fair comparison with prior work.

That said, we acknowledge the importance of evaluating against baselines tailored for true multi-document QA.

We acknowledge, however, that including baselines tailored for true multi-document QA would strengthen the empirical evaluation, especially for settings where information must be aggregated from distinct documents. In the revised version, we will (1) clarify the rationale behind our current baseline selection, and (2) discuss the limitations this imposes.


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

We would like to clarify that the scope of our work is specifically focused on multi-hop QA, as reflected in the choice of both our datasets (HotpotQA, 2WikiMQA, MuSiQue) and our baselines (e.g., HippoRAG-2, KAG). These benchmarks require reasoning across multiple passages, rather than synthesizing/connecting information from entirely distinct documents.

For this reason, we chose not to include unrelated multi-document QA methods such as Visconde or CuriousLLM in our comparison or related work, as they are designed for a different task setting with different assumptions and evaluation protocols.

We will clarify this design choice and scope constraint in the revised manuscript to avoid misunderstanding and to make our positioning more explicit.

**Weakness-5:**
While the paper claims lower LLM cost, there is no concrete token / dollar comparison against iterative baselines.

Here we provide the token/dollar comparison against the single-step retrieval baseline, HippoRAG-2.

Offline indexing:
1) For the offline indexing phase (KG construciton) BrowseNet uses GliNER model to get the entitiesm which does not cost any API. On the other hand, for the case of HippoRAG-2, the following is the cost incurred for the KG construction phase in the HotpotQA dataset.
   
|     | Tokens |
|--------|------|
|Input tokens|5880618|
|Output tokens|2110007|
|Total tokens|7990625|


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

