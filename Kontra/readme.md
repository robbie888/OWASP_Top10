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


More coming soon!