# Reviewer 1: 
We thank the reviewer for the useful comments. We will revise the manuscript that incorporates all reviewers’ comments while submitting for Camera-ready papers

**Weakness-1:**

Thank you for your insightful comment on the need for a more formalized definition for weight assignment. We will now include a precise mathematical formulation. 
For a subgraph query, **$QG \in \mathbb{R}^{k}$** with query nodes sorted in topological order, ($V_{q,1}$, $V_{q,2}$, $\cdots$, $V_{q,k}$), the weighted similarity of the retrieved subgraph, **$SG$** = ($V_{1}$, $V_{2}$, $\cdots$, $V_{k}$) is given by,


$weight_{SG} = \sum_{i=1}^k \frac{similarity(V_{q,1}, V_1)}{i}$ 

Here, the similarity score is the cosine similarity between the embedding of the query node, $V_{q,i}$, and the embedding of the knowledge graph node, $V_{i}$. This weighted score for a subgraph ensures that more weightage is given to predecessor nodes in the topological order. By prioritizing earlier nodes, the approach captures the hierarchical influence within the query structure.

**Weakness-2:**

We appreciate the reviewer’s suggestion to include a latency and accuracy comparison with the existing methods. In response, we have now conducted a detailed comparison with the existing approaches. The existing methods used for the comparison are listed below:
1) BM25, which stands for Best Matching 25, is a probabilistic ranking function that incorporates term frequency saturation and document length normalization to more accurately estimate document relevance to search queries [1].
2) Contriever is an unsupervised dense retrieval model trained using contrastive learning that achieves competitive performance with BM25 on the BEIR benchmark without supervision and demonstrates strong performance after fine-tuning on MSMARCO datasets [2].
3) BlendedRAG improves Retrieval-Augmented Generation accuracy by leveraging semantic search techniques and combining sparse (keyword-based) and dense (semantic) retrievers into hybrid query-based systems that outperform traditional methods on various question-answering benchmarks [3]. In this study Stella model [8] is used as a dense retriever, and the TF-IDF vector is used as a sparse retriever.
4) GTR (Generalizable T5-based dense Retrievers) demonstrates that scaling up model size improves retrieval performance while maintaining fixed embedding dimensions, outperforming previous sparse and dense retrievers on the BEIR dataset with remarkable data efficiency [4].
5) Proposition-based retrieval represents a novel approach that encapsulates distinct factoids within text as retrieval units containing single facts with necessary context, significantly outperforming traditional passage or sentence-based methods across multiple datasets [5].
6) RAPTOR (Recursive Abstractive Processing for Tree-Organized Retrieval) employs two innovative methods—Tree Traversal Retrieval and Collapsed Tree Retrieval—to efficiently navigate hierarchical document structures and consistently outperforms traditional top-k retrieval approaches [6].
7) NaiveRAG represents the simplest form of retrieval augmented generation, following a standard process of indexing, retrieval, and generation that gained popularity after ChatGPT's introduction [7]. For our study, the Stella model is used as the semantic embedding model [8].
8) HippoRAG, inspired by the hippocampal indexing theory of human long-term memory, synergistically orchestrates language models, knowledge graphs, and personalized PageRank algorithms to outperform state-of-the-art methods on multi-hop question answering [9].
9) LightRAG integrates graph-enhanced text indexing and a dual-level retrieval framework to improve the efficiency and contextual relevance of information retrieval [10].
10) GraphRAG builds on traditional RAG by organizing datasets into knowledge graphs of entities and relationships, enabling global sensemaking and hierarchical reasoning [11].

Furthermore, we updated the BrowseNet pipeline to employ BlendedRAG for retrieval instead of NaiveRAG. This change improved the average recall score for BrowseNet, making it the top-performing model compared to the others, as shown in the table below. As LightRAG and GraphRAG use LLM-generated text for retrieval, recall results for those methods are not shown here. The approaches shown below are single-step retrieval approaches.


|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 |
| BM25 [1]   | 55.40    | 72.20    | 51.80    |  61.90    | 32.30    | 41.20    | 46.50    | 58.43   |
| Contriever [2]  | 57.20   | 75.50   | 46.60   | 57.50   | 34.80   | 46.60   | 46.20   | 59.87   |
| Blended RAG [3]  | $\underline{69.65}$   | $\underline{85.35}$   | 57.37   | 67.80   | 42.04   | 56.34   | 56.35   | 69.83   |
| GTR  [4] | 59.40   | 73.30   | 60.20  |  67.90   | 37.40  | 49.10  | 52.33   | 63.43   |
| Proposition [5]  | 58.70   | 71.10  | 56.40   | 63.10   | 37.60   | 49.30   | 50.9   | 61.17   |
| RAPTOR [6]  | 58.10   | 71.20   | 46.30   | 53.80  | 35.70  |  45.30   | 46.70   | 56.77   |
| naiveRAG [7,8]  | **73.65**  | **88.35**   | 57.93   | 68.48   | 41.24   | 57.57   | 57.61   | 71.47   |
| HippoRAG  [9] | 60.05   | 78.10   | **70.40**   | **87.87**   | 41.86   | 53.37   | 57.44   | 73.11   |
| BrowseNet (GliNER)   | 69.40   | 84.55   | $\underline{66.60}$   | $\underline{86.80}$   | $\underline{43.97}$   | **60.46**   | **59.99**   | **77.27**   |
| BrowseNet (GPT-4o)   | 68.80   | 83.95   | 65.68   | 84.60   | **45.21**   | $\underline{60.23}$   | $\underline{59.89}$   | $\underline{76.26}$   |

Answer generation results in terms of Exact match (EM) and F1-score (F1) are reported in the following table. The number of chunks used as input to the LLM for HippoRAG and BrowseNet is ten.

