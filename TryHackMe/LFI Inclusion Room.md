LFI Inclusioon Room

# LFI Inclusion Room

This is a simple room, designed to exploit LFI on a web server.

The room has two flags a user and a root flag.

Here we can see a possible attack vector in the URL.

![9757e51db6fa0bad54ac994971ed19f7.png](images/fe7c41dbfd8d4bb1ba19991c81a11da0.png)

I'll check it for LFI, considering the name of the room ;)

Success!

We have a few use accounts here we can inspect.

![27d7f7fd7e3818df592a48ae80e24a09.png](images/f5d2f8e5682b4b8c8738182f5dbe9ce7.png)

There is also an interesting comment in the above file

![84a9cec1228f0cf5168f532bfa755a3b.png](images/1fad47c5b94541edb6c3abb0e2f04d8e.png)

Those credentials have worked for us!

![8a8c8c878f2178981d365d5cc3ba3ca2.png](images/5dda1259443d48ae8a53a0bb58d05ac5.png)

Here is the user flag

![054959b203dfadcabfa55e098aa98a61.png](images/2d518dc097304830bd3189aeff982660.png)

Now for privilege escalation, socat might be intresting

![9e23aa29b0eec8d7da529434f1cc0d00.png](images/763ab4b0edaf45b3b785521bf218be0e.png)

This link is helpful https://gtfobins.github.io/gtfobins/socat/

And here we have it, root access and the root flag!

![3240ea77298ff8c39d4d4ba99857f47f.png](images/39ae9f770afc475e8f7e3d3e774f46ce.png)