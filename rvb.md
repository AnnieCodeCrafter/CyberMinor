## Red Team versus Blue Team event 
[i]Monday November 26th[/i]

### Table of Contents
* [Preparation](#preparation)
* [Web Applications](#web-applications)
* [Blogging Web Application](#blogging-web-application)
* [Dating Web Application](#dating-web-application)
* [Webshop](#webshop)
* [Reading website](#reading-website)
* [Conclusion](#conclusion)

On the 26th of November, we had our first Red Team vs Blue Team event. In this event, the blue team is tasked to create a web app and secure it as best as possible, as well as monitor it; and the red teamers pentest the web apps to see if the blue team missed anything. I, myself, am part of the red team. 

### Preparation
Of course, you can’t just start doing a pentest. First of all, I set up a Kali virtual machine in the VMware environment that school provided. I could clone from a template, so that wasn’t hard to do. I connected it to the internet and made sure it was up-to-date.
The entirety of the red team is pretty big, so we had divided ourself into smaller groups. I was with four others, who were also in my proftaak group. Someone else had listed the blue team’s applications and assigned them to each group. Our group had four web applications in total. 

### Web Applications
We were given an excel sheet with all the information we could have. We had the name of the application, the group who made it, the ip-address and which VLAN it was on. There were two different VLAN’s and neither were connected to the internet, only to eachother. I had to switch my vm’s VLAN to be able to connect to either. 
The four web applications that we were assigned were as follows: 
•	Blogging Web Application, no IP address provided, on VLAN 1
•	Dating Application, 10.10.2.147, on VLAN2
•	Webshop, 10.10.1.116, on VLAN 1
•	Reading Website, 10.10.2.131 and 132, on VLAN2

### Blogging Web Application
I was originally appointed to this one. It was the only one who didn’t have a given IP address – either because it wasn’t ready or because the owner forgot, I still don’t know. I tried to find its IP address through Nmap, and the only IP address that was up and wasn’t listed in the excel was 10.10.1.118, so I came to the conclusion that it was the missing IP address. However, Nmap showed that it was completely closed off. There were no ports open. I pinged it and got a response, but going there via the web browser didn’t do anything. So, we left it as it was. 
<img src="img/rvimg/Picture1.png">
 
### Dating Web Application
This one I did look into seriously. Me and two others worked on this one. I started out doing an Nmap scan: 
 
This showed that ports 80 and 443 were open, which are normal web ports. Going to the website, I made a simple profile to see what was on the website. 
One thing I noticed is that you could register again when you’re already logged in. This doesn’t really do anything bad, however, it simply logs you out and then logs you in with the newly registered user.  
You can see a list of people available when you enter the website:
You can click on their profiles, see their descriptions, and chat with them. When I edited my own description, I tried to enter cross-site scripting or sql injection, and neither worked. When uploading a profile picture, it sends the image to a different vendor that ‘flattens’ the image and forces the extension into normal .jpg or .png, so you can’t sneak in any payloads that way. 
One smaller thing I found was that the server doesn’t check if values are valid. I made a new person with birthday in the future, and the site allowed it:  
One other thing I noticed is that the user ID is in the link, so you can navigate to a user’s profile that doesn’t exist. 

### Webshop
With this one, I wasn’t involved at first but I have taken a look at it to confirm the other group’s findings.  
Ports 22, 80, and 443 were open, which are all normal web ports. An SSH port was also open.  We had already been given the username/password for the site, but if you wanted to create another user, it’s automatically an admin, someone who can add and remove products. 
Further, you can enter negative entries, so this “fruit” is worth minus one cent. 
 
 
### Reading website
I wasn’t involved in this one either, but I also took a look at it to confirm the other’s findings. 
 
The reading website has two ip’s; one for the frontend, and one for the backend. Note that the MySql port is open – it’s visible that there is a database there, but you can’t actually access it.

For the rest, though, it has too few features to actually make something of it. You can’t click on a fiction, you can’t make a fiction. You can only make your account and that’s mostly it. 

### Conclusion
It was a pretty fun and educational event. There weren’t a lot of vulnerabilities out there, because there were so few features, and the features that are there are built with pre-existing frameworks and services, which are in turn secure. I would advise that they’d at least clean up what input gets accepted, and where, and add some more features. 