|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| LightRAG [10]  |  9.90  | 20.20   | 2.50   | 12.1  | 2.00  | 9.30    | 4.80   |  13.87  |
| GraphRAG [11]  | 51.40  | 67.6   | 45.70   | 61.0   | 27.00   | **42.0**  |41.37    | **56.87**   |
| HippoRAG  [9] | 44.30   | 60.53   | 50.00   | $\underline{63.06}$   | 22.00   | 35.06   | 38.77   | 52.88   |
| BrowseNet (GliNER)   | **56.60**   | **70.46**   | **56.40**   | **63.57**   | **28.30**   | 35.79   | **47.10**   | $\underline{56.61}$   |
| BrowseNet (GPT-4o)   | $\underline{56.30}$   | $\underline{69.37}$   | $\underline{54.50}$   | 61.46   | $\underline{27.70}$   | $\underline{35.97}$   | $\underline{46.17}$   | 55.60   |


BrowseNet with GliNER demonstrates superior performance across all datasets with the highest average exact match score (47.10%), suggesting its effectiveness in retrieving precisely correct answers compared to other methods. Despite GraphRAG achieving the highest average F1 score (56.87%), BrowseNet variants consistently deliver more balanced performance across both metrics, particularly excelling in exact matches while maintaining competitive F1 scores, indicating their robust capabilities for complex multi-hop question answering tasks.



**References:**
1) Robertson, S. E., & Walker, S. (1994). Some simple effective approximations to the 2-poisson model for probabilistic weighted retrieval. In SIGIR’94: Proceedings of the Seventeenth Annual International ACM-SIGIR Conference on Research and Development in Information Retrieval, organised by Dublin City University (pp. 232-241). Springer London.
2) Izacard, G., Caron, M., Hosseini, L., Riedel, S., Bojanowski, P., Joulin, A., & Grave, E. (2021). Unsupervised dense information retrieval with contrastive learning. arXiv preprint arXiv:2112.09118.
3) Sawarkar, K., Mangal, A., & Solanki, S. R. (2024, August). Blended rag: Improving rag (retriever-augmented generation) accuracy with semantic search and hybrid query-based retrievers. In 2024 IEEE 7th International Conference on Multimedia Information Processing and Retrieval (MIPR) (pp. 155-161). IEEE.
4) Ni, J., Qu, C., Lu, J., Dai, Z., Ábrego, G. H., Ma, J., ... & Yang, Y. (2021). Large dual encoders are generalizable retrievers. arXiv preprint arXiv:2112.07899.
5) Chen, T., Wang, H., Chen, S., Yu, W., Ma, K., Zhao, X., ... & Yu, D. (2024, November). Dense x retrieval: What retrieval granularity should we use?. In Proceedings of the 2024 Conference on Empirical Methods in Natural Language Processing (pp. 15159-15177).
6) Sarthi, P., Abdullah, S., Tuli, A., Khanna, S., Goldie, A., & Manning, C. D. (2024, May). Raptor: Recursive abstractive processing for tree-organized retrieval. In The Twelfth International Conference on Learning Representations.
7) Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., ... & Kiela, D. (2020). Retrieval-augmented generation for knowledge-intensive nlp tasks. Advances in neural information processing systems, 33, 9459-9474.
8) Zhang, D., Li, J., Zeng, Z., & Wang, F. (2024). Jasper and Stella: distillation of SOTA embedding models. arXiv preprint arXiv:2412.19048.
9) Jimenez Gutierrez, B., Shu, Y., Gu, Y., Yasunaga, M., & Su, Y. (2024). HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models. Advances in Neural Information Processing Systems, 37, 59532-59569.
10) Guo, Z., Xia, L., Yu, Y., Ao, T., & Huang, C. (2024). Lightrag: Simple and fast retrieval-augmented generation.
11) Edge, D., Trinh, H., Cheng, N., Bradley, J., Chao, A., Mody, A., ... & Larson, J. (2024). From local to global: A graph rag approach to query-focused summarization. arXiv preprint arXiv:2404.16130.


 
 
 **Latency analysis**
 For latency comparison, GraphRAG, LightRAG, and BrowseNET pipelines were considered. 

|Method | Average runtime per query (seconds) |
|------|------|
|BrowseNet|1.18|
|GraphRAG|3.669|
|LightRAG|6.73|

BrowseNet has the fastest retrieval performance compared to the other methods because of its single-step multi-hop retrieval strategy. 


**Weakness-3:**


We appreciate the reviewer’s feedback on the need to analyze critical parameters such as the ColBERT synonymity threshold. In response, we have conducted a detailed sensitivity analysis on the synonymity threshold as shown below. The NER model used below for this analysis is GLiNER.
| Synonymity threshold | Edge Accuracy (2WikiMQA) | Edge Accuracy (MuSiQue) | Recall@5 (HoptpotQA)| Recall@5 (2WikiMQA)| Recall@5 (MuSiQue)|
|------|------|------|------|------|------|
|0.7|100|97.43|84.85|85.22|59.78|
|0.8|99.86|95.18|85.30|85.65|59.86|
|0.9|99.86|94.78|84.55|86.80|60.45|

The table suggests that the recall@5 metric remains approximately equal across all datasets used in the study. However, edge accuracy tends to be higher when the synonymity threshold is low. Additionally, as the threshold decreases, leading to a denser graph, the retrieval time is expected to rise due to the greater number of potential neighbors for each node. This indicates a trade-off between efficiency and effectiveness: a lower threshold increases retrieval time due to greater graph density, while a higher threshold reduces it.

**Weakness-4:**


We appreciate the reviewer’s feedback but would appreciate further clarification on this point to ensure we address it appropriately.

**Comments Suggestions And Typos 1:**

We appreciate the reviewer’s attention to detail. We will correct the JSON formatting in Figure 3 by ensuring the proper closing brace (}) is included.

