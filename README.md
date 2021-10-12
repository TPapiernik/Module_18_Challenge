# Module 18 Challenge - Clustering Cryptocurrencies - Unsupervised Machine Learning

## Overview

Project Origination Date: 2021-10-06

#### Disclaimer

No part of this analysis should be considered Financial Advise. It is to be used for Mathematical
and Computer Programming Demonstration Purposes Only. Conduct Your Own Due Diligence.


### Purpose

The purpose of this analysis is to aid in creating a report that includes what cryptocurrencies
are on the trading market and how they could be grouped to create a classification system
for this new investment.


### Tasks



### Approach

Use an Unsupervised Machine Learning approach with a clustering algorithm to group the cryptocurrencies.


### Deliverables

1. Preprocess the Data for PCA
2. Reduce Data Dimensions Using PCA
3. Cluster Cryptocurrencies Using K-means
4. Visualize Cryptocurrencies Results


### Resources

- Software:
	- Jupyter notebook server 6.3.0, running Python 3.7.10 64-bit
		- Dependencies:
			- hvplot.pandas
			- pandas
			- path [Path]
			- plotly.express
			- sklearn.cluster [KMeans]
			- sklearn.decomposition [PCA]
			- sklearn.preprocessing [MinMaxScaler, StandardScaler]

