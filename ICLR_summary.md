We have uploaded the revised manuscript incorporating the changes suggested by the reviewers after performing the required experiments.

The following are the changes made:
1) We have modified the title, text, figure and the pseudo code to call the constructed graph as graph-of-chunks rather than knowledge graph. (Jw6M, 9MFQ, Dpwg)
2) (lines 237-242): We have provided explanation on how the sparse properties that result in better cost compared to the cost incurred because of $k^p$ cost. Also the details of cost comparison are provided in Appendix A.6 and the same has been summarised in main text (lines 367-370). (9MFQ)
3) (Appendix: A-11, Table 15): The implementation details and results for statistical significance of the retrieval results are provided. The reference to the same has been provided in the main text. (9MFQ)
4) (lines  308-311): We have provided details on the algorithm used for isomorphic accuracy. (9MFQ)
5) (lines 174-179): We have provided the reason for decomposing the query as directed acyclic graphs. (9MFQ)
6) (Figure-2) A snippet subgraph of graph-of-chunks derived from the 2WikiMultiHopQA is presented in the Figure-2. (Jw6M)
7) (lines 232-235, Figure-3): We have clarified how the proposed graph traversal approach is related to beam search method. (Jw6M)
8) We have updated the notation used for cosine similarity in Equation-1 and in pseudocode Algorithm-1. (Jw6M)
9) (Table-3) We have included the question answering results from the new benchmark SiReRAG. (oVjo, Dpwg)
10) (lines 268-272) We have clarified how the benchmark datasets were modified to include the distractor passages from other questions too that reflect the real-world use cases (oVjo, Dpwg)
11) (lines 283-289) We have clarified the fairness in comparison on the kind of generative-LLMs used for all the baselines in this work. (oVjo)
12) (lines 207-209) We have clarified how BroseNet falls back to semantic search if the query cannot be decomposed. (oVjo)