**Comments Suggestions And Typos 2:**

In response to the reviewer’s suggestion, we have conducted additional experiments and presented the results above.

**Comments Suggestions And Typos 3:**

A subquery could fail because of a decomposed query returning no Knowledge Graph (KG) nodes, or a node in KG having no neighbours. We address each of the cases as defined below:
1) Decomposed query returning no KG nodes: As the retrieval and sorting of nodes is based on the cosine-similarity between the query and the passage, a decomposed query will return no KG nodes if the cosine similarity is zero. High dimensionality of the embedding vector (128-1024+ dimensions) and continuous value distributions make true zeros improbable for cosine-similarity.
2) A node in KG having no neighbours: If a given predecessor node in the knowledge graph (KG) has no neighbors, all nodes in the KG are treated as its neighbors, and the search is performed across the entire graph.



# Reviewer 2:
We thank the reviewer for the useful comments. We will revise the manuscript that incorporates all reviewers’ comments while submitting for Camera-ready papers

**Weaknesses 1**

Thank you for your feedback on the formatting and writing quality of our paper. We appreciate your suggestions and will carefully revise the formatting and layout to improve clarity and readability. Additionally, we will refine the text to enhance its polish and coherence in the final version.

**Weaknesses 2**

We appreciate the reviewer’s concern regarding baseline selection. In response, we have expanded our experiments to include additional baselines as detailed below.

The following baselines are included in the study:

1) BM25, which stands for Best Matching 25, is a probabilistic ranking function that incorporates term frequency saturation and document length normalization to more accurately estimate document relevance to search queries [1].
2) Contriever is an unsupervised dense retrieval model trained using contrastive learning that achieves competitive performance with BM25 on the BEIR benchmark without supervision and demonstrates strong performance after fine-tuning on MSMARCO datasets [2].
3) BlendedRAG improves Retrieval-Augmented Generation accuracy by leveraging semantic search techniques and combining sparse (keyword-based) and dense (semantic) retrievers into hybrid query-based systems that outperform traditional methods on various question-answering benchmarks [3]. In this study Stella model [8] is used as a dense retriever, and the TF-IDF vector is used as a sparse retriever.
4) GTR (Generalizable T5-based dense Retrievers) demonstrates that scaling up model size improves retrieval performance while maintaining fixed embedding dimensions, outperforming previous sparse and dense retrievers on the BEIR dataset with remarkable data efficiency [4].
5) Proposition-based retrieval represents a novel approach that encapsulates distinct factoids within text as retrieval units containing single facts with necessary context, significantly outperforming traditional passage or sentence-based methods across multiple datasets [5].
6) RAPTOR (Recursive Abstractive Processing for Tree-Organized Retrieval) employs two innovative methods—Tree Traversal Retrieval and Collapsed Tree Retrieval—to efficiently navigate hierarchical document structures and consistently outperforms traditional top-k retrieval approaches [6].
7) NaiveRAG represents the simplest form of retrieval augmented generation, following a standard process of indexing, retrieval, and generation that gained popularity after ChatGPT's introduction [7]. For our study, the Stella model is used as the semantic embedding model [8].
8) HippoRAG, inspired by the hippocampal indexing theory of human long-term memory, synergistically orchestrates language models, knowledge graphs, and personalized PageRank algorithms to outperform state-of-the-art methods on multi-hop question answering [9].
9) LightRAG integrates graph-enhanced text indexing and a dual-level retrieval framework to improve the efficiency and contextual relevance of information retrieval [10].
10) GraphRAG builds on traditional RAG by organizing datasets into knowledge graphs of entities and relationships, enabling global sensemaking and hierarchical reasoning [11].

Furthermore, we updated the BrowseNet pipeline to employ BlendedRAG for retrieval instead of NaiveRAG. This change improved the average recall score for BrowseNet, making it the top-performing model compared to the others, as shown in the table below. As LightRAG and GraphRAG use LLM-generated text for retrieval, recall results for those methods are not shown here. The approaches shown below are single-step retrieval approaches.


|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 |
| BM25 [1]   | 55.40    | 72.20    | 51.80    |  61.90    | 32.30    | 41.20    | 46.50    | 58.43   |
| Contriever [2]  | 57.20   | 75.50   | 46.60   | 57.50   | 34.80   | 46.60   | 46.20   | 59.87   |
| Blended RAG [3]  | $\underline{69.65}$   | $\underline{85.35}$   | 57.37   | 67.80   | 42.04   | 56.34   | 56.35   | 69.83   |
| GTR  [4] | 59.40   | 73.30   | 60.20  |  67.90   | 37.40  | 49.10  | 52.33   | 63.43   |
| Proposition [5]  | 58.70   | 71.10  | 56.40   | 63.10   | 37.60   | 49.30   | 50.9   | 61.17   |
| RAPTOR [6]  | 58.10   | 71.20   | 46.30   | 53.80  | 35.70  |  45.30   | 46.70   | 56.77   |
| naiveRAG [7,8]  | **73.65**  | **88.35**   | 57.93   | 68.48   | 41.24   | 57.57   | 57.61   | 71.47   |
| HippoRAG  [9] | 60.05   | 78.10   | **70.40**   | **87.87**   | 41.86   | 53.37   | 57.44   | 73.11   |
| BrowseNet (GliNER)   | 69.40   | 84.55   | $\underline{66.60}$   | $\underline{86.80}$   | $\underline{43.97}$   | **60.46**   | **59.99**   | **77.27**   |
| BrowseNet (GPT-4o)   | 68.80   | 83.95   | 65.68   | 84.60   | **45.21**   | $\underline{60.23}$   | $\underline{59.89}$   | $\underline{76.26}$   |


