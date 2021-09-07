SQL Injection Labs

# SQL Injection Labs

Below is a list of the labs on the Portswigger Web Academy for the SQL Injection labs.

## Lab 1

**Description:** SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. This was a simple SQL injection to get more data from the DB than intended. Adding `‘OR 1=1 --` to the query provided additional results.

![](images/5fed2250c14545d6b89d6f37aa9cdb78.png)

## Lab 2

**Description:** SQL injection vulnerability allowing login bypass

There is a login page, with a username and password.

![](images/9c088fa4802f4982b4da3353602980fb.png)

It seems to be using a post method, after attempting a login it doesn’t show the username / password in the address bar, so I’ll capture it with Burp and try and injection attack that way.

I’ll turn on Burp Intercept and edit the post. I’ve changed the username to administrator’-- in attempt to bypass the password check.

![](images/c372a2b5f70147c1a103540422915f08.png)

That got me in

![](images/4d36ab9ad27641b8a2334a9bb636eb01.png)

## Lab 3

**Description**: SQL injection UNION attack, determining the number of columns returned by the query

To solve this I inserted the following into the URL bar ' UNION SELECT NULL,NULL,NULL--

doing more than 3 NULL’s produced an error, therefore there were 3 columns.

![](images/143827c8cc4a40d99185ef90daaed633.png)

## Lab 4

**Description**: SQL injection UNION attack, finding a column containing text. Make the database retrieve the string: 'fgAEo0'

First I’ll attempt to determine the number of columns.

Using ORDER BY, I was able to determine that there are 3 columns of data returned in this query.

![](images/f8092b5a3bb744249c9117109d88ded6.png)

Trying the same attack with 4 produced an error.

No I will try and determine which columns can contain strings.

Using a UNION I was able to determine that the second column contains strings.

![](images/f9234c6831d44ed6b79652ab644150fd.png)

Here we can see I have inserted the letter ‘a’ into the results.

![](images/bb95e508560d487fb382f53e5e5ef8f8.png)

So here I have injected the string as requested by the lab.

![](images/f6e0ea3b919041b28ed3f1456cbc7a6f.png)

## Lab 5

**Description**: SQL injection UNION attack, retrieving data from other tables. The database contains a different table called users, with columns called username and password. To solve the lab, perform an SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

First I’ll work the number of columns and which columns can contain strings, using ORDER BY and UNION SELECT ‘a’, NULL style attacks.

Using ODER BY 2-- has worked, but anything above this produced an error. So we have two coloumns.

![](images/4ffe20b96e9f461eadd6467e6628eb3e.png)

Not to test for string fields in the results, based off the content I’d suggested they’re both strings, but will run the exercise.

Here we have it, both columns are strings, as can be seen by the inject A and B below.

![](images/b2fe2dcd58f2453bb2e463627f5dd133.png)

Not for the interesting information from the ‘users’ database.

Here is the query

![](images/d67cfc89bbe84f33b1377d65b956454f.png)

And we have some nice results in our content

![](images/2a08c77225a94830b76e7cea39ee8633.png)

And the administrator’s account was further down:

![](images/f68f149d11c240b092785765d275a920.png)

Now to login with those credentials to finish the lab.

I’m in!

![](images/4c6ed9ef3fe74a8b9753a17ee25c9cbd.png)

## Lab 6

**Description**: SQL injection UNION attack, retrieving multiple values in a single column. The database contains a different table called users, with columns called username and password. To solve the lab, perform an SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

First I have determined that there are 2 fields and the second field contains strings using a UNION SELECT NULL attack as follows:

![](images/8db344483c5a485592244d649b344aa6.png)

Now that I know the second field is a string I’ll try and pull the username / password combo into that field.

Here is the URL I used to join the two fields into one, and we now have the login credentials in our website output.

![](images/e20a4f70da704ec0ac47d19f0c358026.png) 

![](images/06c93ea0c67246b6b19521d7bcba8319.png)

Now to login as the administrator to complete the lab.

![](images/a424802878bd4295b64da697e4db903a.png)

## Lab 7

**Description**: SQL injection attack, querying the database type and version on Oracle

First I’ll determine the number of columns, then determine which are strings.

With this query I was able to see that there are two string fields I can use.

![](images/a65ec2b233d64737b460eac790a1309b.png)

Now to get the version name

Using the SQL injection cheat sheet on Portswigger I put together the following querying

![](images/e7ce9acfe1d641a6a39e4ed49aaa954d.png)

This printed out the version information into one of the text fields on the webpage.

![](images/c0116bbaea5c4de9b5755939ea453aa7.png)

Lab solved

![](images/a43f71c3718b47ab8c6c0e828454ee03.png)

## Lab 8

**Description**: SQL injection attack, querying the database type and version on MySQL and Microsoft

First I determine the number of columns. I had to add a space after the double dash at the end to make this query work. So at this stage I’m assuming it’s MySQL.

