# Mini CTFs

This section I'll put my write-ups and screen shots of the mini CTF  / practical activities that are included throughout the learning materials of the Web Fundamentals Track in TryHackMe.com

## HTTP Requests

This is a mini ctf from the Web Fundamentals HTTP course.

Room: https://tryhackme.com/room/webfundamentals

### Description

There's a web server running on http://10.10.152.244:8081. Connect to it and get the flags!

1.  GET request. Make a GET request to the web server with path /ctf/get
2.  POST request. Make a POST request with the body "flag_please" to /ctf/post
3.  Get a cookie. Make a GET request to /ctf/getcookie and check the cookie the server gives you
4.  Set a cookie. Set a cookie with name "flagpls" and value "flagpls" in your devtools (or with curl!) and make a GET request to /ctf/sendcookie

### Write-up:

Here I will just list the activities screen shots of the task being completed and provide notes where something my need clarity.

![b5449932d148d7614efb34953a19a231.png](images/f2f75e0dcaec4177ac0910817cbf76ca.png)

![8803a5363b60271384adcc6a649929d7.png](images/3e29d211228c473b94eea4ec809da85c.png)

With Firefox:

![cbcccf4a25c634f533c1ec24844fc02c.png](images/613dd1f518e4487f84321f36b4ef51ff.png)

With firefox, I added the new cookie using the + icon in the cookies section of storage.

![472d777635568193f944f2164cd636ae.png](images/6f389a3eb19341e8a3d0b7666b6c4b69.png)

Flag:

![65d82a501f56d160e76387d3671c9118.png](images/c79cda0c41a342f4839c27757f080467.png)