As we can see from the above table, the average recall score for BrowseNet is higher than all the methods, and as detailed in the manuscript, BrowseNet consistently performs better than all the methods in the Musique dataset. Although the HotpotQA dataset requires two-hop reasoning, it has been identified as a weaker benchmark for multi-hop retrieval due to the prevalence of spurious signals. Consequently, naiveRAG performs comparatively well in this dataset, as its simpler retrieval mechanism remains effective. In contrast, the 2WikiMultiHopQA dataset features connected components in the query subgraph with a maximum length of two, allowing keywords alone to serve as reliable linking elements between passages. 



Answer generation results in terms of Exact match (EM) and F1-score (F1) are reported in the following table. The number of chunks used as input to the LLM for HippoRAG and BrowseNet is ten.

|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| LightRAG [10]  |  9.90  | 20.20   | 2.50   | 12.1  | 2.00  | 9.30    | 4.80   |  13.87  |
| GraphRAG [11]  | 51.40  | 67.6   | 45.70   | 61.0   | 27.00   | **42.0**  |41.37    | **56.87**   |
| HippoRAG  [9] | 44.30   | 60.53   | 50.00   | $\underline{63.06}$   | 22.00   | 35.06   | 38.77   | 52.88   |
| BrowseNet (GliNER)   | **56.60**   | **70.46**   | **56.40**   | **63.57**   | **28.30**   | 35.79   | **47.10**   | $\underline{56.61}$   |
| BrowseNet (GPT-4o)   | $\underline{56.30}$   | $\underline{69.37}$   | $\underline{54.50}$   | 61.46   | $\underline{27.70}$   | $\underline{35.97}$   | $\underline{46.17}$   | 55.60   |


BrowseNet with GliNER demonstrates superior performance across all datasets with the highest average exact match score (47.10%), suggesting its effectiveness in retrieving precisely correct answers compared to other methods. Despite GraphRAG achieving the highest average F1 score (56.87%), BrowseNet variants consistently deliver more balanced performance across both metrics, particularly excelling in exact matches while maintaining competitive F1 scores, indicating their robust capabilities for complex multi-hop question answering tasks.

**References:**
1) Robertson, S. E., & Walker, S. (1994). Some simple effective approximations to the 2-poisson model for probabilistic weighted retrieval. In SIGIR’94: Proceedings of the Seventeenth Annual International ACM-SIGIR Conference on Research and Development in Information Retrieval, organised by Dublin City University (pp. 232-241). Springer London.
2) Izacard, G., et al. (2021). Unsupervised dense information retrieval with contrastive learning. arXiv preprint arXiv:2112.09118.
3) Sawarkar, K., Mangal, A., & Solanki, S. R. (2024, August). Blended rag: Improving rag (retriever-augmented generation) accuracy with semantic search and hybrid query-based retrievers. In 2024 IEEE 7th International Conference on Multimedia Information Processing and Retrieval (MIPR) (pp. 155-161). IEEE.
4) Ni, J., et al. (2021). Large dual encoders are generalizable retrievers. arXiv preprint arXiv:2112.07899.
5) Chen, T., et al. (2024, November). Dense x retrieval: What retrieval granularity should we use?. In Proceedings of the 2024 Conference on Empirical Methods in Natural Language Processing (pp. 15159-15177).
6) Sarthi, P.,  et al. (2024, May). Raptor: Recursive abstractive processing for tree-organized retrieval. In The Twelfth International Conference on Learning Representations.
7) Lewis, P., et al. (2020). Retrieval-augmented generation for knowledge-intensive nlp tasks. Advances in neural information processing systems, 33, 9459-9474.
8) Zhang, D., et al. (2024). Jasper and Stella: distillation of SOTA embedding models. arXiv preprint arXiv:2412.19048.
9) Jimenez Gutierrez, B., et al. (2024). HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models. Advances in Neural Information Processing Systems, 37, 59532-59569.
10) Guo, Z., et al. (2024). Lightrag: Simple and fast retrieval-augmented generation.
11) Edge, D., et al. (2024). From local to global: A graph rag approach to query-focused summarization. arXiv preprint arXiv:2404.16130.

**Weaknesses 3**

In response to the reviewer’s suggestion, we have expanded the evaluation to include quantitative metrics for query-graph generation. We define the metric as isomorphic accuracy that captures the structural similarity of the query-subgraph with respect to the reasoning pathway provided in the 2WikiMQA and Musique dataset.

Two graphs, $G1$ and $G2$, are considered isomorphic if there exists a bijective function $f$ that maps the vertices of $G1$ to those of $G2$ while preserving adjacency. That is, an edge exists between vertices $u$ and $v$ in $G1$ if and only if an edge exists between $f(u)$ and $f(v)$ in $G2$. For our case, $G1$ is the query-subgraph and $G2$ is the graph that can be derived from the reasoning path or query decomposition provided in the 2WikiMQA and musique datasets, respectively.


For instance, in the Musique dataset, consider the query:
"What month did the Tripartite discussions begin between Britain, France, and the country where, despite being headquartered in the nation called the nobilities commonwealth, the top-ranking Warsaw Pact operatives originated?" 

The provided decomposition is: Q1)'What was the nobilities commonwealth?' Q2) 'Despite being headquartered in #1, the top-ranking operatives of the Warsaw Pact were from which country?' Q3) 'What month did the Tripartite discussions begin between Britain, #2 and France?'.

This decomposition can be represented as a graph Q1-->Q2-->Q3. 

The GPT-4o model generated query decomposition is: Q1) What is the nation called the nobility's commonwealth? Q2) <Q1> Where are the top-ranking Warsaw Pact operatives headquartered? Q3) <Q2> In which country did the top-ranking Warsaw Pact operatives originate?\nQ4) <Q3> What month did the Tripartite discussions begin between Britain, France, and the country where the top-ranking Warsaw Pact operatives originated?

