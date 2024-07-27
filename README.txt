README

SECTION 1: Before I start: Checklist of What I will need

1. Google account: mainly to access bigquery platform which is the datawarehouse i chose and where the sql queries to answer the 5 bussines questions will be excuted. I created a shared google account for this case purpose. below credentials:


2. Google service account (json file) which i provided (attached in email because "security" purposes), file name: 'sumup_case_sa'. This file will be use when running the python script to load the 3 tables in my bigquery project
  
3 Google colab notebook (in this repo). File name: 'sumup_case.ipynb'
  
4. The 3 excel files provided (in the base folder in this repo):
  -- transaction
  -- store
  -- device
  
5. credenciales.txt file (in this repo): contains the credentials to the Banco central api access. It will be required to be uploaded to the Google colab enviroment in Section 2.

SECTION 2: Google colab

1. Open bigquery and log in with the credentials of the google account I provided, and check that the dataset 'SumUp_Case' has no tables:
https://console.cloud.google.com/bigquery?hl=en&project=spatial-acumen-415519&ws=!1m0

2. go to https://colab.research.google.com/

3. upload the 'sumup_case.ipynb' file in this repo (ie. file>upload notebook)

4. drag and drop the 3 excel files and the 'credenciales.txt' file to the files section of the colab platform (in the left menu after clicking the folder icon)

5. run the script and upload the service account i provided via email (json file): 'sumup_case_sa'. The script has comments and to explain how it has been approached
6. Lastly, after running the script, check again on bigquery dataset, the tables should be there now.

SECTION 3: Bigquery scripts

Questions:
1. Top 10 stores per transacted amount.
2. Top 10 products sold.
3. Average transacted amount per store typology and country.
4. Percentage of transactions per device type.
5. Average time for a store to perform its 5 first transactions.

(*) All the 5 questions will answered through 5 queries in the folder 'queries' in this repo

Question explanation:

Q1. 
Assumptions:

- The definition of transaction that i'm going to use is: all transactions with a status = 'accepted'.
The rational before this filter is because the questions aim to understand the behavior of customer that use Sumup' devices eefficiently.
I'm going to use this definition for all 5 questions

Logic:

I listed 10 stores through the store_id field (id of the 'store' table) sorting according to the 'amount' field of the 'transaction' table

Q2:

Logic:

I calculated the 10 products with the highest number of associated accepted transactions.
If there is a tie in the top 10 products, the tiebreaker is the sales amount of the product.

Q3:

Logic:

I calculated the average transaction amount grouped by type and country.

Q4:

Logic:

I created an auxiliary table 'total_transactions' where I calculate all 'accepted' transactions.
Finally, I calculate the ratio between the number of transactions opened by device type and the number of transactions calculated in the auxiliary table

Q5. Average time for a store to perform its 5 first transactions
Assumptions:

- the only information we have about when the transactions are made is the date in the field "created_at" from the "transaction" dataset.

- the time (in days) it takes a store to reching its 5th transaction, is similar to the time between the 1st transaction and its 6th transaction.

Logic:
The logic i used to answer this question was the following:

Step 1: I'm building an auxiliar table store_transactions to add an index to the transactions grouped by each store.

Step 2: Build a 2nd auxiliar table "transaction_intervals" where i calculate the dates of the 1st and the 6th transaction.

Step 3: Lastly i calculate the average time between both these dates (i case the 6th transaction is not null) for all stores.