- Data:
	- `crypto_data.csv`
		- Client-Provided dataset of listed cryptocurrencies
		- Derived from CryptoCompare Sample API Coin Listing
		- URL: (https://min-api.cryptocompare.com/data/all/coinlist)
	- `crypto_data_symbol_added.csv`
		- User-Modified version of `crypto_data.csv` where the first field of Header was changed to `Symbol` from being initially blank with a leading comma in CSV File
	- `crypto_data_symbol_added_edited.csv`
		- User-Modified version of `crypto_data_symbol_added.csv`, where leading and trailing whitespace, and European-Style "." and " " thousands separators contained within 3 entries of Field 7 `TotalCoinSupply` have been removed.
	- `crypto_clustering_starter_code.ipynb`
		- Client-provided Starter Code Jupyter Notebook File
	- `crypto_clustering.ipynb`
		- User-Modified version of `crypto_clustering_starter_code.ipynb` used to complete Deliverable 1


Additional information about `crypto_data_symbol_added_edited.csv` is outlined below in Tables 1 & 2.

**Table 1: Source Data Description**
| File Name                               | Brief Description of Contents
|-----------------------------------------|------------------------------
| `crypto_data_symbol_added_edited.csv`   | Non-Quoted Comma Delimited ASCII Text File. Containing Data and Metadata for Listed Cryptocurrencies. Originally sourced from Crypto Compare JSON Data Hosted on the Web on an Unknown Date. 60 kb; 1,253 Lines. 1 Header Line; 1,252 unique Data/Metadata Lines. 1 Line Corresponds to 1 Cryptocurrency Record. 7 Fields. Uncertain if listings were sourced via sampling from a previous complete listing on Crypto Compare, or a complete listing from an earlier date. As of Oct. 09, 2021, Crypto Compare currently has 7,165 Cryptocurrencies listed and 23 of the 1,252 Symbols listed in `crypto_data_symbol_added.csv` are no longer listed. Edited from original client-provided `crypto_data.csv` by adding a missing Header Descriptor, and removing European-Style "." thousands separators from two entries.

**Table 2: `crypto_data_symbol_added_edited.csv` Fields**
| Field Name                              | Brief Description of Contents
|-----------------------------------------|------------------------------
| `Symbol`                                | 1,252 unique entries. Alphanumericsymbolic identifier, 1-6 characters. Fields containing Latin Alphabet Letters, Arabic Numerals, and Symbols (no spaces). All letters present are exclusively Upper Case. 8 entries containing exclusively digits. 4 entries beginning with 1 or more digits, containing letters also. 25 entries beginning with 1 or more letters, containing digits also. 1,214 entries containing letters only. 1 entry beginning with one or more letters, containing a '*' symbol.
| `CoinName`                              | 1,245 unique entries. 7 entries appear twice (2x) each [using either different Ids or Algorithms]: RoyalCoin, PayCoin, MedicCoin, DubaiCoin, Community Coin, Canada eCoin, AcesCoin. Mixed-case alphanumeric coin name including spaces. 3-32 characters. 1 entry contained leading whitespace prior to being removed. 8 entries contained trailing whitespace prior to being removed. 1 entry contains only digits. 1,208 entries contain letters and spaces only. 984 entries contain letters only with no spaces. 1,003 entries contain letters and digits with no spaces. 34 entries contain only upper case characters. [Conditions overlap, line counts exceed 100%]
| `Algorithm`                             | 95 unique entries. Free text name, 11-30 characters. One Algorithm is used 424 times ('Scrypt'), and 50 Algorithms are only used once. All other Algorithms are used between 2 and 197 times each. No leading or trailing whitespace.
| `IsTrading`                             | 2 unique entries ('True', 'False'). 1,144 'True', 108 'False'
| `ProofType`                             | 37 unique entries. Free text, 3-37 characters. One Type is used in isolation 535 times ('PoW'), and 28 Types are only used once. All other Types are used between 2 and 468 times each. These numbers would change slightly if uniform case was enforced ('PoS', vs 'Pos'), and if combined Types were Split ('PoW/PoS'). 1 entry contained leading whitespace prior to being removed. 2 entries contained trailing whitespace prior to being removed.
| `TotalCoinsMined`                       | 565 unique entries. Numeric field containing integers and decimals (no thousands separators or currency symbols). One entry is negative, with a preceding negative sign. 0-17 characters. 395 integer-only entries, ranging from 1-12 digits. 349 decimal entries, with 2-12 digits to the left of the decimal point, and 1-8 digits to the right of the decimal point. 166 entries '0', 508 entries -EMPTY STRING-. Min: -5917977547.96773, Max:  989988713439.649
| `TotalCoinSupply`                       | 546 unique entries. Numeric field containing integers and decimals (no currency symbols). 1-20 characters. 2 Integer Entries originally used "." as a thousands separator, and 1 Integer Entry originally used " " as a thousands separator (these separators have been removed). 1,237 integer-only entries, ranging from 1-19 digits. 15 decimal entries, with 4-12 digits to the left of the decimal point, and 2-8 digits to the right of the decimal point. 92 entries '0'. 5 entries contained leading whitespace prior to being removed. 10 entries contained trailing whitespace prior to being removed. 1 entry contained both leading and trailing whitespace prior to being removed. Min: 0, Max: 1000000000000000000

	  
#### Data Quality                           

Overall, data quality is high, at least at the surface level of metadata, without considering veracity.
The original file had only 6 Header Identifiers for 7 Fields, 508 rows contain NULL values (all in the `TotalCoinsMined` field),
and 3 records in the `TotalCoinSupply` field contained European-Style "." or " " thousands separators.

A handful of records contained leading and trailing whitespace, which were removed.

It is uncertain at this time if a negative value for `TotalCoinsMined` is allowable or not.

Beyond these considerations, all the fields contain their expected types of values, within reasonable ranges (if one Quintillion is an acceptable limit for `TotalCoinSupply`).

## Deliverables

### Deliverable 1

See `crypto_clustering.ipynb`

Note: My removal of trailing whitespace from entries in the `ProofType` field of `crypto_data.csv` resulted in 97 columns for the X DataFrame, rather than the 98 as shown in the provided Starter Code.
In the provided Starter Code, 'ProofType_PoW/PoS' appears twice as a column, because 'PoW/PoS' appears in the original unmodified source data as both 'PoW/PoS' and 'PoW/PoS '.

### Deliverable 2



### Deliverable 3



### Deliverable 4



## Results

### Overview of the Analysis



### Deliverable 1 Overview



### Deliverable 2 Overview



### Deliverable 3 Overview



### Deliverable 4 Overview



### Discussion of Results



## Summary




-- END --