This decomposition can be represented as a graph Q1-->Q2-->Q3-->Q4. Here, as both the queries have different structures, the isomorphic accuracy is 0.

A similar approach is used to compute isomorphic accuracy for the 2WikiMQA corpus. The average isomorphic accuracy is summarized in the following table.
|LLM model|2WikiMQA|MuSiQue|
|------|------|------|
| GPT-4o | 0.973 | 0.685 |
| claude-3.7 sonnet  |  0.967  | 0.487 |

The isomorphic accuracy for query decomposition is higher for the 2WikiMQA dataset compared to the MuSiQue dataset across both models. GPT-4o performs slightly better than Claude-3.7 Sonnet on both datasets, achieving 0.973 accuracy for 2WikiMQA and 0.685 for MuSiQue, whereas Claude-3.7 Sonnet scores 0.967 and 0.487, respectively. 

**Comments Suggestions And Typos - 1:**
We appreciate the reviewer’s feedback on the number of baseline comparisons. In response, we have expanded our evaluation to include additional relevant baselines beyond HippoRAG and NaiveRAG, providing a more comprehensive comparison as previously shown.

**Comments Suggestions And Typos - 2:**
Thank you for your insightful feedback. To assess the broader applicability of our method, we have extended our experiments beyond GPT-4o to include Claude-3.7 Sonnet for both keyword generation and for the query-subgraph generation.
The following table shows the results of evaluating the keyword generation in terms of edge accuracy.

|NER Model|Dataset|Edge Accuracy|
|------|------|------|
|GLiNER|2WikiMQA | 99.86|
|GLiNER|MuSiQue | 91.03|
|GPT-3.5-turbo|2WikiMQA | 99.33|
|GPT-3.5-turbo|MuSiQue | 93.99 |
|GPT-4o|2WikiMQA | 98.74|
|GPT-4o|MuSiQue |  97.83|
|Claude-3.7 Sonnet|2WikiMQA | 99.73|
|Claude-3.7 Sonnet|MuSiQue | 94.33 |

It can be inferred from the given table that gpt-4o model has the highest edge acccuracy compared to the other methods in the case of the MusiQue dataset. For 2WikiMQA dataset, GLiNER model has the highest edge accuracy that the other LLMs employed.


Further, the query-subgraphs generated were also evaluated for their isomorphic similarity with the query decomposition given in the benchmark datasets for both the Claude-3.7 Sonnet and gpt-4o models as shown previously.

**Comments Suggestions And Typos - 3:**
Mitigating noise in LLM-generated text, including hallucinations, factual inaccuracies, and irrelevant content, requires strategies based on both training methodologies and inference optimization. For inference optimization, one approach is to have the LLM generate multiple possible decompositions of the multi-hop query and retrieve corresponding subgraphs for each variation. Reordering these subgraphs based on their similarity to the query-subgraphs may help filter out inconsistencies. However, the effectiveness of this method has yet to be validated. This can be noted as a limitation in the manuscript.

**Comments Suggestions And Typos - 4:**
We appreciate the reviewer’s feedback on the depth of our analysis. In response, we have now conducted a more detailed error analysis, including the breakdown of error types on 100 samples in the musique dataset, and have provided the percentage distribution of each error type in the following table.

|    Error type    | Percentage |
|------|------|
| NER + synonymous word extraction | 2 |
| Query-subgraph extraction | 42 |
| RAG | 56|

As observed in the table, RAG accounts for the majority of the error in the retrieval process of BrowseNet. Upon analyzing the cause, we found that the embedding models used in this study are fine-tuned to retrieve relevant chunks for single-hop queries, but not for decomposed single-hop queries. For example, the question "When was Lady Godiva's birthplace abolished?" can be decomposed into:  "Q1) Where was Lady Godiva born? Q2) <Q1> When was Lady Godiva's birthplace abolished?". While the passage relevant to Q1 can be retrieved, there may be numerous potential candidates semantically closer to Q2. This occurs because the answer to Q1 is not provided to the embedding models, which results in added noise to the query and affects the retrieval accuracy for Q2. One potential direction for future work would be to train an embedding model specifically designed to excel at retrieving relevant information for decomposed subqueries. This would improve the retrieval accuracy by ensuring that the model can effectively handle queries broken down into smaller components, enhancing the overall performance in multi-hop reasoning tasks.

The next significant source of error in the pipeline arises from query-subgraph extraction. As detailed in the error analysis section of the manuscript, this issue occurs because a single multi-hop query can be decomposed in various ways. For instance, the query "What year did the publisher of Labyrinth end?" can be decomposed as: 'Q1) What is Labyrinth?  Q2) <Q1> Who is the publisher of Labyrinth?  Q3) <Q2> What year did the publisher of Labyrinth end?. The subquery Q1, which acts as the initial node, is an unnecessary step that disrupts the performance of the model, as it does not contribute directly to answering the final question and adds complexity to the retrieval process.

The NER (Named Entity Recognition) and synonymous word extraction step contributes the smallest portion (2%) to the error. This is primarily due to the LLMs' inability to recognize certain keywords present in the text, which leads to missed entities or synonyms that could have improved the retrieval process.

**Comments Suggestions And Typos - 5:**
Thank you for your valuable suggestion. 
1) We will revise Table 3 to enhance its formatting, ensuring consistency and better presentation of the results.
2) We confirm that “Bowsenet” was a typo, and we will fix it to correctly read “BrowseNet.”
3) We confirm that “predessorChunks” was a typo, and we will fix it to correctly read “predecessorChunks.”

# Reviewer 3:
We thank the reviewer for the useful comments. We will revise the manuscript that incorporates all reviewers’ comments while submitting for Camera-ready papers


**Weaknesses 1**

