# Level 3 Challenges

The challenges below are not listed in any particular order, this list includes most of the Level 3 challenges.

## Admin Registration

### Description:

Register as a user with administrator privileges.

### Exploit:

Here we will register a user as an admin.

After registering a user, we can see the API section in burp.

![4a4f415682b1cadcda76177684f3ad61.png](images/32cab50a73614564ad9431b3e860c316.png)

We can see the API request and response here:

![f36b2efcc54ba6033d224c6aa74bdc37.png](images/1df93f6f5a624182a6530ab3c9605f9c.png)

In particular the 'role' field looks interesting. We'll send this request over to Repeater and resubmit it to register a new user, but this time I'll add in the 'role' field manually in burp.

Here is my request and response, we can see the site has accepted my new account as an admin.

![951dda26b64a7cdbdfcd52f65dc264b3.png](images/347bc4e1271643a5bc27393280dd911b.png)

This has completed the challenge, we can see my new testadmin account has access to the administration console.

![3686bc9e8625edbb4de9ca154dc55b80.png](images/4e43e7287cb145fa9dbbd13897f10775.png)

## Captcha Bypass

### Description:

Submit 10 or more customer feedbacks within 10 seconds.

### Exploit:

First we'll submit a review and capture it in our Burp proxy

![29311fdc0b1aa43ccc4e7e3218e00d7f.png](images/ed1726edec754e77a72a8de62cba3b60.png)

We can see that in request includes the solved captcha and a captcha id.

![2014d854587e75836ab1c4581caf6021.png](images/7ef937f0842c41f9ab95ad943bafb862.png)

I'll assume that the ID is related to the response. Se we can send this to the Intruder and try submit multiple reviews in a short period. I'll use a simple number based payload and see if we can submit multiple reviews quickly.

We can see this has worked, submitting several reviews re-using the captcha ID and response.

![e277214c5ca9de261796774676c9eff2.png](images/0e0d50d68d344cebb093b98a2dac7a49.png)

## Basket Manipulation

### Description:

Put an additional product into another user's shopping basket.

### Exploit:

First I'll add something to my basket, and capture the request in burp.

Here is the relevant request with the details about the product.

![a88633b89df4438cb4809475a35a71d5.png](images/4efb69e2edd1484d878f573d619353db.png)

Note it includes the basket ID in the request.

If we change the BasketID and send the request, we receive an error.

![e35fcf4b384ae3d0c3f615e91953debb.png](images/6d412f224a714d94b038a1ce19a0698c.png)

So if we try to add another field in our request of BasketID to reset the basket ID we'll see what happens.

![8976bbb364f6f446b6ecd5fe3647449c.png](images/b64964839745435b9fa765203fc14a5a.png)

This worked we have managed to bypass the basket check.

Submitting this request solved the challenge!

## Payback time

### Description:

Place an order that makes you rich.

### Exploit:

Here I will attempt to add a negative qty of items to my basket.

The interface doesn't allow us to reduce items to a negative number, but I'll make a change to the qty and capture it in Burp.

![46b4bfc754f646d417d16d17c4933766.png](images/eb90a36d26ce4cdca7744bc41ebe4e50.png)

Here is the relevant request.

![397795b95a972c4d6563a584cd94a7c6.png](images/b8a4731dd1344bfca84977d731e7bf77.png)

I'll send this over to repeater and send some negative numbers!

It has accepted the negative number.

![86044dcb4393fa9be6fa7dbcd9d55d08.png](images/beef3e80b8b04df0a02dcfcdd51ab938.png)

Now I'll try some larger ones!

Again it has accepted it, I used -300. And It also shows us in our basket that our order is a negative $ amount!

![96d41a6c88c5c36e2c29c94d56d7726b.png](images/acfdf21be4254594884b7648bead8047.png)

After adding some larger amounts, and checking out my wallet balance has increased!

![6364e9b82b487ba0a9f07b94f6593c47.png](images/5a6e2f8e18914a6fa4e92fb66e0805bc.png)

This completed the challenge.

## Forged Review

### Description:

Post a product review as another user or edit any user's existing review.

### Exploit:

First we'll post some feedback to a product, capture the request and see what information is in the request.

![43b67aa3d1aa565abdfda5b315488de1.png](images/566dfcd2088147af977345f7973d2c89.png)

Capturing the request, we can see that it includes an 'author' field.

![65d1680f3c16477691ff242f94fa11db.png](images/37370d286992412e91d3740f272b3b1c.png)

I'll change this to admin@juice-sh.op and send it off to see what happens.

![c805296f2292a126cbc02ff1a47fca5b.png](images/95554ccab61c478cacd0d580f5e8a540.png)

That did it, we posted a review as another user. This solved the challenge.

## Forged Feedback

### Description:

Post some feedback in another users name.

### Exploit:

We'll go over the customer feedback and submit a feedback form, capturing it in burp.

Submitting a feedback form and capturing it in burp gives us the following

![5434ce12ea3064eafbbe8cc7fc35016e.png](images/50c8bd8cae5041e2839f1dbc1364a023.png)

The UserId looks interesting. I'll change this, submit it and see what happens.

![7a706eae16d6940fdfde482ee61e513f.png](images/2e9e3328699e460a81b4aa02828261f5.png)

Submitting this with UserID 1 solved the challenge.

## Upload Size

### Description:

Upload a file larger than 100 kB.

### Exploit:

If we try to upload a file in the complaint section, larger than 100kg, we receive an error.

![fe97169ba1d0ef4cf295a40276fec08c.png](images/c21097ee06a240c091a9a3b47987b640.png)

I'll try and submit a file, then capture it in Burp to see what we get.

