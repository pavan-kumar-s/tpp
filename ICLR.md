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

**Action:** We will clarify this analogy to beam search in the paper and explicitly describe the candidate combination, scoring, and pruning process during subgraph retrieval with a figure in appendix to improve reader understanding.


# Reviewer oVjo:
We thank the reviewer for their valuable feedback. Our response to the comments are as follows

**Weakness-1:**
The core idea focuses on context retrieval, but the novelty is limited since query decomposition and graph-based iterative retrieval have been explored previously; comparison to similar methods (e.g., SiReRAG, ArchRAG, GraphRAG) is missing.

We agree these approaches have been studied before; however, the novelty of BrowseNet lies in its unified pipeline that integrally combines query decomposition, graph-of-chunks construction, and efficient one-shot retrieval, which to our knowledge is not previously realized. In response to the reviewer’s suggestion, we have benchmarked BrowseNet against related methods SiReRAG and GraphRAG. Unfortunately, the ArchRAG code is not publicly available, and the anonymous access link appears expired. We have contacted the authors and will include comparisons once the code becomes accessible.

Below are comparison results on HotpotQA, 2WikiMQA, and MuSiQue datasets using the gpt-4o-mini answer generation model with top-5 chunks provided:


|    Method (K=5)  | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| Browsenet (gpt-4o-mini) | **62.20**  | **77.69**   | **63.90**   | **74.50**  | **41.60**  | **54.08**    | **55.90**   |  **68.76**  |
| SiReRAG (gpt-4o-mini)  | 48.30  | 63.17  | 41.30   | 48.05   | 26.00   | 39.59  | 38.53  |50.27   |
| GraphRAG (gpt-4o-mini) | 51.40   | 67.60   | 45.70   | 61.00   | 27.00   | 42.00   | 41.37   | 56.87   |

In the main paper of SiReRAG, top-20 chunks were provided, hence for comparison with BrowseNet, we have implimented again with top-20 chunks and the results for that are shown below.

|    Method (K=20)   | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | EM | F1 | EM | F1 | EM | F1 | EM | F1 |
| Browsenet (gpt-4o-mini) | **62.20**  | **78.36**   | **68.20**   | **78.47**  | **44.50**  | **57.77**    | **58.3**   | **71.53**   |
| SiReRAG (gpt-4o-mini)  | 52.80  | 69.17  | 46.10   | 54.82   | 29.80   | 44.96  | 42.90  |  56.32  |

As seen from both the Tables, BrowseNet achieves superior performance, establishing itself as the state-of-the-art in this retrieve-then-read framework setting. As both of these methods do not follow retrieve-then-read paradigm, we did not include the results for retrieval.

**Action:** We will incorporate these comparison results with SiReRAG and GraphRAG (for K=5 ) into the paper’s result section. 

**Weakness-2:**
Scalability to large, real-world corpora and real-time use cases is not clearly addressed, as experiments rely on controlled corpora with gold evidence and distractors.

To better reflect real-world use cases, we have modified the benchmark datasets (HotpotQA, MuSiQue, and 2WikiMultiHopQA) by including all passages from other questions as candidate distractors. As shown in Table-1, the number of nodes reflects the number of passages that are given as candidate passages for all the questions in the benchmark dataset. The number of passages for the benchmarks are 9,221, 6,119 and 11,656 respectively for HotpotQA, 2WikiMQA and Musique. This effectively enlarges the candidate corpus, simulating a more realistic retrieval setting where numerous irrelevant documents must be filtered.

**Action**: We will clearly describe this benchmark modification in the manuscript to demonstrate BrowseNet’s applicability and robustness in larger, more challenging retrieval scenarios.

**Weakness-3:**
Fairness of comparison is unclear, particularly whether the same backbone models (generation, NER, embeddings) were used across baselines, which may confound reported improvements.

