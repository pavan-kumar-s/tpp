Reviewer 1: 
We thank the reviewer for the useful comments. We will revise the manuscript that incorporates all reviewers’ comments while submitting for Camera-ready papers<br>
**Weakness-1:**<br>
Thank you for your insightful comment on the need for a more formalized definition for weight assignment. We will now include a precise mathematical formulation. 
For a subgraph query, **$QG \in \mathbb{R}^{k}$** with query nodes sorted in topological order, ($V_{q,1}$, $V_{q,2}$, $\cdots$, $V_{q,k}$), the weighted similarity of the retrieved subgraph, **$SG$** = ($V_{1}$, $V_{2}$, $\cdots$, $V_{k}$) is given by,
<br> <br> $weight_{SG} = \sum_{i=1}^k \frac{similarity(V_{q,1}, V_1)}{i}$ <br>
Here, the similarity score is the cosine similarity between the embedding of the query node, $V_{q,i}$, and the embedding of the knowledge graph node, $V_{i}$. This weighted score for a subgraph ensures that more weightage is given to predecessor nodes in the topological order. By prioritizing earlier nodes, the approach captures the hierarchical influence within the query structure.

|    Method    | HotpotQA ||2WikiMQA||MuSiQue||Average||
|------|------|------|------|------|------|------|------|------|
|  | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 | Recall@2 | Recall@5 |
| BM25 [1]   | 55.40    | 72.20    | 51.80    |  61.90    | 32.30    | 41.20    | 46.50    | 58.43   |
| Contriever [2]  | 57.20   | 75.50   | 46.60   | 57.50   | 34.80   | 46.60   | 46.20   | 59.87   |
| GTR  [4] | 59.40   | 73.30   | 60.20  |  67.90   | 37.40  | 49.10  | 52.33   | 63.43   |
| Proposition [5]  | 58.70   | 71.10  | 56.40   | 63.10   | 37.60   | 49.30   | 50.9   | 61.17   |
| RAPTOR [6]  | 58.10   | 71.20   | 46.30   | 53.80  | 35.70  |  45.30   | 46.70   | 56.77   |
| naiveRAG [8]  | 73.65  | 88.35   | 57.93   | 68.48   | 41.24   | 57.57   | 57.61   | 71.47   |
| HippoRAG  [9] | 60.05   | 78.10   | 70.40   | 87.87   | 41.86   | 53.37   | 57.44   | 73.11   |
| BrowseNet (GliNER)   | 69.40   | 84.55   | 66.60   | 86.80   | 43.97   | 60.46   | 59.99   | 77.27   |
| BrowseNet (GPT-4o)   | 68.80   | 83.95   | 65.68   | 84.60   | 45.21   | 60.23   | 59.89   | 76.26   |


|  TF-TDF [3] | 20   | 21   | 22   | 23   | 24   | 25   | 26   | 27   |

| Blended RAG [7]  | 56   | 57   | 58   | 59   | 60   | 61   | 62   | 63   |