Choosing a smaller file and capturing it in burp gave me this:

![899bd2471017b3f7c6a6a1653571450e.png](images/9bc383c01a06428c95ecc5465d411442.png)

I'll see if I can add more content to the data component where it says 'this is a file'.

I've send the request over to repeater, pasted in a large pdf file.

![0f60fc6f5f44abd45ed3da5c45f78fe3.png](images/5652d52d8e944f18b66310773dc16d4a.png)

Then we submit and that will allow us to upload the content exceeding 100kb!

This solved the challenge! A note to self, using the repeater here is a must as it automatically updates the content-length field of the request. Otherwise it is not accepted properly by the server.

## Upload Type

### Description:

Upload a file that has no .pdf or .zip extension.

### Exploit:

This was a very similar process to the above exploit (upload size).

The only difference is changing the file extension in the request here:

![5f6f2d5019da9566104c279f4edfa99c.png](images/3b4336bb35bd4decb3f08881d9b151d4.png)

Changing this pdf extension from pdf to any other file (other than zip) solves the challenge.

## Login Bender & Jim & Chris

This is actually three separate challenges, but the answer is the same, so I'll only write up once. Note for Chris's login you need to find his email in one of the product reviews or through a SQL injection query to the DB (more on that in an answer below).

### Description:

Log in with Bender's (and Jim's) user account.

### Exploit:

I'll use the same strategy as I used to login to the admin account.

Start off with SQL Injection:

This is my payload and I'll enter in a dummy password.

![d95a45f385dfd13caba1619c79053e2b.png](images/c5a64f5ea6a34e65b1a6dbc4e56e22d7.png)

That did it!

![234b3207b557060459e0a46782a35f28.png](images/d00c2be0a26b4512923dc26533fe6535.png)

Challenges solved.

## CSRF

### Description:

Change the name of a user by performing Cross-Site Request Forgery from [another origin](http://htmledit.squarefree.com).

### **Exploit:**

First I'll look at the source code on the page that sets the user name field.

We can see the interesting parts here

![9bda1361a94ec3e291cf1933aaa9b61d.png](images/e338c4a717d640ccacf8e71e0469eb50.png)

Using the link in the description I'll create a quick form.

![2ebaa0d9dde3f768af7bd8c505167905.png](images/0e2189ef2bea462a9e5854a1aa35c86d.png)

I've setup the above form to post to my local JuiceShop with the relevant fields name and email.

![6ff3b127bbb3c190ffb1a3a8aa23a846.png](images/9790ef72ffdc40c992246a9f1c711c2d.png)

We get a warning in Firefox, but click proceed and there we have it

![31fb7a57a2470dfc625520877a4411e3.png](images/c917d511e23c43c29e5875be908888c5.png)

This completes the challenge.

## Database Schema

### Description:

Exfiltrate the entire DB schema definition via SQL Injection.

### Exploit:

We know that the search field is susceptible to injection from previous challenges.

Attacking this field in Burp repeater provides us with some useful error information.

![3c9bad0912ca0bb51eded9fcb4fc2058.png](images/88d6ea34619d48a3ba1ae4e50702842d.png)

We can see in the error code that it is a SQLite DB.

We can see the query that we need to inject into here

![406a38ecb734af8ba731e4401a91eca2.png](images/9c7f774e40a149548daf5afaaa9e8321.png)

First I'll use a UNION to try and determine how many fields there are being pulled through. Based off a previous query I'd guess 9, as the results of a successful query get me 9 fields. But I'll confirm this.

I'll use a text editor to start building queries

![75dfe0968ec788d69881a70cb1401015.png](images/4e31563d0f7d4b528bf6b2917cf44a40.png)

Here I am trying to determine the number of fields in the query with a UNION.

This gave me an error advising there is more than one field. I'll try 9 now.

![69141b9dc9e51a0c2612696847e98016.png](images/8bff80d9ae8a41d2a66dbddf09472317.png)

![da613f2d97b22d978124ae58dda7e00b.png](images/d5b35dab949d45c6a2b30a5df7284374.png)

When I put in 9x NULL it gives me a different error as apposed to 8 or 10.

![8e2f52e348266f521933da23909e2289.png](images/39af7dd756bb49c482f178277f3d940e.png)

This is because SQLite probably doesn't use NULL, but for this purpose we now know how many fields there are.

Quick research advises that the SQLite schema is in a table called sqlite_master with 'name' as the column names. I'll query this using numbers instead of NULL above.

Here is the query I will use:

`test'))%20UNION%20SELECT%20name,'1','1','2','3','4','5','6','7'%20FROM%20sqlite_master--`

![343945af2c655203b99ce91ea0cd18c2.png](images/be07da13d0c1462c8d3b12498fb012d7.png)

Which gives us the details of the table names:

![b024d63567d22fa97bc6586f6d5d6e2d.png](images/39996e3302144633bab1b378ec08087d.png)

Now if I query for the 'sql' field I can get the table creation commands which gives us the full structure of all the tables, how they were created etc.

![38771f7ddaeaf683ac0481ef9575b6fa.png](images/15e84b5b48cb46cd936b89b39f071c50.png)

This might be useful later! From here we could pretty much query any of the tables to extract information.

We could get the above information in one combined query:

`test'))%20UNION%20SELECT%20sql,name,'1','2','3','4','5','6','7'%20FROM%20sqlite_master--`

This completed the challenge.

### Note:

That's it for now, there are a few more challenges in this category, some a just OSINT on user's security questions and a few that are unavailable in my Docker version of Juice Shop. So I'll come back to those at a later date, as my priority is finishing the training material on the Web Application Hacking course first.