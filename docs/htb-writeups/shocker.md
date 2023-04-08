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
The nmap scan showed port 80(http) and port 2222 which seed to be running OpenSSh 7.2p2<br>

<img src="https://user-images.githubusercontent.com/96850362/230719286-5b32f78e-fdf4-4729-8154-1b15cec716f1.png" alt="hello world" style="border: 2px solid black;">  
<br>



```python
def greeting(name):
    print("Hello, " + name + "!")
    
greeting("Alice")
```
rererererrere
