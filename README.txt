README

SECTION 1: 

SECTION 2: 

SECTION 3: Bigquery scripts

Questions:
1. Top 10 stores per transacted amount.
2. Top 10 products sold.
3. Average transacted amount per store typology and country.
4. Percentage of transactions per device type.
5. Average time for a store to perform its 5 first transactions.

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

