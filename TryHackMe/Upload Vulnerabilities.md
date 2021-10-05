# Upload Vulnerabilities

Tutorial room exploring some basic file-upload vulnerabilities in websites.

## Overwriting Existing Files

Our goal here is to upload a file, overwriting an existing file.

![0768084a30e2a213ae2dacd978a0570a.png](images/0768084a30e2a213ae2dacd978a0570a.png)

Here we can see the background image location.

![fd105c812579e906bd56b97454749894.png](images/fd105c812579e906bd56b97454749894.png)

So I'll try my luck uploading a file called mountains.jpg and see how the server responds.

I've downloaded an image of a lion and renamed it to mountains.jpg.

Here is the upload

![2c17c3e9b71b95bdded0f82d6d0bf428.png](images/2c17c3e9b71b95bdded0f82d6d0bf428.png)

And that was a success!

![1d08c346e5a552dd72666fceaa4b562f.png](images/1d08c346e5a552dd72666fceaa4b562f.png)

## Remote Code Execution

Here we have a page where we can try to upload files, aiming for a reverse shell.

![5e11f1dafab18f7fd21b5a0f59664c95.png](images/5e11f1dafab18f7fd21b5a0f59664c95.png)

First I need to find out where the files are uploaded to. I can try upload the image in the previous challenge.

Trying the obvious location (upload/uploads) didn't work.

![935445d78848bf9df73b341834cee211.png](images/935445d78848bf9df73b341834cee211.png)

I'll try gobuster to look for possible upload locations.

![e1f2e8edc34427a6673048b7b0113a9d.png](images/e1f2e8edc34427a6673048b7b0113a9d.png)

We quickly found a hit, 'resources'.

Here we can see the file I uploaded.

![9e2f03ce818e7b59ef107f6e09385cd3.png](images/9e2f03ce818e7b59ef107f6e09385cd3.png)

Now for some reverse shell options... I'll assume PHP and try my luck.

Here is some PHP code to launch a potential reverse shell.

![455e1f79a71096b88217519a1f4ef5c3.png](images/455e1f79a71096b88217519a1f4ef5c3.png)

I'll open a netcat listener as well (note I had the wrong port in the screen shot below, I changed it to port 4444 afterwards).

![12053da55aa8fef2d989bfee1f747cea.png](images/12053da55aa8fef2d989bfee1f747cea.png)

We can see my uploaded file here:

![247041bb839ef6548fe79be6d415c2fb.png](images/247041bb839ef6548fe79be6d415c2fb.png)

After selecting the link I had a shell open on my netcat listener

![258039044f7e0603b0a2f6a02a7e2d74.png](images/258039044f7e0603b0a2f6a02a7e2d74.png)

Some quick hunting around and we have found our flag!

![14f5896af4f1aff7dd03d80274f35b90.png](images/14f5896af4f1aff7dd03d80274f35b90.png)

## Filtering

Now the remaining challenges in this room will include some filtering defences we have to navigate around. The following challenges / will include different types of filtering.

## Client Side Filtering

Client side filtering should be relatively easy to bypass, as the filtering code is run on the client's web browser. 

Here we have a site we wish to upload a reverse shell to.

![0639e3d7525c76b184e6c9acb0af0fb9.png](images/0639e3d7525c76b184e6c9acb0af0fb9.png)

Let's have a look at the source code.

A quick look and we can see the potential client side filtering script.

![fe0e7e040129c7c0c1d0ac9b79d8a7d4.png](images/fe0e7e040129c7c0c1d0ac9b79d8a7d4.png)

Following this js file we can see it only allows for png images.

![c43724bab49b0f0fbe1f87741a6b29d1.png](images/c43724bab49b0f0fbe1f87741a6b29d1.png)

I'll upload something to see where it goes.

After uploading it, nothing obvious to where it goes, I'll try some default locations like upload, assets, resources, images etc. Should this fail I'll use gobuster.

No need for gobuster this time, I found it:

![6b0b4a7624c16a9772b18fd7d73979c0.png](images/6b0b4a7624c16a9772b18fd7d73979c0.png)

Now I'll try and upload the PHP reverse shell we used previously.

![3a8a5882104a815df3de207949b13801.png](images/3a8a5882104a815df3de207949b13801.png)

No luck... so looks like I'll need to bypass the client side filtering.

So I've uploaded a valid file again and captured it in Burp so I can make modifications.

![4eea9f6eaab02270e1700b949bce7b3e.png](images/4eea9f6eaab02270e1700b949bce7b3e.png)

Now I'll send this over to repeater in burp and try manually modify it to send a php file.

