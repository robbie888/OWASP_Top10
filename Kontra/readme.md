# Kontra Lessons

The Kontra lessons have been quite informative. They're setup like interactive reading materials, providing information on real world attacks, summarising how they were done and with what exploit.

Since these tutorials are mainly knowledge / informational, I'll only provide a brief summary of each lesson I complete here.

## TikTok Cross Site Scripting

In this lesson Kontra discusses cross site scripting and how an attack on a vulnerable search field on TikTok lead to exploiting a political ad campaign running on the TikTok site.

The attack involved

1\. Identifying the vulnerable search field, that reflected the search on the site and would allow for reflective XSS.

2\. Setting registering a 'dummy' domain and setting up web server with a replicate TikTok login portal at that domain. 

3\. Sending the target/victim a spear phishing email in attempt to getting them to open the link with the XSS which in turn redirects them to the 'dummy' login page for credential harvesting.

Since the link in the phishing email actually points to the real TikTok site, but is then redirected with the XSS code, the victim thought they were clicking on a safe link.

Once the victim logs into the dummy site, the attacker has their credentials.

## Ruby Rest Client Backdoor

In this lesson Kontra discusses how a backdoor was installed by an unknown culprit in the Ruby Rest client 1.6.13. Then how it was exploited to gather sensitive information.

The attack involved:

1\. Exploiting a contributor's account to the Ruby Rest client, by credential stuffing from a DB credential leak.

2\. Adding some code to the rest client project which called external code on pastbin,

3\. Setting up a web server which receives information submitted by the malicious code being run by anyone using the exploited rest client. 

As a result the attacker was able to harvest sensitive information such as access passwords for projects using the vulnerable ruby client. When the ruby client was used it would make a call to the attacker's web server, sending over potentially sensitive information.

## SQL Injection

In this lesson Kontra discusses how SQL can be injected into vulnerable code. The example uses a mailing list unsubscribe link to gather database information and extract other private information, such as email addresses. The SQL code was injected using a web proxy to capture web requests and editing the JSON component in the request, adding in the relevant sql code alongside the email address being submitted.

## Command Injection

This lesson discusses command injection using a site called PayHub. The lesson then shows the code of the website, and how it runs an another application (curl) via an OS command to processes a user entered field. The code appends the user entered fields as a string concatenation and does not sanitise the user field. The using an injection payload of `;xxx` we receive and error that the command xxx was not found. Thus showing us we could inject a command here, achieving RCE.

From here we enter another injection to view the /etc/passwd file on the server, which is `;cat /etc/passwd`

The server then responds with the list of user's on the server.

## XML Injection

This example uses a site called PeakFitness to demonstrate XML Injection. The site has an upload portal which allows user's to upload GPX formatted files. The portal accepts XML formatted GPX files. The demonstration shows that we can modify the GPX file, adding in an Element and Entity to load a system file.

Here is the sample code:

```
<!DOCTYPE loadthis [<!ELEMENT loadthis ANY >
<!ENTITY somefile SYSTEM "file:///etc/passwd" >]>
<?xml version="1.0" encoding="UTF-8"?>
```

Then further in the file we add
`<loadthis>&somefile;</loadthis>`

Which should display the file we requested.

Upon uploading the modified GPX file, we receive output on the web page with the file contents of /etc/passwd.

## Directory Traversal

This lesson discusses a site called DealHub, and shows how we can access folders on the server that we shouldn't be able to using directory traversal. On the target site we look at an image being loaded. In the site code way can see a file name and folder being referenced in an URL request that requests the image with GET parameters calling the file name. e.g. `HTTP://site/webpage.jsp?foldername=folder&filename=image.jpg`

From here we can change the file name parameter to `../../../../etc/passwd`

Then instead of getting an image, we can the output of the passwd file!

## Weak Randomness

This lesson discusses a site called CloudZone which offers cloud servers. Here we initiate a password reset through the 'forgotten password' section of the site. We re-submit the request several times and determine the numeric difference in the token values. Inspecting the code for this section of the web application we are shown that the token uses a timestamp rather than a random token. Thus we could predict other tokens and potentially reset the passwords for other user accounts.

More coming soon!