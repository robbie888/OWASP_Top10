# Burp Suite Practicals

The Burp Suite section of the course gives us tutorials on using the different Burp tools.

## Repeater

Here is an example of the repeater, we add a new header into the request captured from the proxy, then forward it on to get a flag!

![b425afcb4d3cf438c3dde317a99e5a31.png](images/d374b56796c0498aacab42b685eb06e4.png)

Next we will capture another request in the products section and send it to the repeater again.

![774ed65997c9fbfdeb3906810d9a7c6b.png](images/d33579a55eb14bac88dace8944e80e6c.png)

Now we will change the product request to try and get a server error 500.

![8c5e1e3bd296cf58c64adec3a8ebd482.png](images/bc29abc9179144b3b9da47e1d966cd23.png)

Trying a 'reasonable' number just gives us a 404 error when the product doesn't exist.

![673747c488b6ba84e29154ddc6b109d0.png](images/53ac54d26bad492c89aa1079890a0c1c.png)

Trying negative numbers gave us an internal server error!

![cf301d1885d1985b5dec0960b027cc1c.png](images/dafb5599090e4f468afa6184dfa88fbe.png)

And the flag we were after!

![39265510913ea08c8555b58ddeb17f22.png](images/9408d8d809d94c6b9a76d8431d1425bb.png)

**Manual Testing for SQL injection:**

First we will request the page with the flaw on it and send it over to the repeater.

![b7fa709660c8834b43e5de31676d2e38.png](images/032603b5a4004d7d926d943ee95b0cee.png)

Now I'll add in a basic SQL injection test, a single quote at the end of GET path.

This generated an error which is good for us (not good for the site owner though!)

![4bf8e9c48d07d3b30d6623b4d541ba0d.png](images/f58d9b83d74c468fa8b21ad1f148bc4d.png)

The response has actually shown us the SQL query in the server error.

![ddf2f8c4bf2fd87985bef02ed08c09e7.png](images/1513228e680f4889818933a1fba6e18b.png)

Now we know the table name, some columns etc.

Now for a more interesting query, to get the columns in the table.

![5aa6a69a59783c61b0e6b7f7d0d4d2a6.png](images/57e2470234424ada882e865278172e3e.png)

Now we have the column name of the first column, but not the rest. So we'll use group_concat().

![e8ffc90e50bf61a970b2e608c61539cf.png](images/459f6eec01064050a69aa5deccfb7ff3.png)

Now we have all the column names! So we're going to read the 'notes' column of the CEO to get our flag. The CEO is id=1 which we can see from navigating the site's about page.

![2c71a0a2569cbe5333a823359b4685cf.png](images/e7d826c91724421e9104ede67b9a945f.png)

Here we have it!

![5fc7bd2b3281b771f41a0adfeab94521.png](images/fa5a5814868e4afab5909bf3b884c956.png)

## Intruder

Intruder is used for fuzzing, there are 4x attack modes

**Sniper:** One set of payloads, putting each entry in each defined position (or parameter). The payloads will not be combined across positions. Only 1 payload will be used per attack. This is more ideal for single position attacks. e.g. passwords with a known username.

**Battering ram:** Takes one set of payloads, like Sniper, but will put the same payload in all defined positions (or parameters) during each attack. E.g. a payload of 'admin' with two parameters will put 'admin' in both parameters for the attack.

**Pitchfork:** Takes two or more sets of payloads (max 20). Pitch fork iterates through both payloads list simultaneously, using the same position in each payload list for each attack. E.g. a payload list of 1, 2, 3 and a, b, c, for two parameters, will use payloads as 1-a, 2-b, 3-c then finish. Ideally the payload lists should be the same length.

**Cluster bomb:** Takes multiple payloads, however iterates each payload list individually, allowing for 'cross' payloads. e.g. payloads of 1,2,3 and a,b,c will iterate like this: 1-a, 1-b, 1-c, 2-a, 2-b, 2-c etc... Essentially iterating through every possible combination in the payload lists.

**Brute Force Login**

Here we will brute force a login with a credential list provided on the target machine.

![a7d41428e5a6e85a57986ba146052155.png](images/686c6e0777c049b3a6a23cba0f22288e.png)

