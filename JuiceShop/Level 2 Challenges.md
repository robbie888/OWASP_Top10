Level 2 Challenges

# Level 2 Challenges

The challenges below are listed in alphabetical order, not necessarily the order they were completed.

## Admin Section

### Description:

Access the administration section of the store.

### Exploitation:

After completing the Login Admin challenge below, I started hunting for the admin section of the site.

Searching through the source led me to the path 'administration'

![3976766b2bc41da42f33600e10c7aaa8.png](images/01d73bee4ff14371a9e1ad9b5e735b49.png)

To access this part of the site I needed to be logged into the Admin account.

![84a7b06bdad84b1bc050dc99157fe91b.png](images/01e0d5e921a54889a490718237cdb2a7.png)

Accessing this administration interface completed the challenge.

## Deprecated Interface

### Description:

Use a deprecated B2B interface that was not properly shut down.

### Exploitation:

First I started looking around the code, and found a comment reference for B2B and invoice uploading.

![139ea504a9f79ae88b78fd3d7285247a.png](images/6493ed6dd7b247eaa1e7fa145a98d6f9.png)

Clicking around the site a bit, I found an 'Invoice' upload in the complaint section, which seems to be out of place.

![e3adb74a6280dda0f33b7033c0ddde3a.png](images/a2693ea3cfed42fba46356f2376fdf58.png)

Uploading a file here completed the challenge.

## Five-Star Feedback

### Description:

Get rid of all 5-star customer feedback.

### Exploitation:

This was quite straight forward, in the administration section we accessed above, we can see the customer feedback. There is (was) only 1 5-star rating, I deleted using the rubbish bin icon it and that completed the challenge. I forgot to take a screen grab before deleting, but here is the feedback list without any 5-star ratings.

![e8b14ed7bb7c9b5ea7fe4bb8615910a2.png](images/79c03612b68f4e3996c1d3913e349526.png)

## Login Admin

### Description:

Log in with the administrator's user account.

### Exploitation:

In this challenge, having previously gone through the site, I noticed that the admin account had left reviews on some of the products. From this we have the admin's login email as below.

![4eb26d3410f0cb883bec8dd01f1861b0.png](images/5925bb325a824b55be017734c8d22b24.png)

Then I referred back to one of the challenges in Level 1 where we received a SQL error in the code after entering in a single quote in the email section of the login page. Here is the error.

![9210bd957202098226995e139c3be8f9.png](images/15030c8b03c547c6936873d7c0a03a63.png)

Looking at what Burp Suite captured, we can see the SQL error and the code used to check credentials. This can easily be bypassed.

![8291afec38321022cf708dc656d9df9e.png](images/c3e6847246674bcd977eeb36834e4da8.png)

Here we can see the vulnerable code, the singled quote was put right into the SQL query.

So now to make the payload.

