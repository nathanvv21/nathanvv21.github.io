<span class="centered" style="font-size: 25pt; color: red;">Shocker</span> 
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
<span class="tag-back">Apache</span> <span class="tag-back">Web</span> <span class="tag-back">Outdated Software</span> <span class="tag-back">Metasploit</span> <span class="tag-back">Bash</span> <span class="tag-back">Web Site Structure Discovery</span> <span class="tag-back">SUDO Exploitation</span> <span class="tag-back">Remote Code Execution</span>
</div>                            
<br>
Shocker is an easy and fairly straightforward box, with a few minor challenges to overcome. The biggest hurdle when hunting for user is when directory searching as you must append a trailing slash to the wordlist in order to get results. The next step is to identify the ShellShock exploit either manually or with nmap. Root will use a simple GTFObins for Perl.
<br>
This Box could also have been exploited with metasploit.  

<span style="font-size: 17pt; color: #4EEEE6;">Nmap</span>  
<br>
The nmap scan showed port 80 (http) and port 2222 which seemed to be running OpenSSh 7.2p2:<br>

<img src="https://user-images.githubusercontent.com/96850362/230719286-5b32f78e-fdf4-4729-8154-1b15cec716f1.png" alt="hello world" style="border: 2px solid black;">  
<br>
<span style="font-size: 17pt; color: #4EEEE6;">Port 80 Recon</span>  
<br>
The web page only shows image with the message "Don't Bug Me!":  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230719543-d0efe389-041d-4098-b5a6-68c67ae7b201.png" alt="hello world" style="border: 2px solid black;">  
<br>
The source of the page is short and has nothing useful:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230719634-e1d1a9df-6519-4be2-8d28-d0e71888b781.png" alt="hello world" style="border: 2px solid black;">  
<br>
<span style="font-size: 17pt; color: #4EEEE6;">Gobuster</span>  
<br>
The initial gobuster scan only showed <span class="important">index.html</span>.  After a bit of trial and error, I discovered a misconfiguration that requires a trailing slash appended to the items in the wordlist. An example being "icons" needing to be "icons/". Most tools have an option to add the trailing slash.  Once done, we begin getting a couple results:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230719953-f120eef5-c649-492d-abef-9f83a9e685d2.png" alt="hello world" style="border: 2px solid black;">  
<br>
This scan shows the directory <span class="important">/cgi-bin/</span>. Time to run the scan against that directory with common CGI script extensions:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230720059-18a0e7d2-a2af-4a5a-be56-c78a1f3de72f.png" alt="hello world" style="border: 2px solid black;">  
<br>
This only finds <span class="important">user.sh</span>.  
<br>
<span style="font-size: 17pt; color: #4EEEE6;">user.sh</span>  
<br>
Navigating to <span class="important">/cgi-bin/user.sh</span> results in Firefox asking to open or download the file:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230720204-60119e5c-5aef-4a5d-b1eb-fc0289ce760b.png" alt="hello world" style="border: 2px solid black;">  
<br>
In Burp Suite we can see the source of the file in the response:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230720254-7cb7cb69-7fa8-4c3b-bb9a-f37c0a1f67fc.png" alt="hello world" style="border: 2px solid black;">  
<br>
The reason why Firefox handles this file poorly is due to the blank space between Content-Length and Content-Type. Because of this space, the browser perceives Content-Type to be in the body instead of the header, where it should be.  
<br>

<span style="font-size: 17pt; color: #4EEEE6;">User Shell</span>  
<br>
Shellshock or CVE-2014-6271 is effectively a Remote Command Execution vulnerability in BASH. The vulnerability is due to the fact BASH wrongly executes trailing commands when a function is imported and stored in an evironment variable. This is the following POC from a post on [Exploit-DB.](https://www.exploit-db.com/docs/english/48112-the-shellshock-attack-%5Bpaper%5D.pdf?utm_source=dlvr.it&utm_medium=twitter)  
<br>
```bash
env x='() { :;}; echo vulnerable' bash -c "echo test"
```
<br>








```python
def greeting(name):
    print("Hello, " + name + "!")
    
greeting("Alice")
```
rererererrere