Thank you for raising the concern regarding the fairness of comparison. We clarify that simple baselines and dense retrievers do not utilize LLMs. For comparable methods such as RAPTOR, HippoRAG, HippoRAG-2, LightRAG, GraphRAG and SiReRAG, we have consistently implemented gpt-4o-mini for indexing, retrieval, and question answering to ensure a fair evaluation. Specifically, HippoRAG-2 requires an embedding model, for which we used NV-Embed-v2 the same embedding model employed by BrowseNet. Furthermore, as reported in the main results and ablation studies, BrowseNet demonstrates superior performance using NV-Embed-v2 and maintains robustness regardless of the choice of NER models (GliNER, gpt-4o, Claude-3.7-sonnet), subquery decomposition models (gpt-4o, Deepseek Reasoner, Claude-3.7-sonnet, gpt-4o-mini), or generation models (gpt-4o-mini, gpt-3.5-turbo, gpt-4.1-mini, Deepseek-chat-v3, Gemini-2.0-flash).

**Action**: We will clarify these backbone model settings across baselines and our method to emphasize fairness and robustness of the comparison.

**Weakness-4:**
The current framework is optimized for structured multi-hop reasoning and may not directly generalize to open-domain retrieval or tasks with unstructured context dependencies.

BrowseNet is designed to be robust to varying query structures. When a query cannot be decomposed into subqueries, retrieval relies solely on initiator nodes, effectively performing a semantic search over the entire corpus. Additionally, during retrieval for each subquery, similarity scores are also computed between chunks and the original multi-hop query, allowing the system to maintain relevance to the full query context. Also, BrowseNet only requires keyword prediction for constructing the graph-of-chunks, hence it creates structured context from the unstructured data and works for any multi-hop reasoning setting. Exploring how performance improves by leveraging such structural preprocessing remains an interesting direction for future work.

**Action:** We will include this point in the final revised version of the manuscript

**Weakness-5:**
The approach depends on LLM-based query decomposition, which may introduce structural or semantic errors when sub-queries are misgenerated, leading to cascading retrieval failures.

Query decomposition is a critical element of our approach and hence we have performed a detailed error analysis on the subquery generation on answer generation. We have presented the error analysis in Appendix A.10.


# Reviewer 9MFQ:
We thank the reviewer for their valuable feedback. Our response to the comments are as follows

**Weakness-1 and Question-1:**
While the results are competitive, other claims such as BrowseNet minimizing dependence on LLMs are not really backed by how the method is desgined. It relies on LLMs at several stages, for creating the KG, embedding chunks, decomposing queries, etc. It is therefore not clear what the actual cost is in comparison with other methods.
Could you clarify in what sense the dependence on LLMs is reduced, or whether the key benefit lies more in structuring the outputs of LLMs rather than avoiding them altogether? Have you quantified the computational or monetary cost of these LLM-dependent components relative to baseline RAG methods?

We agree that BrowseNet uses LLMs for several components, including graph-of-chunks (KG) construction, chunk embedding, and query decomposition. In our paper, we use "LLMs" to specifically refer to models like GPT-class systems that perform text generation. In contrast, keyword generation (GliNER model) and embedding (NV-embed-V2 model) rely on smaller local models that do not incur the high computational or monetary cost associated with large generative LLM calls.

**Clarifying our claim on reduced LLM dependence.**
Our claim is not that BrowseNet eliminates LLMs altogether, but that it reduces reliance on generative LLMs during the retrieval phase. Existing multi-step RAG approaches (e.g., [1, 2, 3]) repeatedly invoke a generative LLM to iteratively decompose the query based on previously retrieved context. This can result in multiple expensive LLM calls per query. In contrast, BrowseNet uses the graph-of-chunks to guide retrieval and requires only a single generative LLM call for query decomposition. After this step, the retrieval procedure is entirely graph-guided and does not require additional generative LLM invocations. Although these iterative query-planning methods are well known, the HippoRAG paper has already shown them to be less effective than graph-based approaches; therefore, we did not include them as baselines but highlighted them briefly in the introduction for completeness.

**Cost comparison.**
Regarding computational cost, we have quantified the LLM-related overhead in Appendix A.7. Our analysis shows that BrowseNet achieves approximately 33× higher cost-efficiency than HippoRAG-2 while maintaining state-of-the-art retrieval performance. This improvement comes largely from avoiding repeated generative LLM calls.

