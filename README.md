# SQL-Projects
----Journey of learning SQL---- 

-----21th Apr 2026----
Aggregate Functions: SQL provides several aggregate functions like AVG(), SUM(), MIN(), MAX(), and COUNT() to summarize data. Each function serves a unique purpose:

AVG() calculates the average value from a specified field.
SUM() adds up all the values in a specified field.
MIN() and MAX() find the lowest and highest values in a specified field, respectively.
COUNT() counts the number of non-missing (not null) records in a field.
Application: You saw how these functions can be applied to both numerical and non-numerical fields, with AVG() and SUM() being exclusive to numerical data due to their arithmetic nature. In contrast, COUNT(), MIN(), and MAX() can be used with non-numerical fields as well.

Practical Examples: Through examples, you learned how to use these functions in SQL queries to extract meaningful insights from the films table, such as calculating the average budget of films or finding the oldest film.

Best Practices: It was highlighted that using aliases in your queries when summarizing data makes the results clearer and more understandable to anyone reading your code.

---22th April 2026--- :

Create a new query.sql file within VS Code after installing MSSQL extension and installing SQL SERVER MANAGEMENT STUDIO and SQL SERVER 2022:

IMPORTANT NOTE:

We can’t change localhost to point to a folder—that’s a category mistake.
localhost in Microsoft SQL Server is the network address of the database engine, not a file path. It represents a running service (process), not where files live on disk. 
Furthermore, while connecting to the localhost with SQL SERVER MANAGEMENT STUDIO checking and making sure that the "Trust this certificate" is ticked!.

Here is what we can control:
“Store the database files (.mdf / .ldf) in a specific folder on my laptop”