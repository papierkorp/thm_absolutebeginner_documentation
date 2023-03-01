# General

One-Stop-Shop for Web-Application Penetration Testing / Mobile Applications (APIs)

Mainwork = Capture and Modify Traffic between Attacker and Webserver. After captureing of Requests, they can be send to Feature-Parts of Burp Suite

# Interface

4 Quadrants

1.  Tasks
    1.  background tasks, which should run while using Burp Suite (for Example Scans)
2.  Event log
    1. what is Burp doing atm (for example starting the proxy)
    2. detailed Informations about Connections
3.  Issue Activity
    1.  Burp Pro only
    2. all Vulnerabilites auto-detected by Burp Scanner
4.  Adivsory
    1. more Informations about the found Vulnerabilites
    2. Advice for the coming procedure

Navigation

- takes place with the Tabs on the Top under the menu, choose between different modules
- ctrl + shift + d = switch to Dashboard

Settings

-   Global = User Options Tab
    -   Connections = how Burp builds a Connection to the Target (e.g. per Proxy)
    -   TSL = Client Certificates and different TLS options
    -   Display = Change the Appearence of Burp / theme
    -   Misc = Keybindings..
-   Projekt = Project Options Tab
    -   Connections = overwrites Global Settings
    -   HTTP = how to handle the HTTP Protocol (e.g. redirects)
    -   TLS = overwrites Global Settings
    -   Sessions = How Burp collects,saves and uses Session Cookies  (e.g. Macros for logging)
    -   Misc = a lot of options for Burp Pro, Logging, inbuild Browser

# Features

-   Proxy
    - capture + change requests / responses  
-   Repeater
    -  capture + change the same request + send again (e.g. SQLInjection)
-   Intruder
    -   Bruteforce
-   Decoder
    - decode captured Informations
-   Comparer
    - compare 2 Datapairs on word/byte Level
-   Sequencer
    - evaluate randomness of session cookies..
-   Extender
    -   Extensions

## Target

-   Sitemap
    - build a sitemap of the Target while browsing the Target
-   Scope
    - e.g. for proxy, so just a specific Target is captured
-   Issue Definitions
    - Burp Pro
    - big list of known Web Vulns

## Proxy

=> Main Function of Burp Suite, captures our Browserrequests/responses for further Manipulation

**Intercept**
-   Forward = Forward Request to Server
-   Drop = Drop Request
-   Intercept is on = keep capturing all requests
-   Action = Forward Request to another Burp Suite Feature (e.g. Intruder for Bruteforce)
-   Open Browser = use Burp Suite intern Browser instead of Proxy

**HTTP history**

**WebSockets history**

**Options**

-   Proxy
    -   Standard: 127.0.0.1:8080
-  Rules of what to Capture
    -  Intercept responses based on the following rules
        -  Add
            -  Or - Request - Was intercepted

### Scoping

So we capture only what we want

Target Tab - Rightclick Target - Add to Scope  
Proxy Options - Intercept Client Requests - And / URL / Is in target scope

### Configure a Proxy in the Browser

-  Firefox
    -  install FoxyProxy Addon
        -  top right - Options
            -  Add
                -  Title: Burp
                -  Proxy IP: 127.0.0.1
                -  Port: 8080
    - configure Certificates
        - While proxy active: open http://burp/cert and download "cacert.der"
        - in Browser: "about\:preferences"
        - search for "certificates"
        -  view Certificates…
            - iImport - „cacert.der“
-   Burp Browser
    -   Refusing to start browser as your current configuration does not suppoert running without sandbox
        -   weil wir als Linux root eingeloggt sind
        - because we are logged in as root
          1. Add new User and open Burp Suite with low Privilege
          2.  Projekt Options - Misc - Embedded Browser - Allow the embedded broser to run without a sandbox (unsecure!!!)


## Repeater

## Intruder

=> Bruteforce Passwords for Website Logins

First you have to forward the Login Request from Proxy to Intruder

-  Positions
    -  choose Attack Type = Sniper / Battering Ram / Pitchfork / Cluster Bomb
    -  Header
-  Payloads
    -  Payload Sets = depends on Attack Type
    -  Payload Options = passwordlist
-  Resource Pool
-  Options

Example:

After Forwarding change the Header:

```
POST /rest/user/login HTTP/1.1
Host: 10.10.110.50
Content-Length: 28
Accept: application/json, text/plain, */*
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.102 Safari/537.36
Content-Type: application/json
Origin: http://10.10.110.50
Referer: http://10.10.110.50/
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Cookie: cookieconsent_status=dismiss; language=en; continueCode=OnaBBarwE2q487KZPVgxzNb59LGl2iDSxjA6DmlyMX3okOQWRneYJpj1vWpK; io=PzBVz6frAjd-n0BaAAAv
Connection: close

{"email":"admin@juice-shop.op","password":"§§"}
```

-   email: which email to attack
-   password: §§ = is Burp Suites Interpretation of ""


Change to "Intruder - Payloads"

-   Payload set: 1
-   Payload type: Simple list
-   Payload Option - Load..: /usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt

 And Start Attack

## Decoder

## Comparer

## Sequencer