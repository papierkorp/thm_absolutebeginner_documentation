- Horizontal Privilege Escalation = User can only work in the same Level as other Users
- Vertical Privilege Escalation = User can work on a higher Level as other Users

Examples:

**Developerconsole**

- Debugger (Firefox) / Source (Chrome)
- search for a "main.js" or something similar and change it with a click on {} to a readable format
- in the "main.js" search for "admin"

**IDOR**

= Insecure Direct Object Reference

is basically a Missonfiguration how to Handle User Input

```
The application uses unverified data in a SQL call that is accessing account information:
pstmt.setString(1, request.getParameter("acct"));
ResultSet results = pstmt.executeQuery( );

An attacker simply modifies the ‘acct’ parameter in the browser to send whatever account number they want. If not properly verified, the attacker can access any user’s account.
http://example.com/app/accountInfo?acct=notmyacct
```

```
An attacker simply force browses to target URLs. Admin rights are required for access to the admin page.
http://example.com/app/getappInfo
http://example.com/app/admin_getappInfo
```

e.g. instead of: `https://example.com/bank?account_number=1234`, lets try `https://example.com/bank?account_number=1235` and see what happens