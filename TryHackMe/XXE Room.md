# XXE Room

This room aims at providing the basic introduction to XML External entity(XXE) vulnerability.

In this lesson we're given an XXE payload field to work with some XML.

![69ba1a02d05b555eede67ab6e30aee32.png](images/e07c8a890cfc4aa098e741225dcacb1a.png)

Using a simple payload we can use an entity and assign it a value 'feast'.

![3942df5235fe1ae4450777520805017d.png](images/0011fc5ef71f4c6ea3c3d48274c009c0.png)

We can use such a variable to call a system file, below as an example we call the /etc/passwd file.

![ac1801ad7317962cd875fd49c4a6dbd3.png](images/9cc0c38ea5814b198f7c0b4086ecba8e.png)

So here we can see the user accounts on the server, the one we're interested in is falcon

![46cf1b441c9a8689b345132d2212de4b.png](images/1453aadb6c3d454fa6c7109a49ffb322.png)

Now we could try to read their ssh key to obtain access to their system.

Knowing their username and home folder we could try and read /home/falcon/.ssh/id_rsa which is the default ssh id.

And here we have it!

![45acc73913b8389e3e9beb05d059aca6.png](images/3cfa85f1cbd547718c856f73c8cd823a.png)

We could now attempt to login with that ssh id.

That's it for this lesson.