Thank you for pointing out the issues with the structure of the paper and the placement of Tables and Figures. We will carefully revise the manuscript to improve its overall clarity and readability. Specifically, we 
will restructure the experimental section to ensure a more logical flow of information. Also, all Tables and Figures will be relocated to appear as close as possible to their corresponding paragraphs in the text. This adjustment ensures that readers can easily refer to them without having to search through other sections of the paper. We believe these changes will address your concerns and significantly enhance the readability of the manuscript. 

**Weaknesses 2**

We appreciate the reviewer’s comment regarding the organization of Section 2.1. In response, we have restructured the related work section to better align with the focus of the paper and ensure it provides a more in-depth discussion of prior research.

**Related work:**
The integration of knowledge graphs (KGs) with retrieval-augmented generation (RAG) systems has emerged as a pivotal strategy for addressing the limitations of conventional RAG frameworks in multi-document question answering (MD-QA). This section synthesizes advancements across two thematic areas: (1) foundational RAG architectures and their constraints in multi-hop reasoning, and (2) KG-enhanced retrieval frameworks.

**Limitations of Traditional RAG in Multi-Document Contexts**
Traditional RAG systems [1][2] rely on vector similarity search to retrieve text chunks from unstructured documents, a method that struggles with multi-hop queries requiring logical connections across disparate sources. For instance, while RAG pipelines excel at answering factoid questions, they often fail to synthesize answers requiring inference over multiple documents. This shortcoming stems from the inherent linearity of vector-based retrieval, which cannot capture relational dependencies between entities or concepts spread across documents.

**Knowledge Graph-Augmented RAG Frameworks**
To tackle these challenges, researchers have incorporated Knowledge Graphs (KGs) into Retrieval-Augmented Generation (RAG) pipelines, leveraging their structured representation of entities and relationships. Knowledge Graph Prompting (KGP) [3] utilizes a dual-module architecture: one module constructs a KG over documents using passage similarities and structural relations, while the second module, a traversal agent, dynamically retrieves context via graph navigation. The GraphRAG [4] framework builds on this approach by encoding unstructured text into node-edge graphs, where edges capture lexical, semantic, and hierarchical relationships. By framing retrieval as a graph traversal problem, GraphRAG facilitates multi-hop reasoning through chain-of-thought prompting, resulting in a 31% reduction in hallucination rates compared to vector-only RAG. HippoRAG [5] enhances this by simulating human memory processes, integrating language models, knowledge graphs, and Personalized PageRank algorithms. It achieves state-of-the-art performance on multi-hop question answering tasks while being computationally efficient, outperforming iterative retrieval methods in both speed and cost. HippoRAG excels in entity-centric tasks and delivers state-of-the-art results on datasets like 2WikiMQA. However, for datasets requiring semantic retrieval, there is room for improvement. In response, we propose BrowseNet, which builds upon existing methods to capture both lexical and semantic relationships between multi-hop queries and passages, leading to more effective retrieval.


**References**
1) Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., ... & Kiela, D. (2020). Retrieval-augmented generation for knowledge-intensive nlp tasks. Advances in neural information processing systems, 33, 9459-9474.
2) Sawarkar, K., Mangal, A., & Solanki, S. R. (2024, August). Blended rag: Improving rag (retriever-augmented generation) accuracy with semantic search and hybrid query-based retrievers. In 2024 IEEE 7th International Conference on Multimedia Information Processing and Retrieval (MIPR) (pp. 155-161). IEEE.
3) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI Conference on Artificial Intelligence (Vol. 38, No. 17, pp. 19206-19214).
4) Edge, D., Trinh, H., Cheng, N., Bradley, J., Chao, A., Mody, A., ... & Larson, J. (2024). From local to global: A graph rag approach to query-focused summarization. arXiv preprint arXiv:2404.16130.
5) Jimenez Gutierrez, B., Shu, Y., Gu, Y., Yasunaga, M., & Su, Y. (2024). HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models. Advances in Neural Information Processing Systems, 37, 59532-59569.


**Weaknesses 3**

The key differences between the KG of BrowseNet and HippoRAG are as given below:
1) The nodes in the HippoRAG are the entities and in the case of BrowseNet, it is the chunks/passages.
2) The edges between the nodes indicate the presence of a relation between the nodes in HippoRAG. For BrowseNet, the edge indicates the  presence of a common keyword between the passages.
3) Construction of KG in BrowseNet requires only named-entity recognition (NER) to be done. For HippoRAG, both NER and relation extraction (RE) have to be done. 


Further, we agree with the reviewer that the document as a KG node has been explored in the previous studies like THREAD [1]. The key difference between these two methods lies in the way of constructing the KG and the granularity of the nodes. KG construction in THREAD involves organizing the documents into five-element logic units; on the other hand, BrowseNet uses the raw documents without any LLM generated text to construct the KG. Moreover, nodes of KG in THREAD includes header, body, linker etc., In the case of BrowseNet, its only the documents or the passages.

**References**
1) An, K., Yang, F., Li, L., Lu, J., Cheng, S., Si, S., ... & Chang, B. (2024). Thread: A Logic-Based Data Organization Paradigm for How-To Question Answering with Retrieval Augmented Generation. arXiv preprint arXiv:2406.13372.

**Weaknesses 4**

Thank you for your valuable suggestion. We acknowledge that our results may not have been presented as clearly as intended. Here, we present here the new set of baselines with which BrowseNet is compared to provide a more comprehensive justification for our conclusions.

The following baselines are included in the study:

