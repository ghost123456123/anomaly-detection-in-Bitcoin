# Anomaly Detection in Bitcoin  
Bitcoin is an emerging digital currency, the most distinctive characteristic is that all it's transactions are recorded in a publicaly avaiable blockchain.  
Therefore the transaction data in Bitcoin serve as an alternative to financial data which might not always be accessible and poses privacy concerns.  
This project shows how to detect abnormal transactions in the Bitcoin data.  


## Data  
The data used in this project can be downloaded from this link.  
http://compbio.cs.uic.edu/data/bitcoin/  

The data was obtained from parsing the blockchain using a Python script.  
The file we use contains Bitcoin transactions from January 2009 to April 2013, with over 15M transactions and 6.3M users.  

Each transaction has:  
- A transaction key  
- User ID sending Bitcoin  
- User ID receiving Bitcoin  
- Date and time when the transaction occur  
- Number of Bitcoins transferred  

| txn_key |	from_user |	to_user	|            date |     amount |
| ------: | --------: | ------: | --------------: | ---------: |
| 1	      | 2	        | 2	      | 20130410142250	| 24.375000  |
|	1	      | 2	        | 782477	| 20130410142250	| 0.770900   |
|	2	      | 620423	  | 4571210 | 20111227114312	| 614.174951 |
|	2	      | 620423	  | 3	      | 20111227114312	| 128.040520 |
|	3	      | 3         | 782479	| 20130410142250	| 47.140520  |

For demonstration purpose, we will use data from January 2009 to December 2010, with 218K transactions and 110K users.  


## Graph Theory  
Transactions in a financial system forms a graph where each user is a node and each transaction is an edge.   
As a result, concepts in graph theory will be very useful when analysing transactions and finding anomalies.  
- Degree - Number of nodes connecting this node  
- Clustering coefficient - Measures how neighboring nodes are connected to each other  
- Strongly connected components - Isolated communities  


## Isolation Forest  
A robust algorithm for anomaly detection.  
- Does not require assumptions on the data, unlike Elliptic Envelope which requires data to be Guassian  
- Able to capture more complex structure than One-Class SVM  

How it works:  
1. Build a tree by randomly split data into different partitions until every data point is isolated.  
2. The idea is data point located at leafs with a shorter path from the root are likely to be outliers.  
3. Build many of those trees to obtain an average path length for each data point.  
4. Identify the proportion with the shortest average path length as outliers.  

![alt text](/images/l14-anomaly-detection-19-638.jpg "Isolation Forest - Tree")  
(source: https://www.slideshare.net/mlvlc/l14-anomaly-detection)  
![alt text](/images/anomaly-detection-using-isolation-forests-10-638.jpg "Isolation Forest - Partition")  
(source: https://www.slideshare.net/turi-inc/anomaly-detection-using-isolation-forests)  

An isolation forest of 100 trees was built from the transactions, and the most extreme 1% transactions were identified as anomalies.  


## Results  
After running the isolation forest model, 2782 transactions were identified as abnormal.  
The model determined transactions with an anomaly score (log average path length) -0.0948 or lower were anomalies.  
The plot shows the distribution of anomaly scores of every transaction, all transactions with score lower than the threshold (left of the dotted red line) are classified as anomalies.  

![alt text](/images/anomaly-scores.png "Anomaly Scores Distribution and Cut-off")  

Let's visualize the transactions to verify that the anomalies found were different to normal transactions.  
t-SNE is a dimensionality reduction technique to visualize high dimension data.  
It tries to approximate the high dimension distribution with a low dimension distribution.  
It is a machine learning algorithm itself!  

The visualization shows 5000 of the transactions, with blue dots indicate normal transactions and red dots indicate abnormal transactions.  
As shown in the plot, the anomalies identified lies in isolated regions and are away from structures formed by normal transactions.  

![alt text](/images/t-sne-transactions.png "t-SNE Visualization of Transactions")  

