Reviewer 1: 
We thank the reviewer for the useful comments. We will revise the manuscript that incorporates all reviewers’ comments while submitting for Camera-ready papers<br>
**Weakness-1:**<br>
Thank you for your insightful comment on the need for a more formalized definition for weight assignment. We will now include a precise mathematical formulation. 
For a subgraph query, **$QG \in \mathbb{R}^{k}$** with query nodes sorted in topological order, ($V_{q,1}$, $V_{q,2}$, $\cdots$, $V_{q,k}$), the weighted similarity of the retrieved subgraph, **$SG$** = ($V_{1}$, $V_{2}$, $\cdots$, $V_{k}$) is given by,
<br> <br> $weight_{SG} = \sum_{i=1}^k \frac{similarity(V_{q,1}, V_1)}{i}$ <br>
Here, the similarity score is the cosine similarity between the embedding of the query node, $V_{q,i}$, and the embedding of the knowledge graph node, $V_{i}$. This weighted score for a subgraph ensures that more weightage is given to predecessor nodes in the topological order. By prioritizing earlier nodes, the approach captures the hierarchical influence within the query structure.

**Weakness-2:**<br>
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
| Blended RAG [3]  | <ins>69.65</ins>   | <ins>85.35</ins>   | 57.37   | 67.80   | 42.04   | 56.34   | 56.35   | 69.83   |
| GTR  [4] | 59.40   | 73.30   | 60.20  |  67.90   | 37.40  | 49.10  | 52.33   | 63.43   |
| Proposition [5]  | 58.70   | 71.10  | 56.40   | 63.10   | 37.60   | 49.30   | 50.9   | 61.17   |
| RAPTOR [6]  | 58.10   | 71.20   | 46.30   | 53.80  | 35.70  |  45.30   | 46.70   | 56.77   |
| naiveRAG [7,8]  | **73.65**  | **88.35**   | 57.93   | 68.48   | 41.24   | 57.57   | 57.61   | 71.47   |
| HippoRAG  [9] | 60.05   | 78.10   | **70.40**   | **87.87**   | 41.86   | 53.37   | 57.44   | 73.11   |
| BrowseNet (GliNER)   | 69.40   | 84.55   | <ins>66.60</ins>   | <ins>86.80</ins>   | <ins>43.97</ins>   | **60.46**   | **59.99**   | **77.27**   |
| BrowseNet (GPT-4o)   | 68.80   | 83.95   | 65.68   | 84.60   | **45.21**   | <ins>60.23</ins>   | <ins>59.89</ins>   | <ins>76.26</ins>   |

Answer generation results in terms of Exact match (EM) and F1-score (F1) are reported in the following table. The number of chunks used as input to the LLM for HippoRAG and BrowseNet is ten.

|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| LightRAG [10]  |  9.90  | 20.20   | 2.50   | 12.1  | 2.00  | 9.30    | 4.80   |  13.87  |
| GraphRAG [11]  | 51.40  | 67.6   | 45.70   | 61.0   | 27.00   | **42.0**  |41.37    | **56.87**   |
| HippoRAG  [9] | 44.30   | 60.53   | 50.00   | <ins>63.06</ins>   | 22.00   | 35.06   | 38.77   | 52.88   |
| BrowseNet (GliNER)   | **56.60**   | **70.46**   | **56.40**   | **63.57**   | **28.30**   | 35.79   | **47.10**   | <ins>56.61</ins>   |
| BrowseNet (GPT-4o)   | <ins>56.30</ins>   | <ins>69.37</ins>   | <ins>54.50</ins>   | 61.46   | <ins>27.70</ins>   | <ins>35.97</ins>   | <ins>46.17</ins>   | 55.60   |

References:
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
    
**Weakness-3:**<br>
We appreciate the reviewer’s feedback on the need to analyze critical parameters such as the ColBERT synonymity threshold. In response, we have conducted a detailed sensitivity analysis on the synonymity threshold as shown below. The NER model used below for this analysis is GLiNER.
| Synonymity threshold | Edge Accuracy (2WikiMQA) | Edge Accuracy (MuSiQue) | Recall@5 (HoptpotQA)| Recall@5 (2WikiMQA)| Recall@5 (MuSiQue)|
|------|------|------|------|------|------|
|0.7|100|97.43|84.85|85.22|59.78|
|0.8|99.86|95.18|85.30|85.65|59.86|
|0.9|99.86|94.78|84.55|86.80|60.45|
The table suggests that the recall@5 metric remains consistent across all datasets used in the study. However, edge accuracy tends to be higher when the synonymity threshold is low. Additionally, as the threshold decreases, leading to a denser graph, the retrieval time is expected to rise due to the greater number of potential neighbors for each node. This indicates a trade-off between efficiency and effectiveness: a lower threshold increases retrieval time due to greater graph density, while a higher threshold reduces it.




