# ZTH: Obscure Web Vulns

Learn and practice exploiting a range of unique web vulnerabilities such as SSTI, CSRF, JWT and XXE.

## SSTI: Server Side Template Injection

The first part of this section discusses testing SSTI with a common payload of {{2+2}}

The course suggests this site when looking for payloads: [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server Side Template Injection#basic-injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#basic-injection)

The material also mentions a tool called Tplmap which can detect and exploit SSTI.

I have grabbed a copy of tplmap in case we need it for later!

![3b7fdf78220a68011ade6c1cb6fc5120.png](images/7dbbc18f766240c8aaacef873c8ec76a.png)

Now to try what we learnt in a small SSTI challenge... We're looking for a flag in the folder /flag.

Here we have the vulnerable site

![0b376ab146cebfc05690e8639c4e24ab.png](images/4215218304554635ac4b2cebbcbf361a.png)

We'll try out standard test {{2+2}} and see if it works.

![f284ae324dd989cc08b0bbb051aabeb8.png](images/5e7f9630ab4141c49050d0ccf808919c.png)

Victory!

Now looking at the source we can see the name of the vulnerable entity, the form is using POST.

![de7c558e6646701b66d5cffdd8c22f82.png](images/d1ef32317c264cf281fca70fc29f3bfa.png)

I'll try out tplmap!

This worked well, we have identified several vulnerabilities here

![43e0851bc58076c8f7a03d5478ecb01d.png](images/d863e4ba9f894161a6b3e85e0f3cd651.png)

Now to look for the flag

Here is the command:

![c9beb412b442db6aa857a07a83708d26.png](images/d8b6dd9fc1d240b3986e78b0c4dffe3c.png)

And the flag in the results

![ff030022cc273a455da1afc6d827755d.png](images/6d605bb09a4a48f38e5ed952f2f34443.png)

We could run many things now, reverse shells or more!

## CSRF - Cross Site Request Forgery

CSRF involves submitting a request to another site.

There is also a tool that can be used for this called xsrfprobe

I've installed this on my Kali box for future use :)

![2aba969b20dd1ad5c18f418d79496d9c.png](images/25f75a94a84044e6bdbd077dd539ff7c.png)

There is no flag challenge for this section.

## JWT - JSON web tokens

JWT tokens are generally quite secure, when configured correctly, but if mis-configured, they can be exploited. By changing the encryption type requested in the JWT, it maybe possible to exploit the token.

In this CTF we have to re-encode a JWT with the server's public key. We're given the location of the public key.

This is a useful resource for JWT [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/JSON Web Token](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/JSON%20Web%20Token)

In this exploit we're going to enable HS256 (symmetric encryption) on the JWT with the provided public key.

We have our token, and I'll decode it from base64 with Burp

![9b05720c327ad4a8c7885d132d5518a0.png](images/94fed6f0252d4f958325c3adeee8d23b.png)

We have our public key here:

![de32951c08c81b680ee7dedebd6c0e9c.png](images/955f7f51049c48a7b30ff9c37ed32973.png)

We'll convert the key to hex for openssl.

![84669720f71cc34582e04fd1aae929ab.png](images/62f933b473f54a49a990c31379c4f4d0.png)

Now I'll change the header component of the of the JWT with HS265 and re-encode the header and payload with base64

![e42d88ed2117a44754e48990bada16d4.png](images/b677294037b94b44a5e6ae1702ae8211.png)

Now with open ssl we use the public key to sign the JWT header / payload we created.

![14e1b56755481314ef502975a367e038.png](images/98d080ab156849bdb6d3dac10fab01d1.png)

Now we have to change the signature here from hex to binary then base64.

Using python we can covert it as so:

![d5081ac4994b4c4a0c0d1262e8d98e85.png](images/d74982e041464053bf8a8c178b0b66d9.png)

The last one should be our signature in the appropriate format.

Now we construct the JWT with &lt;header&gt;.&lt;payload&gt;.&lt;signature&gt;

Here it is:

![5eda073c48dc1efe0195630e7671369d.png](images/6e5767f88813449390a6b01227772d37.png)

Now we get our flag!

![d50e26c0d9d98662e8550755f16e163c.png](images/8f36e056a7c64442b35e2f5446f26faf.png)

This process can be significantly simplified using this tool: https://jwt.io/

The next challenge is to login as admin on a vulnerable site, using the 'none' encryption algorithm.

First we login with the demo account provided

![0b9ec7d0cf8b94ede48bbcbf00fe7f37.png](images/56b691b5c93644e2bc5c16f9044bc1d6.png)

We'll get our token from the developer tools and inspect the token in the jwt.io tool

![fc10dc76f8101b5f1d341b03080c1790.png](images/98bb8b2ad5fb4426b22f0c7e443ae470.png)

Here is the decoded token and interesting fields

![332171b7ade8ccd9ae5c1223134cf5db.png](images/025454e3ea1f4efb86c8982ff14748b6.png)

I've made some changes to the algorithm and user role using burp.

Here is the algorithm changed and rencoded to base64

![4d6e2b8f4eb83a543287e8559908933d.png](images/8a05e637dedd41c183ad2f0cf4cadc72.png)

Now for the payload:

![985dc2adc45ff3b884438197da111931.png](images/2dd697d7a2a445d9bddd25dcec27ee40.png)

I'll reconstruct the JWT with &lt;header&gt;.&lt;payload&gt;.&lt;no signature here&gt; then paste this token back into my browser's developer tools and refresh the page. Note I removed the trailing equal '=' signs from the conversions above.

And here we have the flag! I am a admin!

![c13448bb8031e16a531b58ac8819686d.png](images/18ab4c85821c4b98bb1ad9e246bdf3da.png)

Now we're going to brute force a JWT token using a tool call jwt-cracker found here: https://github.com/lmammino/jwt-cracker

Given this token `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.it4Lj1WEPkrhRo9a2-XHMGtYburgHbdS5s7Iuc1YKOE`

We know the key is 4 chars long, I'll start with lower case letters and numbers.

After a short while we have the key!

![1050d9600761f51bed5d8fb5012cd018.png](images/a916c301892145a390c0877028e8d06c.png)

## XXE - XML External Entity Injection

XXE can be exploited to read resources outside of the web directory or even RCE.

Here we are given a vulnerable XXE site, we're going to try and read a local file.

Here is our registration page

![fbecb4dbceec75667e62eef7933055ea.png](images/1b8d5c6d69a24e4d8bfc98c720eda61e.png)

Using Burp we can intercept a registration attempt.

We can see out request here has XML code in it.

![67e2a03becc46ff4fedd414bee8ea831.png](images/8cd1e1806c484bac8bf1c90e2d1f60b7.png)

I'll send this over to the repeater module in Burp for testing.

We can see in the response that the email address is reflected back

![d53a279dc4327b3497b2a7fcaa52af2f.png](images/e1b67e41c3534047ac02ed5b931455ab.png)

So I'll add a payload to the XML request including an attempt to read a local file /etc/passwd and output it to the email field.

![64d19bfe0ff0290a1246a115df29d789.png](images/bef59489faf94a5086d70d5693b0f6a7.png)

Here is our response, a success!

![6eed571137a98246c0d1b2228d75eab9.png](images/99c2ac381fb94f11acfa6e85f2540ed9.png)

That's all for this room!