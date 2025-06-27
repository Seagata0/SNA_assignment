# **Ukraine Conflict Twitter Analysis Using NetworkX**
**Abstract—This report details the analysis of a Twitter or X dataset concerning conversations on X surrounding the Russo-Ukrainian conflict in 2022\. Utilizing data sourced from Kaggle, which was originally collected from twitter, this study focuses on the network structure of user interactions, specifically replies, to determine the most influential people. The dataset is preprocessed to extract user interactions (source and target), forming a directed graph network using NetworkX, to find the most influential people, and then do community detection using Louvain  and K-means Clustering. The analysis reveals a sparsely connected network, suggesting interactions primarily occur within smaller, potentially isolated groups or that users do not engage broadly across the entire conversation space; the community detected from the dataset incur that the majority of the tweets are condemning Russia, supporting Ukraine, and discussing while giving the updates regarding the war, the participants are from across the globe using various language to react and giving info/opinion regarding the war. The main limitations of the analysis being restricted to a single day's data due to computational constraints.**

**Keywords—NetworkX, Social Network Analysis, Page Rank, Degree Centrality, Community Detection**

## I.  Introduction

&nbsp;&nbsp;&nbsp;&nbsp;The Russo-Ukrainian conflict, Commencing in 2022, generated significant global discussion across various digital domains. X or formerly known as Twitter, emerged as a significant platform for sharing information, expressing opinion, and discussing surrounding these unfolding events.

&nbsp;&nbsp;&nbsp;&nbsp;This mid-term project endeavors to analyze the network structure of conversations on X pertaining to the conflict in Ukraine, to dish out the top 10 influential individuals using each centrality metrics. Specifically, the focus lies upon the reply network, where users directly engage with one another's tweets. Through constructing and subsequent analysis of this network, we seek to pinpoint the most influential figures on the network, employing Page Rank and Degree Centrality as instructed by our dear professor.

&nbsp;&nbsp;&nbsp;&nbsp;Further this study does community detection to get a better grasp on the conflict’s segmented opinion. The method employed for this purpose is the Louvain algorithm and K-means clustering for a machine learning approach. The Louvain is used for a user centric community detection, and the K-means used for tweet based community detection.

## II. Methodology

### *A. Data Acquisition and Preprocessing*

&nbsp;&nbsp;&nbsp;&nbsp;The dataset utilized within this project is hailed from Kaggle \[1\], having been meticulously gathered through the Twitter API during the conflict in 2022\. Owing to computational power limitations, the analysis is limited to data corresponding to a single day '19 August 2022'.

&nbsp;&nbsp;&nbsp;&nbsp;The dataset is filled with the very essence of tweets itself i.e. consisting of the tweets, source of the tweet, reply to who, time, day, etc. Though all the dataset can be used for various things, this project focused on building the network and dissecting its essences.