1) BM25, which stands for Best Matching 25, is a probabilistic ranking function that incorporates term frequency saturation and document length normalization to more accurately estimate document relevance to search queries [1].
2) Contriever is an unsupervised dense retrieval model trained using contrastive learning that achieves competitive performance with BM25 on the BEIR benchmark without supervision and demonstrates strong performance after fine-tuning on MSMARCO datasets [2].
3) BlendedRAG improves Retrieval-Augmented Generation accuracy by leveraging semantic search techniques and combining sparse (keyword-based) and dense (semantic) retrievers into hybrid query-based systems that outperform traditional methods on various question-answering benchmarks [3]. In this study Stella model [8] is used as a dense retriever, and the TF-IDF vector is used as a sparse retriever.
4) GTR (Generalizable T5-based dense Retrievers) demonstrates that scaling up model size improves retrieval performance while maintaining fixed embedding dimensions, outperforming previous sparse and dense retrievers on the BEIR dataset with remarkable data efficiency [4].
5) Proposition-based retrieval represents a novel approach that encapsulates distinct factoids within text as retrieval units containing single facts with necessary context, significantly outperforming traditional passage or sentence-based methods across multiple datasets [5].
6) RAPTOR (Recursive Abstractive Processing for Tree-Organized Retrieval) employs two innovative methods—Tree Traversal Retrieval and Collapsed Tree Retrieval—to efficiently navigate hierarchical document structures and consistently outperforms traditional top-k retrieval approaches [6].
7) NaiveRAG represents the simplest form of retrieval augmented generation, following a standard process of indexing, retrieval, and generation that gained popularity after ChatGPT's introduction [7]. For our study, the Stella model is used as the semantic embedding model [8].
8) HippoRAG, inspired by the hippocampal indexing theory of human long-term memory, synergistically orchestrates language models, knowledge graphs, and personalized PageRank algorithms to outperform state-of-the-art methods on multi-hop question answering [9].
9) LightRAG integrates graph-enhanced text indexing and a dual-level retrieval framework to improve the efficiency and contextual relevance of information retrieval [10].
10) GraphRAG builds on traditional RAG by organizing datasets into knowledge graphs of entities and relationships, enabling global sensemaking and hierarchical reasoning [11].

Furthermore, we updated the BrowseNet pipeline to employ BlendedRAG for retrieval instead of NaiveRAG. This change improved the average recall score for BrowseNet, making it the top-performing model compared to the others, as shown in the table below. As LightRAG and GraphRAG use LLM-generated text for retrieval, recall results for those methods are not shown here. The approaches shown below are single-step retrieval approaches.


|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 |
| BM25 [1]   | 55.40    | 72.20    | 51.80    |  61.90    | 32.30    | 41.20    | 46.50    | 58.43   |
| Contriever [2]  | 57.20   | 75.50   | 46.60   | 57.50   | 34.80   | 46.60   | 46.20   | 59.87   |
| Blended RAG [3]  | $\underline{69.65}$   | $\underline{85.35}$   | 57.37   | 67.80   | 42.04   | 56.34   | 56.35   | 69.83   |
| GTR  [4] | 59.40   | 73.30   | 60.20  |  67.90   | 37.40  | 49.10  | 52.33   | 63.43   |
| Proposition [5]  | 58.70   | 71.10  | 56.40   | 63.10   | 37.60   | 49.30   | 50.9   | 61.17   |
| RAPTOR [6]  | 58.10   | 71.20   | 46.30   | 53.80  | 35.70  |  45.30   | 46.70   | 56.77   |
| naiveRAG [7,8]  | **73.65**  | **88.35**   | 57.93   | 68.48   | 41.24   | 57.57   | 57.61   | 71.47   |
| HippoRAG  [9] | 60.05   | 78.10   | **70.40**   | **87.87**   | 41.86   | 53.37   | 57.44   | 73.11   |
| BrowseNet (GliNER)   | 69.40   | 84.55   | $\underline{66.60}$   | $\underline{86.80}$   | $\underline{43.97}$   | **60.46**   | **59.99**   | **77.27**   |
| BrowseNet (GPT-4o)   | 68.80   | 83.95   | 65.68   | 84.60   | **45.21**   | $\underline{60.23}$   | $\underline{59.89}$   | $\underline{76.26}$   |


As we can see from the above table, the average recall score for BrowseNet is higher than all the methods, and as detailed in the manuscript, BrowseNet consistently performs better than all the methods in the Musique dataset. Although the HotpotQA dataset requires two-hop reasoning, it has been identified as a weaker benchmark for multi-hop retrieval due to the prevalence of spurious signals. Consequently, naiveRAG performs comparatively well in this dataset, as its simpler retrieval mechanism remains effective. In contrast, the 2WikiMultiHopQA dataset features connected components in the query subgraph with a maximum length of two, allowing keywords alone to serve as reliable linking elements between passages. 



Answer generation results in terms of Exact match (EM) and F1-score (F1) are reported in the following table. The number of chunks used as input to the LLM for HippoRAG and BrowseNet is ten.

|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| LightRAG [10]  |  9.90  | 20.20   | 2.50   | 12.1  | 2.00  | 9.30    | 4.80   |  13.87  |
| GraphRAG [11]  | 51.40  | 67.6   | 45.70   | 61.0   | 27.00   | **42.0**  |41.37    | **56.87**   |
| HippoRAG  [9] | 44.30   | 60.53   | 50.00   | $\underline{63.06}$   | 22.00   | 35.06   | 38.77   | 52.88   |
| BrowseNet (GliNER)   | **56.60**   | **70.46**   | **56.40**   | **63.57**   | **28.30**   | 35.79   | **47.10**   | $\underline{56.61}$   |
| BrowseNet (GPT-4o)   | $\underline{56.30}$   | $\underline{69.37}$   | $\underline{54.50}$   | 61.46   | $\underline{27.70}$   | $\underline{35.97}$   | $\underline{46.17}$   | 55.60   |


In the QA evaluation, BrowseNet with GliNER demonstrates superior performance across all datasets with the highest average exact match score (47.10%), suggesting its effectiveness in retrieving precisely correct answers compared to other methods. Despite GraphRAG achieving the highest average F1 score (56.87%), BrowseNet variants consistently deliver more balanced performance across both metrics, particularly excelling in exact matches while maintaining competitive F1 scores, indicating their robust capabilities for complex multi-hop question answering tasks.