I'll use `admin@juice-sh.op'; --`

This should look up that account in the database, and comment out the password check part of the query.

![9a92c3cb998d93310fd6b1347c9203c9.png](images/b79e2f8885a84bf0ae2b9266b3c3ea99.png)

I put in a dummy password, and clicked login. 

![225ffc4a519929a9c977f0e4a587e30e.png](images/67c91b8ce2ae410f9ebb1079c8ec945b.png)

And we're in!

## Login MC SafeSearch 

### Description:

Log in with MC SafeSearch's original user credentials without applying SQL Injection or any other bypass.

### Exploitation:

In the administration section of the web-site we can see MC SafeSearch's email login.

![15c0fc0f9e17e57afb7761448c386a89.png](images/2e05f2fc89e64f699189b6969a71e088.png)

Googling MC SafeSearch comes up with an interesting music video about passwords and security. During the video the rapper states that his password is Mr Noodles, but he changed the o's to 0's so it's secure.

After trying a few different variations of the password to login we end up with `Mr. N00dles` as the login.

Now we're logged in as MC SafeSearch!

## Meta Geo Stalking

### Description:

Determine the answer to John's security question by looking at an upload of him to the Photo Wall and use it to reset his password via the Forgot Password mechanism.

### Exploitation:

Looking through the user list from when we logged into the administration section we can see a username john@juice-sh.op.

Then going to the forgotten password section will show the security question only after a valid user account has been typed in the email field. (This could be useful for user account enumeration!)

![d7a9366d74fe1a77567aa86002d30167.png](images/b50d9075a1bf457d8bf70e07158fd912.png)

We can see the security questions is about John's favourite hiking location.

Navigating to the photo wall we can see a photo uploaded by 'johnny', I am assuming this is John's picture.

![8011720827527373812e34a243e07a6a.png](images/3cb1519856564893bcc4121fe9d51599.png)

Next I downloaded the image from the site and used an online tool to view the geo tag info. There are plenty of sites that do it easily.

![3a0700f065df124de8df3005b038ed3d.png](images/d85e6f76f70d4d718cb3b46e96342d00.png)

Now I'll copy the long / lat details and paste to google maps to see what that area is called.

![f1f43a974ff63866f95abe3f9959dd84.png](images/9365330296b04f3f9403c8395924e5c7.png)

After trying a few trail names and forest names around the area, it turns out Daniel Boon National Forest is the answer, pasting that in the security question below allowed me to reset the password.

![d287a3a3e9bcf2fe066ec18c278a96df.png](images/b47767e55bdd4018a346e7fae3fefd50.png)

And now we have John's account!

## Password Strength

### Description:

Log in with the administrator's user credentials without previously changing them or applying SQL Injection.

### Exploitation:

This challenge implies that the password is weak, so I'll try a brute force attack on the account login page. Since Burp Suite Community is throttled, I'll use OWASP ZAP for this part.

With ZAP setup and proxying my browser, I'll first capture a login request.

![cf92838a0af342b5ec97e274e42bb2e2.png](images/5bd8bdad97a94e93b21835c476caa9ca.png)

Here is the request:

![1debd05a6e9b53ad7acafeb537a5a2b8.png](images/bef68e5bb63841c98d5fe762c5e0b429.png)

And the response from the server

![2f4b01f2afd4897d6c514985c756a717.png](images/007e638c7a134d0eb189e70847d4fe54.png)

I'll send this over to the Fuzzer and load my default password list in Kali (rockyou.txt).

Now we have our fuzzer setup with the password list attacking the password field.

![bf66d32135855842044ca874fc98e6a2.png](images/56ad235d0bcb4c8c858dc5b22d151f13.png)

I'll kick it off and go have a coffee!

Coming back after a short while, I sorted the responses by size and found something interesting ;)

![12404a750f221394567255e6cdec71a5.png](images/823486f67a9347c88763be20daddbce8.png)

There we have it, logging in with admin123 as the password completed this challenge.

## Security Policy

### Description:

Behave like any "white-hat" should before getting into the action.

### Exploitation:

This description was not very helpful, it turns out that it is referring to securitytxt.org which was a hit provided in the course.

This site suggests using a file in the site called .well-known/security.txt as the contact information for site administrator to report any discovered vulnerabilities.

![889a76eb61ee3aab25f129210e5aff3e.png](images/7312df823bd64282b4be2f36cbe30c81.png)

Navigating to this file completed the challenge.

## View Basket

### Description:

View another user's shopping basket.

### Exploitation:

In this exploit we want to login and view another user's shopping basket. There are two ways that this can be done. Using Burp suite and changing the GET request, or changing the local session data in our browser.

First I'll go through the Burp Suite method.

I'll login with an account we exploited Emma (see below for exploit).

![0907d22e27d9cc0f82838c9f236fdb5b.png](images/06c295ad4db8449bb2f760524a218bce.png)

We can see she has nothing in her basket.

Having a look through what Burp captured when navigating to the basket I can see an associated ID for Emma. Basket 8.

![095902fd397838d38acdac2aa9eae3ab.png](images/9167b0b343c54192a90fd28a600042ac.png)

So I'll navigate to the basket again, and Intercept the request with Burp.

Below we can see the GET request.

![829299db3ce2a89620166a42cb7ffb0b.png](images/0175295090ac4f748b1e0ea39bc7d437.png)

I change the basket number in the GET request above, in this case I changed it to 1.

Now we can see some items in the basket, indicating that it is not ours. The items belong to whoever owns basket 1. 

![9c3aefae200e7e32328609dcfc942ad8.png](images/4a45dc16d4b542be974ce0213e1a2845.png)

The other method this could be achieved is using the browser inspector and changing the local session information.

Using the inspect feature in my browser I navigate to storage and session storage information.

![763b6929a0e10444946ec42b9e9d3355.png](images/c0317a9fef504ada81c1a58526145274.png)

Here I can change the 'bid' value (i assume this stands for basket id).

![e0c50cb886df1b36fc090de175a8efd0.png](images/0776685f869749b59415142120982205.png)

This time I changed it to a value of 2.

After refreshing my page, we can see another basket here.

![9d7924a102a926061b35d874df0f2371.png](images/f1be18c937e14e80a30e30e6fc47a3fb.png)

Accessing the baskets either way completed the challenge.

## Visual Geo Stalking

### Description:

Determine the answer to Emma's security question by looking at an upload of her to the Photo Wall and use it to reset her password via the Forgot Password mechanism.

### Exploitation:

My assumption will be that Emma's account is emma@juice-sh.op. we can verify this in the Forgot Password section, if we see the security question when entering in the email address.

![c916f0a34826a22e1cfae38a39e4c2df.png](images/c1782cbc92cb42bfa5ac7b559e985238.png)

Here we can confirm that Emma's login is as I assumed above, and her question is about the first place she worked at.

Now we'll have a look at the photo wall as per the challenge description.

Here we can see an upload but Emma showing us her old workplace.

![b135b1947ba9d55a1d738f83891e9298.png](images/2ac82632474d4bdbb31c9298a501bb80.png)

Let's download this image and see if we can see a geo tag or anything up close in the image (since it is a 'visual' geo stalk challenge).

No geotag was on the image.

![95f49bf970e8c72770d9ec419e75b0cb.png](images/4d1499edcd894704a056537ec3f96922.png)

The only writing I could make out on the image is here:

![b02dab155cd6fd8d8d3dbadccfa71e2f.png](images/b5553406a967430f88dae8876f8d818e.png)

Seems to say ITsec, so I'll try that and some variations of it.

Well that worked on my first go, 'ITsec'

![589266a23c4d09b715bb1fa058c51e7b.png](images/17518174ab0841bfb39e64ac2544c7db.png)

Now we have Emma's account.

## Weird Crypto 

### Description:

Inform the shop about an algorithm or library it should definitely not use the way it does.

### Exploitation:

Reading through this article about cryptography: https://wiki.owasp.org/index.php/Guide\_to\_Cryptography#Cryptographic_Algorithms 

There is a section outlining the vulnerable algorithms. MD5 being the first on the list.

Reviewing the previous server error we received when performing SQL injection, we can see the password hash in the error.

Here is a snippet from the error:

![7a48493fc93706832e989afbdcc548ff.png](images/60dc4c37fee24e9cac91e6c690ffa704.png)

I looked this hash up in an online hash identifier tool. One of the possible types was MD5.

![57effe5e007d369ea3faadc2956d2287.png](images/01f9c4bf8b114871a17d25ddfdd6bac6.png)

I then verified this using an MD5 decoder, and it decoded to the correct password - confirming that MD5 hashing is being used which is insecure.

![fdde2b8d2a0a331d6e81b00dfdf07d7f.png](images/ad8da6aa824b443ea869153ac748f07f.png)

Entering MD5 into the customer feedback form completed this challenge.

Note there is one more XSS challenge but it is not avaialble on my Docker version of Juice Shop, so I'll add that at a later date when I have a chance to make another instance of Juice Shop.

That's all for this level!