We have uploaded the revised manuscript incorporating the changes suggested by the reviewers, along with the additional experiments conducted in response to their comments.

The following are the changes made:
1) We have revised the title, text, figure and the pseudo code to call the constructed graph as graph-of-chunks rather than knowledge graph. (Jw6M, 9MFQ, Dpwg)
2) (lines 237-242): We have added an explanation of how sparsity in the proposed graph results in lower computational cost compared to the $k^p$ cost. Detailed cost comparisons are provided in Appendix A.6, with a summary included in the main text. (lines 367-370). (9MFQ)
3) (Appendix: A-11, Table 15): We have included implementation details and statistical significance results for the retrieval performance. Corresponding references have been added to the main text. (9MFQ)
5) (lines 308-311): We have provided details of the algorithm used to compute isomorphic accuracy. (9MFQ)
6) (lines 174-179): We have clarified the motivation for decomposing queries into directed acyclic graphs. (9MFQ)
7) (Figure-2) We have added a snippet subgraph of the graph-of-chunks derived from the 2WikiMultiHopQA dataset. (Jw6M)
8) (lines 232-235, Figure-3): We have clarified the relationship between the proposed graph traversal strategy and the beam search method. (Jw6M)
9) We have updated and unified the notation for cosine similarity in Equation (1) and Algorithm 1. (Jw6M)
10) (Table-3) We have included the question answering results for the new method, SiReRAG. (oVjo, Dpwg)
11) (lines 268-272) We have clarified how benchmark datasets were modified to include distractor passages from other questions, thereby better reflecting real-world scenarios. (oVjo, Dpwg)
12) (lines 283-289) We have clarified the fairness of the experimental comparison, specifically with respect to the use of generative LLMs across all baselines. (oVjo)
13) (lines 207-209) We have clarified how BrowseNet falls back to semantic search when a query cannot be decomposed. (oVjo)

Finally, two reviewers indicated that they increased their ratings after reading our responses. We have also addressed all remaining reviewer comments to the best of our ability and believe that the revised manuscript satisfactorily resolves the concerns raised.
