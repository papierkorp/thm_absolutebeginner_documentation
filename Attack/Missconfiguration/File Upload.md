- overwrite existing files on the server
- upload + excecute reverse Shell
- evade Client-Side Filtering
- evade Server-Side Filtering
- evade Content-Type Checks

[[Web Fundamentals#File Filtering]]

# Basic method

1. Enumerate the Website to find about the languages/Frameworks/Backends/Uploads used
  - [[Web#Wappalyzer]] - Browse around
  - [[Burp Suite]] - make a request + Capture
2. If a Upload Page is found. Is is it a 
  - [[#Client Side Filtering]]
    - Capture the request per Burp Suite and change the [[Web Fundamentals#File Filtering|MIME Type]] 
  - [[#Server Side Filtering]]
    - Blacklist: if a invalid file extension is possible `testimage.invalidfileextension` else its a Whitelist
    - Magic Number: Try uploading an innocentfile with a changed Magic Number to an filtered type
    - Test for the file length
4. Try a innocent file upload.
   - can we access it
   - is it embedded somwhere?
   - is there a naming scheme?
   - use [[Web#GoBuster]] for possible locations (maybe with -x wtich)
5. Try uploading a [[RCE#Web Shell]] or [[RCE#Reverse Shell]]

# Client Side Filtering

Possibilites:

- deactivate javascript in the Browser
- capture the request per [[Burp Suite]] and delete javascript Filter
- capture the request per [[Burp Suite]]
- send File directly to Target-Upload-Point and ignore webpage (e.g. `curl -X POST -F „submit:<value>“ -F "<file-parameter>:@<path-to-file>" <site>`)

Javascript Filter Example:
```js
if (file.type != "image/jpeg") {upload.value =""; alert("This page only accepts JPEG files");}
```

First Thing first:

Find the path


## Capture Page with Burp

- check Sourceode for javascript filter
- start up Burp and reload Site
- if neccessary: "Proxy - Options - Intercept Client Requests - ^js$"
- Do Intercept - Response to this request. - Forward - delete javascript Part - Forward

=> [[RCE]]

## Capture File with Burp

- check Sourceode for javascript filter / [[Web Fundamentals#File Filtering|MIME Type]]
- start up Burp and reload Site
- change file name shell in shell.jpg and upload
- catch filename and change Content-Type auf shell.php / text/x-php + forward

=> [[RCE]]


## Overwrite existing Files

1. enumerate for specific Image Files
  - e.g. Sourcecode: `<img src='images/dog.jpg' alt=''>` 
  - e.g. use [[Web#GoBuster]]
2. Upload new File with the same Name

=> [[RCE]]

# Server Side Filtering

## Unknown Filter

Enumerate what is allowed and what isnt by uploading different file extensions (e.g. `.jpg`, `.php`).

If something is allowed try: `.jpg.php` to bypass a badly programmed filter.

=> [[RCE]]

## Black Filter

could be for example something like this in the backend (not for us to see!):

```php
<?php  
    //Get the extension  
    $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];  
    //Check the extension against the blacklist -- .php and .phtml  
    switch($extension){  
        case "php":  
        case "phtml":  
        case NULL:  
            $uploadFail = True;  
            break;  
        default:  
            $uploadFail = False;  
    }  
?>
```

Possible bypasses for such a filter would be to use other php extensions like:
`.php3`, `.php4`, `.php5`, `.php7`, `.phps`, `.php-s`, `.pht`, `.phar`

It could also happen (default for apache2) that the server doesnt recognise such file extensions as php files and only displays it as text files.

=> [[RCE]]


## Magic Number

[[Web Fundamentals#File Filtering]]

Try adding the Magic Number in Front of the webshell.php
After we know jpeg is allowed choose a random Magic Number for jpeg Files:  `FF D8 FF DB`

First lets take a look at our current webshell:

```
file webshell.php
> webshell.php: PHP script, ASCII text
```

Now edit our current webshell.php and add 4 Random Chars in front:

```
vi webshell.php
> AAAA
> <?php
```

Finally open the webshell.php with an hexeditor

```
sudo apt install ncurses-hexedit
hexeditor webshell.php
> 00000000  41 41 41 41  0A 3C 3F 70   68 70 0A 2F  2F 20 70 68   AAAA.<?php.// ph #41 = A, change the 41 to our jpeg Magic Number from above
> 00000000  FF D8 FF DB  0A 3C 3F 70   68 70 0A 2F  2F 20 70 68   .....<?php.// ph
```

The file should now be an jpeg:

```
file webshell.php
> webshell.php: JPEG image data
```
=> [[RCE]] 