![](images/ca6a0d805c894254ae2a844a54770d09.png)

Now the column types

![](images/bfd785eb9e444d75a7cb8aa427db50c4.png)

We can see that both columns are text.

Now I will query the version, attempting a MySQL query first.

Here is the query

![](images/ae7fe5fddcce41ad926a07f2b66dcd93.png)

And we can see the version results here:

![](images/a19d6987afee40059d5dba6084f2ff5e.png)

And the lab is done

![](images/037c04c092e742759782273f972c1661.png)

## Lab 9

**Description**: SQL injection attack, listing the database contents on non-Oracle databases.

This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables. The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users. To solve the lab, log in as the administrator user.

Looks like we have two columns of text type.

![](images/a89829dfba864780be82818d5ac21e8d.png)

So now I’ll try and get some database names…

I used this query:

![](images/9d02e32ae5604d7eb856140c69a7c860.png)

And got a huge list, now I’ll start looking through for a ‘users’ type database and query the table’s columns.

![](images/38801bf7e7ff4fa797b2f7f7872fe117.png)

Found one called users_xagzie which might be interesting.

![](images/08c1586a62ef4e4aa54ef7fb9b33cdf6.png)

Now to query this DB’s columns.

This is the query I used, and got some hits in the results.

![](images/7bd9767f86bd423e9d6a0a2e6d2d64a7.png)

The column names are:

username_ujsjvf

password_vxcwmu

Now to query the database to spit out the contents we’re after. Usernames and passwords.

And here we have it, they query:

![](images/0a38e8d626224227818896faff58b03f.png)
And some interesting results:

![](images/f39a2dc1041a4a7598ad49ec6901bc9b.png)

Now to login and finish the lab

![](images/9675189c2f194577a1a87603050b8a62.png)

## Lab 10

**Description**: This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables. The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users. To solve the lab, log in as the administrator user.

First I’ll work out the column numbers are data types

Seems there are two columns:

![](images/5ecdfc64cf0749469e1e1feb61ec079e.png)

Both are text fields.

![](images/05f69c5618a64bac8b3b2a40a17b08d6.png)

Now to get the database names, this query got me the database names.

![](images/327412d1630c4663b9e3a6bf8980ae0e.png)

This table name looks interesting

![](images/4604c0f2d01f49f88a40c28be798fe85.png)

Now to get the column names:

![](images/f9b71c939b0d470fb8d95be91c26c796.png)

Here are the column names we’ll use to get the credentials out.

![](images/57c23427163b43c392ba73bc8ac1de16.png)

PASSWORD_UFEUSH

USERNAME_ALKLZI

And now we have the credentials, here is the query:

![](images/12b6679bb1ce4a85b01e32689329435f.png)

And the data:

![](images/68eb56f70a9240ac895dafcb8bf6fc85.png)

And the lab is solved:

![](images/3e1b34dbae694f6790511f0b29aea4dd.png)

## Lab 11 (Blind Injection)

Description: This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs an SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

To start off, we can test our injection with the ‘welcome back’ message here using the Repeater function in Burp.

![](images/7ba06ded289745959dee29a368c0a8c2.png)

Now if we inject ‘ AND ‘1’=’2 into the cookie field when we capture this in Burp we can see this message is removed, so it is vulnerable.

![](images/b9b55bb9f9574ed3b3fb5909d8546c79.png)

This change in the site indicates our injection worked.

Now I’ll move on to the next step, we’ve been told a database name and account to try and get the password out one letter at a time.

First I need to see if it’s Orcal or not, so will start using the Oracl based SUBSTR query and see if we can get a positive response.

After some trial and error, I managed to get a successful injection advising us that the first letter of the password is less that z. Now I can move this request into the intruder and try to brute force each char in the password, one at a time.

I used the SUBSTR command indicating that it is likely an oracl db.

' AND SUBSTR((SELECT password FROM users WHERE username = 'administrator'), 1, 1) < 'z

![](images/3be38c8e0f6049208db94aa3e72860ee.png)

I’ll setup the intruder function with a list of the alphabet and change the char each round.

![](images/33b6559fb50f480a8c196ad9d483ef33.png)

A quick python script to print the range of chars I’ll use in the attack. I’ll start with letters.

![](images/ba0e1b320ad143e6bb8c88e462b490e5.png)

Turns out I don’t need this, Burp has a brute forcer option that I’ll use!

I’ll start with lower case

![](images/1909b0bcacb947378be1bfb5808e809c.png)

So far so good, we have the first char. ‘b’

![](images/8df57899921948f68b6057f72b2f99d3.png)

Now for the rest, I’ll update the SUBSTR request and use the ClusterBomb attack tool to traverse through the password positions and chars. This will take some time since Burp Community is throttled. If it takes too long I might jump over to OWASP ZAP.

It’s getting therem but taking it’s time.