**Action:**
We will clarify this distinction in the manuscript, explicitly state the meaning of minimizing LLM dependence during retrieval.
 
**References:**
1) Harsh Trivedi, Niranjan Balasubramanian, Tushar Khot, and Ashish Sabharwal. Interleaving retrieval with chain-of-thought reasoning for knowledge-intensive multi-step questions. arXiv preprint arXiv:2212.10509, 2022a.
2) Yao Yao, Zuchao Li, and Hai Zhao. Beyond chain-of-thought, effective graph-of-thought reasoning in language models. arXiv preprint arXiv:2305.16582, 2023.
3) Yu Wang, Nedim Lipka, Ryan A Rossi, Alexa Siu, Ruiyi Zhang, and Tyler Derr. Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI Conference on Artificial Intelligence, volume 38, pp. 19206–19214, 2024.

**Weakness-2 and Question-2:**
An important source of cost is in retrieval for non-initiator nodes. This requires considering a total of k^p chunks that need to be scored, increasing the cost of the method. The retrieval process for non-initiator nodes appears to involve scoring k^p candidates. Could you provide more details about how this complexity behaves in practice?

Thank you for this important question. We clarify that while the k^p complexity is theoretically valid, practical factors might mitigate this concern.
1. Sparse Query Structures: Most evaluation datasets have shallow, sequential dependencies (p ≤ 4). Even with k=5 and p=2, we evaluate only 25 combinations.
2. Graph-Based Pruning: Candidate chunks are restricted to knowledge graph neighbors (Algorithm 1, lines 11-12), drastically reducing the effective search space versus corpus-wide evaluation.
3. Empirical Efficiency: BrowseNet's retrieval stage averages **1.19 seconds per query** (MuSiQue), with only **0.49 seconds additional overhead** compared to HippoRAG-2, while achieving substantially higher recall (R5: 93.30 vs. 90.20).
4. Beam Search Pruning: We retain only top-k scoring subgraphs, enabling early termination and further constraining computational cost.
5. Ablation studies (Table 4) show that increasing subgraph count from 5 to 15 yields <0.1% performance gains, confirming we operate near the efficiency frontier.

**Weakness-3 and Question-3:**
The query decomposition is based on a directed acyclic graph, which might lead to a limitation for queries that do involve cycles or cannot be expressed as a DAG. Whether this is a source of issues is not discussed. How would BrowseNet handle queries that involve cycles, mutual dependencies, or other forms of recursive reasoning?

We appreciate the opportunity to clarify our design choice regarding DAG-based query decomposition.
1. Inherent Nature of Question Answering:
Question answering tasks are fundamentally acyclic by design. A well-posed question cannot logically require its own answer as a prerequisite—doing so would create an infinite loop and violate the principle of well-founded reasoning. For example, if Q1 requires Q2 to be answered, and Q2 requires Q1, neither question can be answered, making the query semantically ill-defined.
2. Dataset Evidence:
Across our evaluation benchmarks (HotpotQA, 2WikiMQA, MuSiQue), all gold decomposition chains are acyclic. No instances of cycles or mutual dependencies appear in naturally-occurring multi-hop questions. This suggests that queries expressible as DAGs capture the full spectrum of practical QA scenarios.
3. Design Justification:
By restricting decomposition to DAGs, we ensure:
- Termination guarantees: Every subquestion has a well-defined base case
- Tractable reasoning: No infinite loops or circular dependencies
- Semantic validity: Decompositions remain faithful to the underlying question structure
  
**Scope and Future Work**: We acknowledge that if a user intentionally constructs a query with cyclic dependencies (e.g., "Answer Q1 if Q2 is true, and answer Q2 if Q1 is true"), BrowseNet would not handle it by design. However, such queries fall outside the scope of standard QA tasks and would require fundamentally different reasoning approaches (e.g., fixed-point computation or constraint satisfaction). For practical multi-hop QA, DAG-based decomposition is both sufficient and necessary.

