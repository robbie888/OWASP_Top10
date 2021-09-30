# SSRF - Server Side Request Forgery

This room aims at providing the basic introduction to Server Side request forgery vulnerability(SSRF).

We can use SSRF when the server makes a request or another URL without sanitising the input. We can use this to access URLs that the server might be allowed to access, but external devices cannot. E.g accessing a local port that is otherwise blocked by a firewall.

Here we have an example of a vulnerable SSRF form, we will try to determine how many ports are open, internally and externally.

![1dff18fc2a3dd72adc5163b03bf8f8bc.png](images/1dff18fc2a3dd72adc5163b03bf8f8bc.png)

Trying this payload we can see we get an error, so there is some filtering happening.

![49721912408c5835fc0c0b6a12fd83bd.png](images/49721912408c5835fc0c0b6a12fd83bd.png)

So I'll try another payload

Trying these payloads resulted in the same error http://localhost:3306 and http://:::3306 and http://\[::\]:3006

Next I'll try a hex based payloads: http://0x7f000001:3306

This was a success:

![906489d03690f3ff57014cce51593f28.png](images/906489d03690f3ff57014cce51593f28.png)

We could now use this process to try and determine which ports are open.

I'll use ZAP's fuzzer, here I've captured a request

![aab6ac29032f089101df3addac4f76c6.png](images/aab6ac29032f089101df3addac4f76c6.png)

Now I'll fuzz the port component of my request and look for different sizes in the response.

Here is the fuzzer setup to scan 10000 ports. We could make educated guesses on the ports manually e.g. look for standard DB ports or SSH etc, but I wanted to try this method.

![fc8a26f9c15e4f5d52266262f3957a30.png](images/fc8a26f9c15e4f5d52266262f3957a30.png)

After scanning for a few moments, we already have a hit, port 22.

![8fc19ee16513ee41cd36b1d602e096f6.png](images/8fc19ee16513ee41cd36b1d602e096f6.png)

We can see the response size differs slightly.

The response size of 1041 is a success:

![2b1bd59a156d90e0816f9dac7a174f64.png](images/2b1bd59a156d90e0816f9dac7a174f64.png)

After about 5 mins the fuzzer completed scanning 10000 ports and we have the following results. Note that the first result was the original payload using port 3306, which is a duplicate results.

![2621b50e1c7cfd4eda9c5864fffa50a4.png](images/2621b50e1c7cfd4eda9c5864fffa50a4.png)

As per above we can see 5 ports open to us: 22, 3306, 5000, 6783, 8000.

Next we'll try using the same method to read a local file with the payload file:///etc/passwd

![660fbf340fcb8b1c5acac9f1c164a862.png](images/660fbf340fcb8b1c5acac9f1c164a862.png)

This was a success! We can see the file contents here!

That's all for this room!