Here is the modified POST request.

![449e047cc552bb7f32938e0147fa5b98.png](images/449e047cc552bb7f32938e0147fa5b98.png)

And our success response

![e7102991a17feb08c61f759a4f852b4c.png](images/e7102991a17feb08c61f759a4f852b4c.png)

Now to check out the site and try the reverse shell.

![dde6c5f13979d6a1828c2782c45468c2.png](images/dde6c5f13979d6a1828c2782c45468c2.png)

So far so good!

![13b3200bd43fa6d9ae202d09fab8a550.png](images/13b3200bd43fa6d9ae202d09fab8a550.png)

Win! I have a shell!

And for the flag:

![46dcad774a06a4f605fe75f8370c1777.png](images/46dcad774a06a4f605fe75f8370c1777.png)

## Server Side Filtering - File Extensions

This might be a little tougher, we need to trick the filtering code on the server now. This section filters by file extension.

So for this challenge we have an interesting looking file upload portal. A fake shell :)

![1c8426d463d1e81fe062cb3f250a295c.png](images/1c8426d463d1e81fe062cb3f250a295c.png)

Let's try a jpg file to begin with and see where it lands on the server.

![8a96f5c10dd02f5a2f895b0ff56d4bbd.png](images/8a96f5c10dd02f5a2f895b0ff56d4bbd.png)

So a JPG file worked ok, now I need to workout where it went. Trying the other folders I used last time didn't work.

Over to gobuster!

Privacy looks interesting.

![e49e78fd96700c6a0b04db38a13b81b5.png](images/e49e78fd96700c6a0b04db38a13b81b5.png)

We have a winner!

![e281bb3b64cc2368a2bb13962b82ee02.png](images/e281bb3b64cc2368a2bb13962b82ee02.png)

So now we know the location, I need to try and upload our reverse shell.

Using a similar process to last time, I'll re-use our Burp captured upload with a valid file, edit the request and see what we can get! First I'll try adding in two file extensions e.g. rs.jpg.php and see if that works, if not I'll try some other extensions.!

![1b8910ca7f0b3556faae620b19834972.png](images/1b8910ca7f0b3556faae620b19834972.png)

No luck

![e71a69818d01f1a0ce1e7d054a8d8714.png](images/e71a69818d01f1a0ce1e7d054a8d8714.png)

Now for additional file extensions.

![2ed61bc5df1183e3b65288acf1a9ceba.png](images/2ed61bc5df1183e3b65288acf1a9ceba.png)

PHP5 extension worked, we can see the file here:

![c15cb05d299e31a14da5868b5688241d.png](images/c15cb05d299e31a14da5868b5688241d.png)

Now for our reverse shell and flag!

![b962873ce6ba40b53cfda804bcfcdad0.png](images/b962873ce6ba40b53cfda804bcfcdad0.png)

And there we have it!

## Server Side Filtering - Magic Numbers

Magic numbers are a more accurate way of determining file types using a string of hex digits to determine the file type. Our lesson here advises that it is a very strong method when using a PHP server, but might be easier with other languages. 

In this challenge we have levelled up! The directory listing has been disabled, so we'll need to try and find the upload file directly rather than the folder location.

 Here is our target

![6b49e8bf97c0c350fb089d559369dd83.png](images/6b49e8bf97c0c350fb089d559369dd83.png)

I'll start as usual with a standard jpg upload. Lions again.... yes :)

GIFs only, oh dear...

![bf534286086aeb45d9bae631dfbb8d95.png](images/bf534286086aeb45d9bae631dfbb8d95.png)

I'll rename my file...

That didn't work, looks like the magic number filtering is working ;) I'll get a lion gif!

![2ef07a7ee638443d9315606123815b70.png](images/2ef07a7ee638443d9315606123815b70.png)

So here we are with an uploaded file, lion.gif and a valid gif file. 

Now I need to workout where it went!

I'll use ZAP because it's quick! So I tried to load site/images/lion.gif now I'll fuzz the images component of the uri.

Here is our fuzz attack

![d4b83469aeab9efa682a39928fa1e3ad.png](images/d4b83469aeab9efa682a39928fa1e3ad.png)

So the folder is 'graphics'!

![9f758b39e629d15a96b8c266e4fc4964.png](images/9f758b39e629d15a96b8c266e4fc4964.png)

Next I need to try and trick the server into thinking my file upload is a GIF!

Wikipedia has a nice list of magic numbers used, here is our gif file magic!

![a946f5417a06f0bef601f1e730683eb7.png](images/a946f5417a06f0bef601f1e730683eb7.png)