![](images/6bf948fc0531428b94c9af88f1c653ff.png)

While that’s happening I’ll setup ZAP and see if I can beat this attack with another.

I noticed the attack is missing a few positions, that must be uppercase letters or special chars which I’ll try and get on a second attack.

So OWASP Zap was much much faster, I got 15 chars so far, Burp is only a third of the way through due to the throttling.

![](images/c4663013a9e040149fa1fe78e5c4fa39.png)

The password was long so I did a second batch to test positions 15-30 in ZAP.

Here are the results.

![](images/c02ebf40c13f4350ac5eb5ac7d270893.png)

So the password is: bd6hlz8l16gqrcwa3os1

And that did it

![](images/b833644d721a4ce4ad2fabd2f01b1536.png)

I’m in.

## Lab 12: Blind injection

**Description**: This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs an SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

First I’ll try and inject a conditional error based code and determine which database i.e. which conditional statement I need to use.

So I got a little stuck and didn’t think of using the double pipe to concatenate the queries. That was a good lesson.

After know that I constructed the following to test if I could get an error.

![](images/a799c6a425c0403784a26dfabf7ec368.png)

After trying the different conditional error methods on the SQL injection cheat sheet I had the above begin to work. Where having the 1=1 case provided an error and having 1=2 displayed the normal page. So I can infer that this is a Oracl DB.

Now with this I can amend the condition to check for the administrator’s password in the users database as in the last lab.

I’ll use ZAP again just because the fuzzer is quicker that the community burp.

Now I have a query that is working with the username/password lookup.

![](images/f8dba03a89184395a6b3ee4b3512d952.png)

I’ll adjust it to error if the letter in the password position is NOT correct.

This will be my payload.

'||(SELECT CASE WHEN (SUBSTR((SELECT password FROM users WHERE username = 'administrator'), 1, 1) != 'a') THEN to_char(1/0) ELSE NULL END FROM dual)||'

I’ll adjust the position in the SUBSTR and the char I’m testing for and put this through the fuzzer.

If the page loads correctly in the fuzzer I should have a char from the password.

Here is my fuzz attack.

![](images/ec5453a5dfec4bd691e9806800a9abda.png)

After running the attack for a few mins I have a list of chars and their position in the password.

![](images/cf4d82e52ea94392b8698e738930b13d.png)

The password is: 321sd9cysa3kngnx3wnm

Now to login.

![](images/dd472404a2414039905a6e3c2432afaa.png)

Lab done.

## Lab 13: SQL Blind Injection Time delay

**Description:**

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs an SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

To solve the lab, exploit the SQL injection vulnerability to cause a 10 second delay.

**Notes:**

As with the previous lab, we will attack the tracking cookie field.

![7e64d55137ccd1676297b01eda663ce6.png](images/d9b8b85087a64605b621c8e82cde8750.png)

I'll write a few SQL injection attempts for different DB types and try the all.

After trying a few payloads, this one did the trick and provided me with a 10 second delay before opening the site.

`'||(SELECT pg_sleep(10))||'`

This means it is a postgres DB.

![4e171abdcec025a50f0e0a5442f94ff6.png](images/46c1bc96d0a94a1595f86686f30accd3.png)

## Lab 14: Blind SQL injection with time delays and information retrieval

**Description:**

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs an SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.

**Notes:**
First, using the repeater in Burp, I'll try a few payloads to achieve a time delay and confirm what type of DB is running.

I'll go for Postgres first, since that was the DB in the last Lab.

Success, I had a 10 second delay with the following payload.

`'||(SELECT pg_sleep(10))||'`

Now I'll get a payload with a condition in it. Using the sleep function if true.

![284c755dbe4d4754a75c4d78655ab92e.png](images/71abae397aa447f5b65dda6138ee887c.png)

This condition worked.

Now to try and brute force the password. I'll switch over to ZAP for this one as it will be faster.

I'll try and get a working payload by testing if the first letter in the password is smaller than 'z'.

![022901eee1616e268c22e9162ce0ab91.png](images/f37d0dc39fef4ad9b1f0168b6c0d20f5.png) 

Now I have a payload to test the administrator's password, so will load this into the fuzzer like previous attacks and look for responses that have been delayed.

Here is the payload:

`'||(SELECT CASE WHEN (SUBSTR((SELECT password FROM users WHERE username = 'administrator'), 1, 1) = 'a') THEN (SELECT pg_sleep(10)) END)||'`

And here we have it, the password in my payload results. I sorted the results by response time, but will now export it to excel and sort by payload to make it easier to write the password down.

![6b9ef79dfce50923b4330afca4ca272a.png](images/2b291eb8e08b4c66ac7e9df0babefb3a.png)

And that is the lab done.

![image](images/31909f62991c4ef5a9d75996a9475bd6.png)

I'll have to stop here, the next two SQL Injection labs require Burp Pro.