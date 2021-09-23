# Cross-site Scripting

Understand how cross-site scripting occurs and how to exploit it.

This room consists of a series of challenges around different types of XSS.

To get started I need to setup an account on the target host.

![f99ddf14937dd7d2431bbb6fd785544c.png](images/eccc38147ea44f1887620cbaa54c2b1a.png)

I'll use my nickname Robbie and begin with Reflected XSS.

## Reflected XSS

We have a search field here that displays our search text above the field after entering it.

![29058c4e3b38f94b1c2f144e4f35d75a.png](images/bf9810f87d21436e8cf717deaada64bd.png)

Now to try a basic payload to test for XSS.

![da58077274a10a10e95b5f31057d957d.png](images/4b95730b82624a048a0b89a2c5066842.png)

this worked!

![56e5407a2983eff3158d76f087352133.png](images/7ce9b6e157644524b060717c21b166d4.png)

Now we need to get the host IP, a quick google advises that Javascript can request the IP with location.host, so we'll try to fit that in our alert box.

![afcac3e2c4fd7bf83e780b1afb9c6dd6.png](images/ab8c780af3cf4782be6045d45fdee85f.png)

And here we have it!

![3e8683122e81cea2e8f7051b6611e398.png](images/ca28850faebb49afbdf41428b31feac1.png)

## Stored XSS

Here we have a comments section on a site we can enter text into.

![f8440779ef2cb6001b68e6acd12185f3.png](images/9a55733ada2f429bab50facaaca0dff7.png)

I'll try some HTML code

![f2caffad1cf5a410b7127e6d3e9e7645.png](images/4d051c7acfe54b918453917cfcae1687.png)

And this worked!

![81366c7d3379e62322a723c853f3de12.png](images/eb906c0e4da54921bdea366b53052383.png)

Now I'll try and get the document.cookie on our account with an alert box.

![b5084fdbd5061c61e067d0f2b2b22e65.png](images/f195466c6ba2475983c9f0a455eb72c5.png)

Here we are!

![b6324c611225043ff42ccea34e3eaded.png](images/157f00d2137a481ca5ac1afc6fcbcffd.png)

Now we have to try to change the title in the page with Javascript. We're changing the heading "XSS Playground" to "I am a hacker".

Looking at the source we can see the id of the title here

![02df6349ed31c12a3897a68a74c4aa97.png](images/491cd12dd03f4debb7fc122375e82034.png)

Here is the payload I'll use to make the change.

![5a59ca2e085bc8732e2f7c1cb636d766.png](images/539d209a68a74b72908a8b605880ab2b.png)

Success!

![6c12c8c0dc952afc1ea855c7615e7c25.png](images/b1efd9be7ba94ad7955ab7b3e9e85023.png)

Now for the final stored XSS task, we're going to hijack jack's account and post a comment as him.

This site has a built-in capture page called logs so I can use that instead of setting up my own web-server for this task.

![3faac4925f07df5ce9a678d621c09589.png](images/1db427050c3f4628904f9bc78c6cb883.png)

Then we'll use this payload to send a get request to my server each time a user loads the site.

![cf875133c3a13820566c3acc400f8127.png](images/15e8aa7e963f49b7952d5c38f2d1cf10.png)

Heading over to our logs page we can see a cookie there!

![4472d31d3f2dbdc88469f71b2512de47.png](images/35b5d73829174c508209427e84f950b5.png)

I'll change my cookie using the developer tools

![85f5a2cff63c5ff914a19f47c736bd09.png](images/db526ff9127a458ebaa31df3003bcca0.png)

Now after refreshing the page, I'm Jack!

![10796f7ec3b5b707ba353082567ad83f.png](images/1464e63c733f4bf394ce4520b7f26a5b.png)

Now I can post as him!

![df058dc38ff7101e1acaa7474ac857d2.png](images/365ee04feb4e49af8fd902b5d0c0f558.png)

## DOM Based XSS

We need to find where we could insert a javascript alert on this page, which displays our cookie.

![17ca6bfdce247b8f78115d42a87689ed.png](images/6bfe0a67096c4859a4bd288a5743db89.png)

We can see the vulnerable code here:

![27d7e31e33d4b2d42b40660d38ec2659.png](images/0c5da8f9d7bb49e0882f7a3533cbb6f6.png)

It looks like we could insert some script code directly into the img-url field.

I'll try to use standard injection and comment out the reset of the line so I can add my own onerror code.

x'" onerror=javascript:alert(document.cookie) alt="Image not found..." width-400>' //

That seemed to work

![086c8e96b9bfab6cf2a9b257556e498e.png](images/5a8ff3205bd644728753bd12c6106e2d.png)

The challenge wanted us to use mouse over, so I used this payload to receive the same results but when hovering over the image area.

x" onmouseover="alert(document.cookie)"

Next we need to change the background to red. A quick google and we get the following code to change the background colour, I'll place the code in the same payload as above.

x" onmouseover="const body = document.body;body.style.background = 'red';"

![94c4e63556c1301c46b05a0088194f87.png](images/8a4200886e7c497ba24599b20c24bc41.png)

## Filter Bypass

Here is a set of challenges to bypass code filters used in attempt to remove code from input fields.

Challenge 1: Are you able to bypass the filter that removes any script tags.

As expected our 'script' word was removed.

![56eb74c316fc95807538ee66385efe5f.png](images/ef6349ec4c0c4d8389ff8bd552446bf8.png)

Using the img tag worked ok, here is the payload: &lt;img src=1 href=1 onerror='alert("Hello");'&gt;&lt;/img&gt;

![ee9172467cfe2f0ac4c8801c1d634f46.png](images/6c02e6162c75492189c7e996d00b2185.png)

Challenge 2: The word alert is filtered too.

This time we'll use the confirm feature instead of alert with the payload: &lt;img src=1 href=1 onerror='confirm("Hello");'&gt;&lt;/img&gt;

This did the trick!

![69a03bd9232c0a0139a8fb1617561d1f.png](images/a3fa85c8505e4d10bd32c919bafb7474.png)

Challenge 3: The word Hello is filtered.

This time I'll write the word Hello nested so it filters out the middle part, leaving the rest remaining. E.g. HeHellollo.

So our payload will be: &lt;img src=1 href=1 onerror='alert("HeHellollo");'&gt;&lt;/img&gt;

That did the trick!

![ea17140da5264d2915d7fb970dce22cd.png](images/28fc1aac6fb245918b2df8d03a8ab01b.png)

Challenge 4: Filtered is the following:

word "Hello" script onerror onsubmit onload onmouseover onfocus onmouseout onkeypress onchange

I did some research to find other events I might be able to use. This link was useful: https://www.w3schools.com/tags/ref_eventattributes.asp

I used the 'onclick' event with this payload: &lt;img src=1 href=1 onclick='alert("HeHellollo");'&gt;&lt;/img&gt;

And clicking on where the image should be worked ;)

![86c360fe0dbea32c79e822f28e55af82.png](images/7fd18b086b2641c8aae8de0d6ee547dc.png)

The lesson had some other interesting information regarding Javascript port scanners and keyloggers, which could be very dangerous and this all just demonstrates how important it is to control XSS on your website.

That's all for this lesson!