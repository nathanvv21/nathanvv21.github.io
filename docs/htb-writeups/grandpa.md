<span class="centered" style="font-size: 25pt; color: blue;">Grandpa</span> 
<head>
  <link rel="stylesheet" type="text/css" href="/docs/button.css">
 </head>
 
<body>
  <!--   <button style="--clr:#EA00FF"><span>Tags</span><i></i></button> -->
  <!-- <button style="--clr:#FFF01F"><span>Tags</span><i></i></button> -->
  <!-- <button style="--clr:#7FFF00"><span>Tags</span><i></i></button> -->
  <!-- <button style="--clr:#FF5E00"><span>Tags</span><i></i></button> -->
  <button onclick="document.getElementById('tags').style.display='inline'" style="--clr:#8A2BE2"><span>Tags</span><i></i></button>

</body>
<div id="tags" style="display:none">
<span class="tag-back">ASP</span> <span class="tag-back">WebDav</span> <span class="tag-back">Outdated Software</span> <span class="tag-back">Misconfiguration</span> <span class="tag-back">Arbitrary File Upload</span> <span class="tag-back">IIS</span> <span class="tag-back">Reconnaissance</span> <span class="tag-back">Web</span>
</div>                            
<br>
Overview.  
<br>




```fish
┌──(steak㉿dreadnought)-[~]
└─$ nmap -sC -sV 10.129.247.0 -p-                                                            
Starting Nmap 7.92 ( https://nmap.org ) at 2023-04-09 05:43 CDT
Nmap scan report for 10.129.247.0
Host is up (0.030s latency).
Not shown: 65534 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
|_http-title: Under Construction
| http-methods: 
|_  Potentially risky methods: TRACE COPY PROPFIND SEARCH LOCK UNLOCK DELETE PUT MOVE MKCOL PROPPATCH
| http-webdav-scan: 
|   Server Date: Sun, 09 Apr 2023 10:45:17 GMT
|   Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
|   WebDAV type: Unknown
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, COPY, PROPFIND, SEARCH, LOCK, UNLOCK
|_  Server Type: Microsoft-IIS/6.0
|_http-server-header: Microsoft-IIS/6.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 112.63 seconds
```