Here we have a captured login attempt.

![f0fc444abb62c6301d0825870f91b29e.png](images/3203e4b160154781bf52ab3e46b470a8.png)

We will send this request over to Intruder to attack this form.

In the request below, we have selected the username and password fields to run our attack against.

![dfa78a4f0debc0e5d4dedac9f7f6e4f4.png](images/f5b839ab7bf542e7a1fcc7da6a0c9f02.png)

We've loaded the provided user and password lists into payloads 1 and 2 respectively.

![2c6c525e1b6aa1e31b4a17093396ccfa.png](images/475d28780d7741069b8fae4befe526a2.png)

After the attack has completed, we can sort by response length or status code to look for differences. In this case the response length was different for one of the login attempts, indicating that this is our user / pass combo!

![d5636535ca3896da75acfd1775468c28.png](images/ee18bed06ed94698842b8798f41e052c.png)

**Challenge**

Now that we can login, we will attack the support tickets part of the site, seeing if we can access other tickets that are not assigned to our user.

Here we've logged in with the credentials we received from the previous exploit.

![679b43274f313c58f129369e8ef8aaf9.png](images/ca890a539fe54a6d8c93d1c3badae9a5.png)

We can see some ticket numbers in the support page.

The ticket numbers are referenced in the URL

![23d49e8e8a1340bce6644bab80781c1f.png](images/be5846e6d4d143c3914bab15acf6b075.png)

Se we can exploit this, assuming access control is not setup properly allowing for IDOR.

Sending our previous request to view ticket 6 to the Intruder, we will use the sniper attack with a range of ticket numbers and see what we can see. I'll leave the cookie in tact so we remain authenticated, and just attack the GET request path.

![b28fb14a57f509367fd168aa8f743835.png](images/5d326195369a4d08b2a7b52640ddedf3.png)

I'll use a numbers payload from 1 to 100.

![8238fa80e2c34a56d4289c07d530f227.png](images/790be02b02a441e1b38356872e9d509e.png)

We can see here that we have accessed additional tickets that our login didn't have access to originally. Indicating the IDOR vulnerability.

![d8b6cbb12f4de54d00898e1e87e270c4.png](images/318e952fd3db413aa1876886edd0be47.png)

And here is the flag

![5c449d7ab5bda871dc603defc299c33b.png](images/008d2e5c8aa44c4c88eb06da5c31561f.png)

**CSRF Token Bypass**

Here we will attack the admin login port, that uses a CSRF Token.

![89ad9cfd38c50c711f44ed29f7d20aab.png](images/556904799185405e8bfe763fcfdd7056.png)

In a captured request, using the repeater I can quickly see that the 'loginToken' value in the response changes and a session cookie that changes. We will need to change these for each login attempt accordingly. For this we can use Macros in Burp.

First we'll send our login request over to Intruder.

![bedb9321faa4a981f5fc91ded0424cf7.png](images/3bff5ff6aea94948909b7e360e7c3d5d.png)

We will now add a Macro to get the CSRF token

![ec626040b640a38bb6e514ec05a5b94f.png](images/373d72b1e4e141d7aa1645a0b3f794d2.png)

Then we'll add the macro to run for our session handling rule, ensuring it only changes the loginToken field and session cookie.

![219844a0a703f0fe182870717a1635a6.png](images/67107b07b82a4c14a481a888ffb3e9db.png)

Now the macro is added:

![4c2d76164a9f2c8447c91f15f36a1069.png](images/310dd8c527314a75a638353168cf29c6.png)

Here is our intruder attack setup

![a444bc063d641dda9ee6f0070b85b9ff.png](images/b6cc9bce8c2f4122904d3f3b559d0947.png)

I'll load the same lists and use pitchfork like the previous attack we just completed.

After running our attack, one login stands out:

![30b4e93b2fd3ea04474bdfe581bec96b.png](images/02e516f0c2454c7eb61da592268dbcdc.png)

Trying these credentials has logged us in!

After a successful login, we can see the flag in the source code.

![9448452078f34f9fb63671d266fdb1ef.png](images/5869ee3fa34a41f992245c4912150aec.png)