**Weakness-4**
Calling the constructed graph a “knowledge graph” seems somewhat misleading, since the edges primarily encode textual similarity or shared entity mentions, rather than semantic relations between well-defined concepts. It might be more accurate to refer to it as a semantic chunk graph or entity-linked similarity graph. Clarifying this terminology would prevent confusion for readers coming from the KG community.

We recognize that a traditional knowledge graph involves entities as nodes and predicates as edges, while our graph is constructed over chunks of text, which suits retrieval-augmented generation tasks better. We initially followed prior work using the term "knowledge graph" [1][2] for similar graphs, but upon reflection, we agree the term could be clearer. We will revise the manuscript to replace "knowledge graph" with "graph of chunks" wherever appropriate

**Action:**
We will revise the manuscript to replace all instances of "knowledge graph" with "graph of chunks" to improve clarity and accuracy.

**References:**
1) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI conference on artificial intelligence (Vol. 38, No. 17, pp. 19206-19214).
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).

**Weakness-5**
Some improvements reported (e.g., Table 2, HotpotQA in Table 3) are small, and it’s unclear whether they are statistically significant. Including confidence intervals or significance tests (e.g., paired bootstrap) would increase confidence in the reported gains.

We agree that including confidence intervals and statistical significance testing would strengthen the validity of our reported improvements. We plan to apply paired bootstrap to estimate confidence intervals. We will include the confidence intervals obtained from paired bootstrap (n = 200) as shown in the below Table. 


|    Method    | HotpotQA ||2WikiMQA||MuSiQue||
|------|------|------|------|------|------|------|
|  | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 |
| NV-embed-V2  | **83.96±0.79**  | 95.68±0.43   | 68.99±0.70   |  76.65±0.78| 53.33±0.89 | 69.80±0.91  |  
| HippoRAG-2 |81.75±0.84 | 96.18±0.44  | 75.47±0.77   | 90.63±0.58   | 53.72±0.92   | 73.68±0.84  |  
| BrowseNet  | 83.90±0.81 |**96.40±0.42**  | **76.79±0.78**   | **93.30±0.46** |  **55.15±0.91**  | **73.88±0.90**   |

**Action:** We will include paired bootstrap-based confidence intervals in Table 2 of the revised manuscript to provide a more rigorous evaluation of BrowseNet’s performance gains.

**Question-4**
The isomorphic accuracy is an interesting way to measure the generated subgraphs. I assume that since the graphs are likely small, you used an exact algorithm for isomorphism check. Could you please elaborate on this?

Thank you for your interest in our evaluation methodology. We clarify the implementation details of the isomorphism check below.

**Algorithm Used:** We employ **NetworkX's `is_isomorphic()` function**, which implements the VF2 algorithm for graph isomorphism testing. This is an exact algorithm that provides deterministic results.

**Computational Feasibility**
As you correctly note, the query subgraphs in our evaluation datasets are small:
- Average node count: 2-4 nodes (corresponding to 2-4 subquestions)
- Maximum depth: 4 hops (MuSiQue dataset)
- Typical structure: Linear or tree-like dependencies with minimal branching

For graphs of this size, the VF2 algorithm runs in effectively constant time (< 1ms per comparison), making exact isomorphism checking tractable even across hundreds of test queries.

**Scalability Note**
For applications involving larger knowledge graphs or more complex queries, approximate graph matching algorithms could be substituted. However, for standard multi-hop QA benchmarks, exact isomorphism checking remains computationally efficient and provides the most rigorous evaluation metric.

# Reviewer Dpwg:
We thank the reviewer for their valuable feedback. Our response to the comments are as follows

**Weakness-1:**
Novelty is limited. KG construction (Section 3.1), query decomposition (Section 3.2.1), and answer generation (Section 3.3) are standard practice in the literature. The rest of the approach, KG traversal (Section 3.2.2), is heuristic and its generalizability is not well justified given experiments on only three datasets.

BrowseNet indeed builds on standard components such as KG construction, query decomposition, and answer generation, but the contribution lies in how these elements are integrated into a unified, graph–based associative retrieval pipeline and in the specific traversal and candidate-pruning strategy used for multi-hop QA.​

