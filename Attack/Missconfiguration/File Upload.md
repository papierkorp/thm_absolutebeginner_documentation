- overwrite existing files on the server
- upload + excecute reverse Shell
- evade Client-Side Filtering
- evade Server-Side Filtering
- evade Content-Type Checks

[File Filtering](Web%20Fundamentals#File%20Filtering)

# Basic method

1. Enumerate the Website to find about the languages/Frameworks/Backends/Uploads used
  - [Wappalyzer](Web#Wappalyzer) - Browse around
  - [Burp Suite](Burp%20Suite) - make a request + Capture
2. If a Upload Page is found. Is is it a 
  - [Client Side Filtering](#Client%20Side%20Filtering)
    - Capture the request per Burp Suite and change the [MIME Type](Web%20Fundamentals#File%20Filtering)
  - [Server Side Filtering](#server%20side%20filtering)
    - Blacklist: if a invalid file extension is possible `testimage.invalidfileextension` else its a Whitelist
    - Magic Number: Try uploading an innocentfile with a changed Magic Number to an filtered type
    - Test for the file length
4. Try a innocent file upload.
   - can we access it
   - is it embedded somwhere?
   - is there a naming scheme?
   - use [GoBuster](web#gobuster) for possible locations (maybe with -x wtich)
5. Try uploading a [Web Shell](RCE#web%20shell) or [Reverse Shell](RCE#reverse%20shell)

# Client Side Filtering

Possibilites:

- deactivate javascript in the Browser
- capture the request per [Burp Suite](Burp%20Suite) and delete javascript Filter
- capture the request per [Burp Suite](Burp%20Suite)
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

=> [RCE](RCE)

## Capture File with Burp

- check Sourceode for javascript filter / [MIME Type](Web%20Fundamentals#File%20Filtering)
- start up Burp and reload Site
- change file name shell in shell.jpg and upload
- catch filename and change Content-Type auf shell.php / text/x-php + forward

=> [RCE](RCE)


## Overwrite existing Files

1. enumerate for specific Image Files
  - e.g. Sourcecode: `<img src='images/dog.jpg' alt=''>` 
  - e.g. use [GoBuster](web#gobuster)
2. Upload new File with the same Name

=> [RCE](RCE)

# Server Side Filtering

## Unknown Filter

Enumerate what is allowed and what isnt by uploading different file extensions (e.g. `.jpg`, `.php`).

If something is allowed try: `.jpg.php` to bypass a badly programmed filter.

=> [RCE](RCE)

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

=> [RCE](RCE)


## Magic Number

[File Filtering](Web%20Fundamentals#File%20Filtering)

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
=> [RCE](RCE)

# Walkthrough for the Challenge

[Copied from here](https://muirlandoracle.co.uk/2020/06/30/file-upload-vulnerabilities-hints/)

Start by using gobuster on Jewel using the `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` wordlist (`gobuster dir -u http://jewel.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`). You’ll see that there is a page called `/admin`, and a directory called `/content`. Files you upload will end up in `/content` with a random three letter filename. Go to the homepage and use Burpsuite to remove the Client-Side Filter as demonstrated in task seven. The webserver is using Node.js (as the `X-Powered-By` header will show you). Download a Node.js reverse shell from [here](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#nodejs), and fill it in with your own IP and chosen port. Call the shell “file.jpg” to get around the MIME filter on the server (or edit the MIME type with burpsuite after uploading). Next use gobuster with the wordlist in the room to fuzz for your upload: `gobuster dir -u http://jewel.uploadvulns.thm/content -w <path-to-wordlist> -x jpg` — notice the `-x jpg` switch adding the .jpg file extension to each request. Have a look at each of those files in your web browser — one of them will be your shell. Remember the name of this file, and start a netcat listener on your own machine using your chosen port number; then go to the admin page and type in `../content/<name-of-file>` — so, for example, it might be `../content/ABC.jpg`. You should receive a reverse shell