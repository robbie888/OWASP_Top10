# Level 4 Challenges

The challenges below are not listed in any particular order. This list currently only covers some of the level 4 challenges.

## Christmas Special

### Description:

Order the Christmas special offer of 2014.

### Exploit:

Using the SQL injection flaw we identified previously on the search bar.

I'll first get a search captured in Burp and send it over to repeater, then generate our SQL error with an injection of test'

![61a180dd3a963ca2b9d99b7815b68d59.png](images/fbe680cd156948eba86504818bf2e8c5.png)

Here is the relevant part of our query, it checks for 'deletedAt', meaning that an item which is deleted is likely still in the DB just flagged as deleted.

![9af030ec3e5cf74c46d56b4197555a08.png](images/0f5e6fe23da14b218b6ab4a5b500a112.png)

So if I just comment out the rest of the query we should be able to get a list of all items in the search results, deleted or not.

Here is my query and we can see deleted items are showing.

![d91ab915324446d383b8cffe09d92b58.png](images/f1660e5ca0c54036ae5e1e3c80104344.png)

Here we can see the item ID of the Christmas special

![298c25ddc2e42d8d02aeef5b7770f39d.png](images/fa8ddf46a76041f594050cd9dce8d4d8.png)

I'll try add it to my basket using Burp to intercept a query to add an item to the basket.

Here we can see a productID in the request

![adb6972cdd0835df613740518797ba11.png](images/db1dfb8f35754bd0a3ddad1c8c205707.png)

I'll send this to repeater and change it to 10.

And here we have it, the item is added to my basket.

![03b669fd7ad9101a4176ef3f57421ac8.png](images/c7d714ba29504632ab804266f9c19cce.png)

Completing the order process completed this challenge.

## User Credentials

### Description:

Retrieve a list of all user credentials via SQL Injection.

### Exploit:

Referring back to level 3 where we got the DB schema, we're going to use the same process to dump the 'Users' table which we identified in that challenge.

Referring to the sql_master sql entries which define how the tables were created, we can see all the names of the columns in the table. Below is some of the output for the users table. Scrolling across we can see all the column names.

![33a6d9560a9b423fb64b6278f9f75a69.png](images/168e66f1aa5549babc310971bd99c612.png)

Here is the query we will use, getting the information from the table details above, and using the 9 columns in the UNION which we discovered in our previous challenge.

`test'))%20UNION%20SELECT%20id,username,email,password,role,deluxeToken,totpSecret,isActive,'7'%20FROM%20Users--`

Injecting this into the search field 'q' we get the following:

We can see lots of information here:

![cd9662693f3168484fdef10540f2b10f.png](images/1e11f2fde357415e80e2bc87531ecc1f.png)

We also know that they're using MD5 hashes, as per the previous challenge in level 2. So we could easily decode all these passwords.

Running the query above completed this challenge.

## Forgotten Developers Backup

### Description:

Access a developer's forgotten backup file.

### Exploit:

Heading over to the FTP site we discovered earlier

![52ef8b797418538bfbb22ca3385bb611.png](images/badacc81b66c40628277e81812605cd5.png)

We can see some backups here:

![b47d827dbd11e451dbd2ee1342f2bb2c.png](images/08312a7e61a2491ea14ed4c75dabff57.png)

Attempting to download them directly yields an error - only md or pdf files allowed.

If we insert the 'poison null byte' into the URL with a suffix of .md we get the download prompt

![07696d90f1b040c26916d4ff39f31485.png](images/563ebceaabf94756b2536b551562d47f.png)

![9d7f62f671a0477ae8ca5fa1154fb5fe.png](images/3b9173d937924d158b01a9c01c8e090d.png)

That solves the challenge! This actually solves another challenge in the list called 'Poison Null Byte'.

## Hidden Easter Egg & Nested Easter Egg

I'll combine these two challenges into one.

### Description:

Find the hidden easter egg.

Apply some advanced cryptanalysis to find *the real* easter egg.

### Exploit:

First we download the eastere.gg file from the FTP site just like the last challenge (see above).

Opening the file we get this:

![5fe8b14ce653a2f3b49276662cedc7b2.png](images/6dd5a581563c4812bac05a4296218cb5.png)

So we've located the hidden easter egg, now to find out the nested one.

This looks like BASE64 encoding, I'll use Burp to decode it.

After BASE64 decoding we get something that looks like a URL / Path

![685f1f8eb8cefa0b4dc999ed14674a40.png](images/7f353d93440b4e8c9803f476ba254526.png)

It turns out this is ROT13, using an online decoder we get the following:

![bd07c8d6c3a593665a0725430ae9e9df.png](images/ea99de64e2294946b9c9912c4d706c0d.png)

Now to browse to that url path:

![174e13264090e774b7657547b4fca946.png](images/0455beaaa78f43aba4b6ef4f98c67ad8.png)

And out Easter egg is the earth. Browsing here completed the challenge.

### Note:

That's it for now. That concludes the Beginners Web Application Hacking course.

There are more challenges in this category so I'll come back to those at a later date. My priority at the moment is finishing the other training material I've outlined in this repo first.