BrowseNet’s graph traversal is not a generic heuristic walk but a query-subgraph–guided process that combines: (i) LLM-based decomposition into an explicit query subgraph; (ii) alignment of subgraph nodes with initiator chunks via semantic similarity; (iii) constrained expansion over a content-centric graph rather than an entity-only KG; and (iv) multi-hop scoring that jointly considers local subquery relevance and consistency with the global multi-hop question. This design allows one-shot query decomposition followed by structured, bounded exploration, which differs from existing KG-RAG and GraphRAG variants that typically rely on iterative LLM-in-the-loop traversal or purely similarity-based expansion.​

Regarding generalizability, BrowseNet is evaluated on three challenging multi-hop benchmarks (HotpotQA, 2WikiMultiHopQA, MuSiQue), which together cover diverse reasoning patterns (bridge, comparison, compositional reasoning) across varied topics. Across these datasets, BrowseNet consistently outperforms strong graph- and structure-aware RAG baselines such as GraphRAG and SiReRAG in EM and F1, suggesting that the traversal strategy is not overfit to a single corpus or question style


**Weakness-2 and Quesion-1:**
Baselines are focused on recent RAG solutions, but the proposed approach is closely related to early multi-hop QA solutions, which are not compared empirically.
Can you compare your approach with early multi-hop QA methods that also build a graph of passages based on their similarity (e.g., common entities)?

Our baselines focus on state-of-the-art RAG solutions that currently achieve the best performance on multi-hop QA benchmarks. Early multi-hop QA methods such as KG-Prompting [1] employ a different evaluation setting: they construct one question-specific graph per query using the labeled distractors and golden passages provided for that question. In contrast, our method extends the benchmark difficulty by augmenting the candidate corpus to include all passages from other questions as distractors as mentioned in the text. This makes the retrieval task significantly harder and more realistic, where the system must select relevant context from a large, mixed set rather than a question-limited set of passages.

Because of this fundamental difference in setup, direct empirical comparison with early multi-hop QA methods is not feasible.

**References:**
1) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI conference on artificial intelligence (Vol. 38, No. 17, pp. 19206-19214).

**Weakness-3**
The constructed KG is actually not a KG but a graph of passages connected based on common entities. This is a common practice in conventional multi-hop QA research. It is not a KG that should represent relations between entities.

We recognize that a traditional knowledge graph involves entities as nodes and predicates as edges, while our graph is constructed over chunks of text, which suits retrieval-augmented generation tasks better. We initially followed prior work using the term "knowledge graph" [1][2], but upon reflection, we agree the term could be clearer. We will revise the manuscript to replace "knowledge graph" with "graph of chunks" wherever appropriate

**Action:**
We will revise the manuscript to replace all instances of "knowledge graph" with "graph of chunks" to improve clarity and accuracy.

**References:**
1) Wang, Y., Lipka, N., Rossi, R. A., Siu, A., Zhang, R., & Derr, T. (2024, March). Knowledge graph prompting for multi-document question answering. In Proceedings of the AAAI conference on artificial intelligence (Vol. 38, No. 17, pp. 19206-19214).
2) Yang, Z., Zhu, Z., & Zhu, J. (2025, April). CuriousLLM: Elevating multi-document question answering with llm-enhanced knowledge graph reasoning. In Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 3: Industry Track) (pp. 274-286).

**Question-2**
How do you demonstrate the generalizability of your heuristic graph traversal?

The generalizability of our heuristic graph traversal is demonstrated through consistent strong performance across three diverse multi-hop QA benchmarks, HotpotQA, 2WikiMultiHopQA, and MuSiQue—which encompass varied reasoning types including bridge, comparison, and compositional questions in multiple domains. These datasets differ significantly in corpus size, question complexity, and domain, yet BrowseNet’s traversal strategy consistently outperforms competitive graph- and structure-aware retrieval baselines such as HippoRAG and SiReRAG. This empirical evidence indicates that our traversal heuristic effectively adapts to different reasoning requirements and corpus characteristics without being overfitted to a single dataset or domain.
