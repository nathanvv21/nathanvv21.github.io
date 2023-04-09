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
I am capable of performing Shellshock testing through both manual techniques and by utilizing an nmap script. I will demonstrate both methods.  
<br>
<span style="font-size: 14pt; color: #F70DF1 ;">Nmap</span>  
<br>
nmap can be used to test for Shellshock with a script:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230754672-6f007139-537f-4d53-8abd-1ecaa4a2ec4f.png" alt="hello world" style="border: 2px solid black;">  
<br>
<span style="font-size: 14pt; color: #F70DF1 ;">Manually</span>  
<br>
First I will send the request <span class="important">user.sh</span> to Burp Repeater. It is worth noting that in order to view the execution of commands in the response, the "echo" command must be utilized. Additionally, it is necessary to use the full path of the command when executing commands. After some short testing, I found a working payload:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230754828-4e3eafc5-9176-4b93-9cf3-abb5c771a5ab.png" alt="hello world" style="border: 2px solid black;">  
<br>
Now that the vulnerability is confirmed, next task is to craft a payload for the reverse shell. One simple tool for reverse shells is [revshells.com](https://www.revshells.com/). I start a <span class="important">netcat</span> listener and then add a simple bash reverse shell to the User-Agent field of the http request:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230754954-56ad68ce-bef0-48c9-8038-423edc7b0e4d.png" alt="hello world" style="border: 2px solid black;">  
<br>
This web request hang and I check the listener for the reverse shell:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230755001-112fe7e8-f911-4c34-9d59-2867a7671040.png" alt="hello world" style="border: 2px solid black;">  
<br>
I then upgraded my shell the usual way and check <span class="important">user.txt</span>:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230755048-832c5f11-1660-4928-9c03-376dfa61f38d.png" alt="hello world" style="border: 2px solid black;">  
<br>
<br>
<span style="font-size: 17pt; color: red;">Root</span>  
<br>
The first thing I usually do is check <span class="important">sudo -l</span> which showed the ability to run perl as root without a password:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230755118-38da6363-1b18-4522-ad96-cfef717ff901.png" alt="hello world" style="border: 2px solid black;">  
<br>
Checking [GTFOBins](https://gtfobins.github.io/gtfobins/perl/) is an easy way to abuse this misconfiguration. Checking for sudo shows a simple one liner to escalate:
<img src="https://user-images.githubusercontent.com/96850362/230755218-ec7155a7-5672-46bd-955a-110da7e9c8a7.png" alt="hello world" style="border: 2px solid black;">  
<br>
And easy enough, root has been achieved.

