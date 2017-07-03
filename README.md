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
![alt text](https://image.slidesharecdn.com/anomalydetection-150918115852-lva1-app6892/95/l14-anomaly-detection-19-638.jpg?cb=1442577598 "Isolation Forest - tree")
