- overwrite existing files on the server
- upload + excecute reverse Shell
- evade Client-Side Filtering
- evade Server-Side Filtering
- evade Content-Type Checks

[[Web Fundamentals#File Filtering]]


# Client Side Filtering

- deactive javascript in the Browser
- capture the request per [[Burp Suite]] and delete javascript Filter
- capture the request per [[Burp Suite]], send it directly to Target-Upload-Point and ignore webpage (e.g. `curl -X POST -F „submit:<value>“ -F „<file-parameter>:@<path-to-file>“ <site>`)

Javascript Filter Example:
```
if` `(file.type !=` `"image/jpeg"``) {upload.value =` `""``; alert(``"This page only accepts JPEG files"``);}
```


**Capture Page with Burp**
- check Sourceode for javascript filter
- start up Burp and reload Site
- if neccessary: "Proxy - Options - Intercept Client Requests - ^js$"
- Do Intercept - Response to this request. - Forward - delete javascript Part - Forward

**Caputre File with Burp**

- check Sourceode for javascript filter
- start up Burp and reload Site
- change file name shell in shell.jpg and upload
- catch filename and change Content-Type auf shell.php / text/x-php + forward



# Overwrite existing Files



1. enumerate for specific Image Files
  - e.g. Sourcecode: `<img src='images/dog.jpg' alt=''>` 
  - e.g. use [[Web#GoBuster]]
2. Upload new File with the same Name


# RCE

[[RCE]]