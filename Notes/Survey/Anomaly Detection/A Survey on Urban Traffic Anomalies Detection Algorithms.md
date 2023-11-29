<h2>1. Abstract</h2>
Divide existing solutions into two categories: flow outlier detection and trajectory outlier detection. First group includes: statistical, similarity and pattern mining approaches. Second group includes: offline processing for trajectory outliers and online processing for sub-trajectory outliers. Main contribution: provide a deep analysis in urban traffic data, including flow values, segment flow values, trajectories and sub-trajectories.
<h2>2. Introduction</h2>
- Flow outlier detection:
	- Statistical: 
		- Univariate: DPMM [1], KSNB [2], RADT [3].
		- Multivariate: MSQC [4], MB-MSQC [5].
	- Similarity: 
		- Distance: kNN-F [6], CSW [7], DSTF [8].
		- Density: BLOF [9], SOM-RF [10].
	- Parrern mining: 
		- Casual Interaction: CI-OF [11], PM-STOD [12], DM-TF [13].
		- Congested Patterns: Fpgrowth-CP [14], STCTree [15].
- Trajectory outlier detection:
	- Offline processing: MonaVTT [16], LoTAD [17], TPRO [18], TPRRO [19], iBAT [20], PBOTD [21], OTD-UTF [22], ClustiVAT [23].
	- Online Processing: iBOAT [24], TROAD [25], PN-Outlier [26], TN-Outlier [27], DB-TOD [28], TF-Outlier [29], TODCSS [30].
<h2>3. Flow outlier detection algorithms</h2>
Four main steps: i) urban sensing and data acquisition (capture data related to urban traffic flow by deploying sensors, GPS devices and other IT infrastructures), ii) data collection (collect and organize data according to the application used flow values or segment flow values), iii) outlier detection (data collected are used to find outliers) and iv) output three kind of outliers: single flow outliers, congested roads and interaction between road outliers. 
<h2>4. Trajectory outlier detection algorithms</h2>
Three main steps: i) Pre-processing (collect the trajectories database and information related to the road network of urban city. The mapping function allows generation of the mapped trajectory database), ii) outlier detection (the mapped trajectory database is entered to the outlier detection algorithms including clustering, density and distance approaches to find trajectory outlier) and iii) output visualization.
<h2>5. Discussion</h2>
<h3>5.1 Flow outliers</h3>
- Merits:
	- Statistical: low time consumption.
	- Similarity: employ neighborhood computation methods to derive outliers.
	- Pattern mining: consider correlation between flow outliers.
- Limits:
	- Statistical: difficult to find the appropriate distribution of the given traffic flow data
	- Similarity: ignore correlation between the single flows.
	- Pattern mining: multiple scanning of the flow database.
<h3>5.2 Trajectory outliers</h3>
- Merits:
	- Offline processing: fast time consumption.
	- Online processing: finds sub-trajectory outliers.
- Limits:
	- Offline processing: finds only whole trajectory outlier.
	- Online processing: high time consumption.
<h3>5.3 Future research directions</h3>
- Improving the run-time performance of existing pattern mining approaches by adapting recent pattern mining approaches.
- Improve hot spot and crime detection by considering the correlation between single flow outliers.
- Need to deal with other patterns such as constructing the distribution of flows in a given time measurement and finding the distribution of flows that deviate from the normal distribution of flows in a given period time.
- Improving the run-time performance of online algorithms to find sub-trajectory outliers by adapting computational intelligence approaches and high performance computing.
<h2>6. References</h2>
[1] https://ietresearch.onlinelibrary.wiley.com/doi/full/10.1049/iet-its.2014.0063.
[2] https://arxiv.org/abs/1512.08413.
[3] https://journals.sagepub.com/doi/10.1177/0361198106195700108.
[4] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=881011.
[5] https://journals.sagepub.com/doi/10.3141/1840-03.
[6] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7251924.
[7] https://www.sciencedirect.com/science/article/pii/S0198971517301096.
[8] https://www.sciencedirect.com/science/article/pii/S019897151730042X.
[9] https://www.researchgate.net/publication/308026147_Traffic_Outlier_Detection_by_Density-Based_Bounded_Local_Outlier_Factors.
[10] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=4357769.
[11] http://urban-computing.com/pdf/Discovering20Spatio-Temporal20Causal20Interactions20in20Traffic20Data20Streams-kdd202011.pdf.
[12] https://link.springer.com/chapter/10.1007/978-3-642-25856-5_18.
[13] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6413908.
[14] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7795635.
[15] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7511741.
[16] https://dl.acm.org/doi/10.1145/2346496.2346521.
[17] https://link.springer.com/article/10.1007/s11280-017-0487-4.
[18] https://link.springer.com/chapter/10.1007/978-3-319-26190-4_2.
[19] https://link.springer.com/article/10.1007/s11280-016-0400-6.
[20] https://dl.acm.org/doi/pdf/10.1145/2030112.2030127.
[21] https://link.springer.com/chapter/10.1007/978-3-319-55753-3_15.
[22] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8257968.
[23] https://link.springer.com/article/10.1007/s00371-015-1192-x.
[24] https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6450098.
[25] https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=4497422.
[26] https://dl.acm.org/doi/pdf/10.1145/2623330.2623735.
[27] https://dl.acm.org/doi/pdf/10.1145/3013527.
[28] https://dl.acm.org/doi/pdf/10.1145/3132847.3132933.
[29] https://ieeexplore.ieee.org/document/8016602.
[30] https://link.springer.com/article/10.1007/s10489-017-1104-z.