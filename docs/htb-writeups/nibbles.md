The Nibbles box is running a vulnerable version of [Nibbleblog](http://www.nibbleblog.com/) which can be exploited for user access. Privesc to root abuses sudo on a file that has lax permissions.  
<br>
  
<span style="font-size: 17pt; color: #4EEEE6;">Nmap</span>  
  
The nmap scan showed http (80) and ssh (22)  
<br>
![image](https://user-images.githubusercontent.com/96850362/230301039-3d359d04-42aa-4194-9a7d-1b8d48d676d8.png)  
  
<span style="font-size: 17pt; color: #4EEEE6;">Port 80 Recon</span>  

The web page only shows the simple message "Hello World!".  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230306767-c0910d88-e6a4-4d41-8df9-19aa5a31a4d7.png" alt="hello world" style="border: 2px solid black;">  
<br>
If we look at the source of the page however, a hidden message is revealed.  
<img src="https://user-images.githubusercontent.com/96850362/230306451-2df8ffe4-deec-4da7-ab44-e22e4701fa03.png" alt="source of root" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
  
<span style="font-size: 17pt; color: #4EEEE6;">/nibbleblog</span>  

The Page at <span style="background-color: black"> /nibbleblog/. </span> shows an empty blog page.  
<img src="https://user-images.githubusercontent.com/96850362/230315021-75ded80c-fb15-4e2a-99ec-e2af9a0cb0c9.png" alt="/nibbleblog page" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
  
<span style="font-size: 17pt; color: #4EEEE6;">gobuster</span>  
  
Use gobuster to search for interesting directories  
  
<img src="https://user-images.githubusercontent.com/96850362/230331724-f9675ca2-564f-42b9-a447-53f90bd6bc77.png" alt="/nibbleblog page" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
The path <span style="background-color: black"> /nibbleblog/content. </span> is a good place to start.  
Content lists the directories so we can navigate to find <span style="background-color: black"> /nibbleblog/content/private/user.xml </span> which shows users and IPs.  
<img src="https://user-images.githubusercontent.com/96850362/230332599-007a60b7-423f-4539-845b-771b3f56de17.png" alt="/nibbleblog page" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
We can also get the Nibbleblog version number from <span style="background-color: black"> /nibbleblog/README </span> :   
<img src="https://user-images.githubusercontent.com/96850362/230345076-63c1db91-9956-48f6-bd21-6d75993309ab.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
  
<span style="font-size: 17pt; color: #4EEEE6;">Admin Login</span>  
  
The gobuster search revealed <span style="background-color: black"> /admin.php </span> which leads to a login page.  
<img src="https://user-images.githubusercontent.com/96850362/230343601-32cb3133-159b-40d8-a7a0-16edc8b68dc4.png" alt="/nibbleblog page" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
With a little bit of guess work, we can log in with the password "nibbles"  
<img src="https://user-images.githubusercontent.com/96850362/230344323-e017da13-ddcd-425c-90bb-c69a439b34b5.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
  
<span style="font-size: 17pt; color: #4EEEE6;">Shell</span>  
  
To exploit this box we need to navigate to the plugins page to activate the "My image" plugin  
<img src="https://user-images.githubusercontent.com/96850362/230354032-a9fb3256-cb43-4e2c-aeb4-ee11e3feefba.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
<br>
Clicking on "configure" under the "My image" plugin allows us to upload a file.  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230354400-49a32320-338f-47e0-9a7f-166632eec553.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
<br>
We can then upload a php reverse shell and ignore the warnings.  
<br>
Set up a netcat listener like "nc -lvnp 4444".  
Then navigate to <span style="background-color: black"> http://X.X.X.X/nibbleblog/content/private/plugins/my_image/image.php </span> which will connect your shell back to the netcat listener.  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230355777-75923f39-5c0f-46db-a3c7-0e47df27dff7.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
Now we have user.txt flag.  

<span style="font-size: 17pt; color: #F70D0D;">Root</span>  
  
First thing to check is the user's sudo privilege:<br>
<br>
<img src="https://user-images.githubusercontent.com/96850362/230357753-3f2df4bd-5c2a-4c74-96a4-bd3832f604c1.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
<br>
We can run <span style="background-color: black"> monitor.sh </span> without a password. 
<br>
Checking the permissions of the file shows that it is writable:  
<img src="https://user-images.githubusercontent.com/96850362/230358656-9ab95934-6e3b-4668-802c-da1ccedacc2d.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
<br>
Just overwrite the script with a simple reverse shell gets the job done:  
<br>
<img src="https://user-images.githubusercontent.com/96850362/230361167-75fe6307-ed50-4fc2-92be-a7ece1fbaa47.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
<br>
Next Setup the netcat listener like "nc -lvnp 4242"  
Then run <span style="background-color: black"> monitor.sh </span> to get the root shell.  
<img src="https://user-images.githubusercontent.com/96850362/230361706-5aba6fcd-bfca-491e-bf6f-489a294e4dd7.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
  
  
<img src="https://user-images.githubusercontent.com/96850362/230362021-10685736-5d8c-4df5-adc0-23b020c4a86d.png" style="border: 2px solid black; max-width: 800px; max-height: 600px;">  
<br>
Now all that is left to do is read the root flag from <span style="background-color: black"> root.txt </span>
