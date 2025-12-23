# SQL injection

Q: What is SQL injection?

A: When user's input is not validated and directly execute in a SQL query -> attacker can modify the inject payload to retrieve data from database, such as user's PII data, password, ... or attacker can do anything like he can directly execute the SQL query with database.


Q: How can you detect a SQLi vulnearbility?

A: I can input some special character in SQL quert such as single quote, double quote, comment symbol, ... and observe the different of response package. Sometimes may trigger the SQL error, sometimes the response may different from the normal response, maybe change some words, delay time, ..., and we can assume that maybe we have a SQLi vulnerability here.

Q: 