So I guess I will try add these HEX values to the PHP file I'll upload...

I've added in the GIF header 'GIF87a' into our reverse shell PHP file with VIM, now the linux 'file' command think's it's a GIF file which is a good start.

![32877eac1fe895ba42491b777b69ea28.png](images/32877eac1fe895ba42491b777b69ea28.png)

Next I'll try and upload this file...

![015e835cda1ddf22f2bb1537f7057755.png](images/015e835cda1ddf22f2bb1537f7057755.png)

WIN!

Now for our reverse shell...

![6da536bcd703c60bd626b42c363fbfc9.png](images/6da536bcd703c60bd626b42c363fbfc9.png)

Victory!

I'll get the flag now...

![4168c145051cf9fb0638f1ddaef6dc8b.png](images/4168c145051cf9fb0638f1ddaef6dc8b.png)

That was fun! now for the final challenge...

## Challenge

Here we have our final challenge, we have a blackbox and and upload portal.

![a01e8877a37e417c9f9a81485f744237.png](images/a01e8877a37e417c9f9a81485f744237.png)

Starting off with uploading my lion.jpg I get a file to big error! But I'd guess (due to the speed it comes back) it is a client side filter. 

So the first thing to note is that this site is not driven by php, according to wappalyzer it is a node.js site.

![5e16cb4ff4123d6e5db8ac2f0bbb3438.png](images/5e16cb4ff4123d6e5db8ac2f0bbb3438.png)

So we can assume that a php shell will not work here.

Looking through the source code of the site we can see a few checks happening on the client side.

![59bc6d8c39049e53c8024f7d8bd002e8.png](images/59bc6d8c39049e53c8024f7d8bd002e8.png)

![bc51988ccfe7c3f85759e2adf621b8ec.png](images/bc51988ccfe7c3f85759e2adf621b8ec.png)

Note there is an error in the code here the file type needs to have an extension of **jpg AND jpeg** so nothing will upload unless we fully bypass this javascript. 

So I used Burp to remove the javascript from loading on my page by intercepting the web request and removing the code seen above. Sorry I forgot to take a screen grab here, but the idea is to intercept the server response, delete the JS code that checks for the file type / content. 

Now I have prepared a reverse shell code for a node.js server which I got from allthingspayloads github. This has been edited accordingly.

![ceb6219e524995df8107808bac7f3322.png](images/ceb6219e524995df8107808bac7f3322.png)

Trying to upload it as a .JS file failed, but uploading the payload as a jpg seemed to work!

![7538d04ad54c08ac42281734804c6327.png](images/7538d04ad54c08ac42281734804c6327.png)

Now I need to try and find out where this uploaded. The lesson advises that it might not be the same file name we chose, so I might need to get creative with a fuzzer.

the room provides a list of 3 letter capital letters, I assume this will come in handy for the upload file name or folder name (or both!).

We got some interesting stuff from gobuster, admin and content folder in particular.

![e8aa8baa700b5befd12024df8a318018.png](images/e8aa8baa700b5befd12024df8a318018.png)

The admin folder seems to execute modules!

![c5b3a27dfe68ed61457eef8bbe099bf3.png](images/c5b3a27dfe68ed61457eef8bbe099bf3.png)

The content folder is not listable, I'm going to assume that this is the upload directory. But loading the 'image' I uploaded by name doesn't work. I'll try fuzzing it with the list we were provided.

Looks like I have something interesting here, fuzzing it with ZAP and we have FKJ as a potential file we uploaded.

![71685fc6544f0adfe373b9637fc74cba.png](images/71685fc6544f0adfe373b9637fc74cba.png)

We're on the right track here, the file contains errors as it is being loaded as an image....

![536e6c6883f4278846efb220a34004ee.png](images/536e6c6883f4278846efb220a34004ee.png)

Meaning we have found our uploaded JS file!

Now to try and reverse shell it, using the admin page that runs modules.

So we get an error when running the FKJ.jpg in the admin page - the file doesn't exist, perhaps I can try a relative path!

![d87e5112afe714bf6f64f9fb9aba83f8.png](images/d87e5112afe714bf6f64f9fb9aba83f8.png)

I'll try ../content/FKJ.jpg

Bingo!

![4c7c7a64a8d33715b44ebbd566ad9bd5.png](images/4c7c7a64a8d33715b44ebbd566ad9bd5.png)

Our netcat listener got a hit, running the relative path from the admin page has worked.

Now for the flag!

![7be492981ca27476f793f2185662e216.png](images/7be492981ca27476f793f2185662e216.png)

Presto!

That's it for this room. :)