# ToolsRus

Practise using tools such as dirbuster, hydra, nmap, nikto and metasploit

This room has a few questions to answer, to get the answer's we'll need to user a variety of tools!

Our target

![95df8577cf0adada910f252c7af0436b.png](images/95df8577cf0adada910f252c7af0436b.png)

We need to enumerate folders, finding ones that start with G.

![df55ea0a29a0111dd932e01bcd27bf9b.png](images/df55ea0a29a0111dd932e01bcd27bf9b.png)

Our first answer is guidelines.

Now we need to find a name in this directory?

![5f053659dcdf530a54d3134fa0fe9f58.png](images/5f053659dcdf530a54d3134fa0fe9f58.png)

BOB!

Next question is which folder has basic authentication?

Gobuster is still running and has more folders for us

![126ccd6641e2341e37ef34093aff44b2.png](images/126ccd6641e2341e37ef34093aff44b2.png)

Browsing to this folder shows us the authentication

![10968ddca01b5cad43631b23c2320761.png](images/10968ddca01b5cad43631b23c2320761.png)

Now to find Bob's password, I'll try the rockyou password list.

![4739c431afa78f0743e76d5512edb8ad.png](images/4739c431afa78f0743e76d5512edb8ad.png)

That was quick!

![31d9cf56f6e6e41e0761d7257d0bb595.png](images/31d9cf56f6e6e41e0761d7257d0bb595.png)

Apparently this page has moved ports! i'll use nmap to try and find it!

![9388f169a102e3fcc316c411f3f89793.png](images/9388f169a102e3fcc316c411f3f89793.png)

We have some ports! 1234 is the next one we're going to look at.

A bit more info this time

![b7efcb6f776339d1f8b1fef75709f776.png](images/b7efcb6f776339d1f8b1fef75709f776.png)

Now we'll use Nikto to scan that port in folder manager/html with the creds we've got. We need to find out how many documentation files there are.

This scan took over an hour! But the answer was 5 :) 

![9f3739afeaf5b822a0bb18d9d4fd581f.png](images/9f3739afeaf5b822a0bb18d9d4fd581f.png)
I didn't screen shot the answer because I uploaded this to github already!

The next questions were just versions numbers in the nmap scan above, so I won't list them in the write-up.

Now we need to try and get a shell with metasploit...

First I tried the tomcat mgr deploy, but that failed.

![3349c6405ced7b9075423a9739ac2e67.png](images/3349c6405ced7b9075423a9739ac2e67.png)

Next I tried the upload module and we had a winner!

![cbe65d432ff3aa0f621e79e9ddc98c9f.png](images/cbe65d432ff3aa0f621e79e9ddc98c9f.png)

Here are the settings I made, and the shell!

![a3a45ca417633f1a5de398173258b745.png](images/a3a45ca417633f1a5de398173258b745.png)

Who are we? ROOT!

![a34f9eb39638adffedb9d7b5f81d12e3.png](images/a34f9eb39638adffedb9d7b5f81d12e3.png)

And our flag!

![09a7a9c2bd7a7c0f3b071821f8b381cf.png](images/09a7a9c2bd7a7c0f3b071821f8b381cf.png)