**References:**
1) Robertson, S. E., & Walker, S. (1994). Some simple effective approximations to the 2-poisson model for probabilistic weighted retrieval. In SIGIR’94: Proceedings of the Seventeenth Annual International ACM-SIGIR Conference on Research and Development in Information Retrieval, organised by Dublin City University (pp. 232-241). Springer London.
2) Izacard, G., Caron, M., Hosseini, L., Riedel, S., Bojanowski, P., Joulin, A., & Grave, E. (2021). Unsupervised dense information retrieval with contrastive learning. arXiv preprint arXiv:2112.09118.
3) Sawarkar, K., Mangal, A., & Solanki, S. R. (2024, August). Blended rag: Improving rag (retriever-augmented generation) accuracy with semantic search and hybrid query-based retrievers. In 2024 IEEE 7th International Conference on Multimedia Information Processing and Retrieval (MIPR) (pp. 155-161). IEEE.
4) Ni, J., Qu, C., Lu, J., Dai, Z., Ábrego, G. H., Ma, J., ... & Yang, Y. (2021). Large dual encoders are generalizable retrievers. arXiv preprint arXiv:2112.07899.
5) Chen, T., Wang, H., Chen, S., Yu, W., Ma, K., Zhao, X., ... & Yu, D. (2024, November). Dense x retrieval: What retrieval granularity should we use?. In Proceedings of the 2024 Conference on Empirical Methods in Natural Language Processing (pp. 15159-15177).
6) Sarthi, P., Abdullah, S., Tuli, A., Khanna, S., Goldie, A., & Manning, C. D. (2024, May). Raptor: Recursive abstractive processing for tree-organized retrieval. In The Twelfth International Conference on Learning Representations.
7) Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., ... & Kiela, D. (2020). Retrieval-augmented generation for knowledge-intensive nlp tasks. Advances in neural information processing systems, 33, 9459-9474.
8) Zhang, D., Li, J., Zeng, Z., & Wang, F. (2024). Jasper and Stella: distillation of SOTA embedding models. arXiv preprint arXiv:2412.19048.
9) Jimenez Gutierrez, B., Shu, Y., Gu, Y., Yasunaga, M., & Su, Y. (2024). HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models. Advances in Neural Information Processing Systems, 37, 59532-59569.
10) Guo, Z., Xia, L., Yu, Y., Ao, T., & Huang, C. (2024). Lightrag: Simple and fast retrieval-augmented generation.
11) Edge, D., Trinh, H., Cheng, N., Bradley, J., Chao, A., Mody, A., ... & Larson, J. (2024). From local to global: A graph rag approach to query-focused summarization. arXiv preprint arXiv:2404.16130.


**Weaknesses 5**

We appreciate the reviewer’s feedback on the depth of our analysis. In response, we have now conducted a more detailed error analysis, including the breakdown of error types on 100 samples in the musique dataset, and have provided the percentage distribution of each error type in the following table.

|    Error type    | Percentage |
|------|------|
| NER + synonymous word extraction | 2 |
| Query-subgraph extraction | 42 |
| RAG | 56|


As observed in the table, RAG accounts for the majority of the error in the retrieval process of BrowseNet. Upon analyzing the cause, we found that the embedding models used in this study are fine-tuned to retrieve relevant chunks for single-hop queries, but not for decomposed single-hop queries. For example, the question "When was Lady Godiva's birthplace abolished?" can be decomposed into:  "Q1) Where was Lady Godiva born? Q2) <Q1> When was Lady Godiva's birthplace abolished?". While the passage relevant to Q1 can be retrieved, there may be numerous potential candidates semantically closer to Q2. This occurs because the answer to Q1 is not provided to the embedding models, which results in added noise to the query and affects the retrieval accuracy for Q2. One potential direction for future work would be to train an embedding model specifically designed to excel at retrieving relevant information for decomposed subqueries. This would improve the retrieval accuracy by ensuring that the model can effectively handle queries broken down into smaller components, enhancing the overall performance in multi-hop reasoning tasks.

The next significant source of error in the pipeline arises from query-subgraph extraction. As detailed in the error analysis section of the manuscript, this issue occurs because a single multi-hop query can be decomposed in various ways. For instance, the query "What year did the publisher of Labyrinth end?" can be decomposed as: 'Q1) What is Labyrinth?  Q2) <Q1> Who is the publisher of Labyrinth?  Q3) <Q2> What year did the publisher of Labyrinth end?. The subquery Q1, which acts as the initial node, is an unnecessary step that disrupts the performance of the model, as it does not contribute directly to answering the final question and adds complexity to the retrieval process. 

The NER (Named Entity Recognition) and synonymous word extraction step contributes the smallest portion (2%) to the error. This is primarily due to the LLMs' inability to recognize certain keywords present in the text, which leads to missed entities or synonyms that could have improved the retrieval process.

**Comments Suggestions And Typos - 1:**
We appreciate the reviewer’s suggestion to move Figure 2 to page 3. We will make this adjustment to improve the flow of the paper and ensure better clarity.

**Comments Suggestions And Typos - 2:**
Graph density measures how close a graph is to being complete. For an undirected graph KG with V being the set of nodes (vertices) and E being the set of edges, the density D is calculated as:

$D = \frac{2 * |E|}{|V| * (|V| - 1)}$ 

Here $|.|$ denotes the cardinality of the set.

We will now include a formal definition and explanation of how Graph Density is computed.

**Comments Suggestions And Typos - 3:**
We appreciate the reviewer’s careful reading. We have identified the missing words in Line 399 and have corrected the sentence for clarity and completeness as shown here.
"GPT-4o model retrieves relevant entities accurately in the benchmark datasets"
