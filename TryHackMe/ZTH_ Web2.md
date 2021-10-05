# ZTH: Web 2

A room more focused on the misconfiguration of web servers, rather than sheer exploits

The purpose of this room, is to show the more subtle vulnerabilities. These vulns won't get you RCE, or LFI, but they will allow you to access sensitive information that a client would want to keep protected.

The topics that will be covered in this room are:

\- IDOR

\- Forced Browsing

\- API based Authentication Bypass

## IDOR: Insecure Direct Object Reference

Here we have to exploit IDOR to obtain a flag.

We have been provided a login to a note taking based web site, after logging in we can see the note number referenced in the URL

![35e86dcada77511c7a8daa1a29947705.png](images/35e86dcada77511c7a8daa1a29947705.png)

We can try exploit this by simply changing the note number in the url path.

I'll use ZAP to try several note numbers very quickly.

Here I am using the fuzzer to try note numbers 0 through to 100

![f93fe639a06bc41ba5bbd559c8256768.png](images/f93fe639a06bc41ba5bbd559c8256768.png)

We have the flag! item 0 worked with the flag given in the response.

![de4ca247f054b581c09044721adf53a4.png](images/de4ca247f054b581c09044721adf53a4.png)

## Forced Browsing

Similar to IDOR we try and browse to logically likely url paths.

In this section they introduced a tool called wfuzz which is similar to dirbuster but provides more control over the path.

The example provide is example.com/user1/note.txt here we could fuzz the user1 component of the path to try and find other user accounts' notes.

Here is a target site as explained above

![e4fc787b7efdfeb29210a7c390898c09.png](images/e4fc787b7efdfeb29210a7c390898c09.png)

Here I've started a fuzz, we can see a word of 57 when we receive a 404, so I'll exclude these results.

![d20e3b79aff087907e831083a60ecd11.png](images/d20e3b79aff087907e831083a60ecd11.png)

![becc832e575fbc7f9708b4b0241daa9a.png](images/becc832e575fbc7f9708b4b0241daa9a.png)

Here is the next run, hiding responses with a word count of 57:

![5f77b0e030aaf3de5c43dd1cc604c84d.png](images/5f77b0e030aaf3de5c43dd1cc604c84d.png)

Here we have a hit!

![30276f758976795a1f04cb29af4d121d.png](images/30276f758976795a1f04cb29af4d121d.png)

And the flag is...

![69427a43e0db4dbef5a642794d4de834.png](images/69427a43e0db4dbef5a642794d4de834.png)

## API Bypass

This is a generic kind of exploit, taking advantage of misconfigurations on a site. E.g. Maybe bypassing a login if we know the URL path to the resource we want to access.

In this example we are asked to find a flag in a file flag.txt

Here we have logged in with provided details and can access a command field.

![2168927ac529e83c1085ad229861cc37.png](images/2168927ac529e83c1085ad229861cc37.png)

Typing in a command we can see the path used

![cdd96fab08940a8c8068f60ac56e0491.png](images/cdd96fab08940a8c8068f60ac56e0491.png)

there is an API.php file that looks interesting, this command didn't yield any results thought I'll try some others.

After a few attempts - no results returned, the tutorial talks about having different parameters that we might need to discover. I'll use a similar method as the previous challenge to fuzz parameters on this api.php file.

Here I've setup ZAP fuzzer to fuzzer the parameter in the url path. I've used a word list big.txt from dirbuster.

![ce2271527b6ff96400722fb26e601207.png](images/ce2271527b6ff96400722fb26e601207.png)

doing a few fuzzes didn't yield much, so I started trying the target flag.txt in different locations. Turns out it's directly accessible from the root path. Here we have it

![7637eef053dfe9483549143911887a61.png](images/7637eef053dfe9483549143911887a61.png)

That's all for this lesson.