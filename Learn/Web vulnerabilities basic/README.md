# SQL injection

Q: What is SQL injection?

A: When user's input is not validated and directly execute in a SQL query -> attacker can modify the inject payload to retrieve data from database, such as user's PII data, password, ... or attacker can do anything like he can directly execute the SQL query with database.


Q: How can you detect a SQLi vulnearbility?

A: I can input some special character in SQL quert such as single quote, double quote, comment symbol, ... and observe the different of response package. Sometimes may trigger the SQL error, sometimes the response may different from the normal response, maybe change some words, delay time, ..., and we can assume that maybe we have a SQLi vulnerability here.

Q: List some type of SQLi?

A: Simple SQLi: you can see the result of SQL query in response, in this case you can directly exploit the SQLi, the data you retrive will reflect in the response. Blind SQLi: you can not see the output of SQL query in response, you can use error-based or time-based payload to exploit in this case, compare the different or time delay in response package. Second order SQLi: your payload is not execute immediate, will save in the database and execute in the second time when server use it.

Q: How to prevent SQLi?

A: You need to validate all the user input first. For example, with the numeric input type, you can use type casting. You can also use prepared statement to execute the SQL query.

Q: How can prepared statment protect the SQLi?

A: The prepared statment will send the query and the data from user input to server separately. It use a template-like SQL query, so, database server know that what is the SQL query and which is the data that used in the query. When you use prepared statement to execute the SQL query, your input data will never be 
