# Avengers Blog

Learn to hack into Tony Stark's machine! You will enumerate the machine, bypass a login portal via SQL injection and gain root access by command injection.

Let's hack the avengers!

![ed0d1fdf699eae756b03886f3a6e70d6.png](images/ed0d1fdf699eae756b03886f3a6e70d6.png)

Hope I don't get hulk smashed ;)

Here is our blog

![ec243881ff9bd1d73835fc18f2849341.png](images/ec243881ff9bd1d73835fc18f2849341.png)

For our first flag we're advise to find the cookie!

Here we go!

![185f6d135696e4840523b410e6373f80.png](images/185f6d135696e4840523b410e6373f80.png)

The next payload is in the response header from the server, using Burp we can find it easily here:
![fb8d71edcb2c2b045f0c899743e45f73.png](images/fb8d71edcb2c2b045f0c899743e45f73.png)

Next we need to access the FTP server on this device.

We've given a hint to look at the forum, we can see groot has a predictable passwords...

![bff33839a6426fa4eeab3197dff04cce.png](images/bff33839a6426fa4eeab3197dff04cce.png)

And we're in!

![8190a81f6e0fdb904356923427d2a714.png](images/8190a81f6e0fdb904356923427d2a714.png)

Here is our next flag

![35fbb4ab3551c7bf78da85682aee1664.png](images/35fbb4ab3551c7bf78da85682aee1664.png)

Now we need to look for some hidden folders, a login page ideally.

Gobuster here we come!

![eb1922a5bacb49b4be2f5cbce423e814.png](images/eb1922a5bacb49b4be2f5cbce423e814.png)

Some good stuff here, we're interesting in 'portal' as this seems to be their login page.

![3d92959665ab96b14b8abd6b82defd4e.png](images/3d92959665ab96b14b8abd6b82defd4e.png)

Now we're going to login here apparently, there might be a SQL injection flaw! Tony... shame...

![7e6c3b4dd2e62003e1b5a09a7da8ba1b.png](images/7e6c3b4dd2e62003e1b5a09a7da8ba1b.png)

Username and passwords as the above!

And we're in, not the flag at this point (flag4) is the line count of the code for this site. 223.

![6cf946e0810e6a19878274f6a20a2bfe.png](images/6cf946e0810e6a19878274f6a20a2bfe.png)

Command injection time!

A little hunting, and we can see a flag file, which is what we want!

![319ba6c5b998d6aef6f5d3df9762cf39.png](images/319ba6c5b998d6aef6f5d3df9762cf39.png)

Command is disallowed :(

![2aab0d80fc37c9c83f99df71b2f6dc6c.png](images/2aab0d80fc37c9c83f99df71b2f6dc6c.png)

We need an alternative to cat i think...

![d30b37c9df304980c59b2f62c3887f1d.png](images/d30b37c9df304980c59b2f62c3887f1d.png)

Bingo!

Our flag done :)

That's it for this room.