&nbsp;&nbsp;&nbsp;&nbsp;The raw dataset underwent a preprocessing procedure that seems sufficient and necessary. Rows exhibiting null values in the \`in\_reply\_to\_screen\_name\` column were excluded, as these entries signify tweets that do not constitute direct replies within the observed temporal scope or contextual framework of the dataset. 

&nbsp;&nbsp;&nbsp;&nbsp;In the realm of the Louvain algorithm, solely the \`username\` (indicating the source of the reply) and \`in\_reply\_to\_screen\_name\` (indicating the target of the reply) columns were preserved for its use. These columns were subsequently designated as "Source" and "Target", respectively, to accurately represent the directed edges within the network graph structure. 

&nbsp;&nbsp;&nbsp;&nbsp;As for the machine learning approach, KNN Method here is deemed to be fitting and useful for the job, hereby it is being used by us to be deployed for community detection. The dataset first underwent a simple null removal in the columns of 'text','username', and 'in\_reply\_to\_screen\_name'. The 'text' column underwent a similar treatment, to remove the unnecessary word like link and extra space. After such a meticulous process, the last play is to vectorize the column ‘text’ for KNN.

&nbsp;&nbsp;&nbsp;&nbsp;The refined data was subsequently stored in a Comma-Separated Values (CSV) file format, named \`edges.csv\` to be used in Python as a Data Frame (df).

### *B. Network Construction*
&nbsp;&nbsp;&nbsp;&nbsp;A directed graph (DiGraph) was instantiated from the preprocessed data employing the \`networkx\` library within the Python programming language, executed in a Google Colaboratory (Colab) environment. The \`pandas\` library facilitated the ingestion of the \`edges.csv\` file and converted it into a more manageable Data Frame, that can be easily dissected by python. Each distinct username corresponds to a node within the graph, and a directed edge is established from node A (Source) to node B (Target) contingent upon user A having replied to user B.

### *C. Network Analysis Metrics*
*1) Network Structure Metrics:* The Basic network properties were calculated for context:<br>
&nbsp;&nbsp;&nbsp;&nbsp;*a) Network Size:* Showing the number of nodes (users) and edges (replies). <br>
&nbsp;&nbsp;&nbsp;&nbsp;*b) Network Density:* Used to understand the overall network connectivity sparsity.<br>

*2) Influence Metrics (Centrality Analysis):* The core analysis focused on identifying influential users using:<br>
&nbsp;&nbsp;&nbsp;&nbsp;*a) Degree Centrality:* Calculated for each node. In a directed graph, includes:<br>
&nbsp;&nbsp;&nbsp;&nbsp; -  *In-degree:* Number of replies received by a user, indicating direct popularity or attention received (acting as the user B).<br>
&nbsp;&nbsp;&nbsp;&nbsp; -  *Out-degree:* Number of replies sent by a user, indicating activity level (acting as the user A). <br>
&nbsp;&nbsp;&nbsp;&nbsp;*b) PageRank :* Calculated using algorithms within NetworkX. PageRank assesses a node's importance based not just on the number of incoming links but also on the importance of the nodes linking to it.<br>
&nbsp;&nbsp;&nbsp;&nbsp;*c) Closeness Centrality*: a reciprocal of the sum of the shortest paths from a node to all others; measures the average distance between a node and all other nodes. In other words it means someone that is the center of many connections, that can spread information the fastest.  <br>
&nbsp;&nbsp;&nbsp;&nbsp;*d) Betweenness Centrality*: the number of shortest paths from all nodes to all others that pass through that node; measures the frequency of occurrence of a node on the shortest paths between network nodes, in a simpler term the people that most oftenly act as the bridge of connection for shortest path.  

*3) Social Contagion (Contagion):* a method of transmission that does not rely on a direct intent to influence; in normal language it means the method or process of an influence (can be another thing) to pass between people via social links  <br>
&nbsp;&nbsp;&nbsp;&nbsp;*a) Information Diffusion*: process by which a piece of information (knowledge) is spread and reaches individuals through interactions.

### *D. Network Visualization (Gephi)*
&nbsp;&nbsp;&nbsp;&nbsp;Visualization was executed using Gephi after exporting the graph data. Layout algorithms (Yifan Hu Proportional, ForceAtlas 2\) were used for spatial organization. Crucially, node attributes were mapped to visualize influence:<br>
*1) Node Size:* Scaled proportionally to PageRank scores to highlight globally influential nodes. Optionally, size could also represent In-Degree Centrality for direct comparison.<br>

*2) Node Color:* Determined based on community structures to show the context in which influence operates (just for clarity).

### *E. Community Detection*
&nbsp;&nbsp;&nbsp;&nbsp;Louvain is a heuristic algorithm\[6\], that try to divide the network to maximize the modularity

*1) Cliques:* In graph theory and network analysis, a clique is rigorously defined as a "subset of vertices in which every pair of vertices are adjacent to each other" \[2\]. <br>

*2) Modularity:* Modularity is a scalar metric that "quantifies the density of links within communities compared to links between communities" \[5\]. Typically modularity spreads between \-1 and 1, the higher the modularity the more robust and well-defined the community structure is. <br>

*3) Louvain Method:* The Louvain algorithm by Blondel et al. (2008) is very simple and elegant. The algorithm optimises a quality function such as modularity or CPM in two elementary phases: (1) local moving of nodes; and (2) aggregation of the network. \[3\], the 2 steps can be translated as (1)Modularity Optimization and (2)Community Aggregation. <br>

*4) K-Means Clustering:* K-Means Clustering is an Unsupervised Machine Learning algorithm which groups unlabeled dataset into different clusters. It is used to organize data into groups based on their similarity. 

## References
1.  Bwandowando, "Ukraine-Russian Crisis Twitter Dataset (1-2 M rows)," Kaggle. \[Online\]. Available: https://www.kaggle.com/datasets/bwandowando/ukraine-russian-crisis-twitter-dataset-1-2-m-rows/data. \[Accessed: Mar. 16, 2025\]

2. D. Palsetia, Md. M. A. Patwary, W. Hendrix, A. Agrawal, and A. Choudhary, “Clique guided community detection,” 2021 IEEE International Conference on Big Data (Big Data), vol. 4, pp. 500–509, Oct. 2014, doi: 10.1109/bigdata.2014.7004267.

3. V. A. Traag, L. Waltman, and N. J. Van Eck, “From Louvain to Leiden: guaranteeing well-connected communities,” Scientific Reports, vol. 9, no. 1, Mar. 2019, doi: 10.1038/s41598-019-41695-z.

4. N. Dugué and A. Perez, “Directed Louvain : maximizing modularity in directed networks,” Nov. 20, 2015\. [https://hal.archives-ouvertes.fr/hal-01231784](https://hal.archives-ouvertes.fr/hal-01231784)

5. N. R. Smith, P. N. Zivich, L. M. Frerichs, J. Moody, and A. E. Aiello, “A guide for choosing community detection Algorithms in social network Studies: The question Alignment Approach,” American Journal of Preventive Medicine, vol. 59, no. 4, pp. 597–605, Sep. 2020, doi: 10.1016/j.amepre.2020.04.015.

6. S. Sahu, “Heuristic-based Dynamic Leiden algorithm for efficient tracking of communities on evolving graphs,” arXiv (Cornell University), Oct. 2024, doi: 10.48550/arxiv.2410.15451.

7. U. Brandes et al., “Maximizing Modularity is hard,” arXiv (Cornell University), Jan. 2006, doi: 10.48550/arxiv.physics/0608255.

8. S. Wasserman and K. Faust, Social network analysis. 1994\. doi: 10.1017/cbo9780511815478.
