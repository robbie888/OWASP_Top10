Level 1 Challenges

# Level 1 Challenges

The challenges below are listed in alphabetical order, not necessarily the order they were completed.

## Bonus Payload

### Description

Use the bonus payload `<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>` in the DOM XSS challenge.

### Exploitation

Having already completed the DOM injection below, this was quite straight forward. The search bar in the web-site is vulnerable.

When searching for a product, the search is reflected in the results as below

![30d567dc807cea50b715570ad91a1fb6.png](images/ed63593d4db2471aaffeaa008c90d141.png)

Here I've pasted the above code in the search field, we can see the iframe has loaded!

![0c1c3cead8bdb9662d87c200310e0310.png](images/76e4fe5df348488880fa7eb41ff0d50a.png)

## Bully Chatbot

### Description

Receive a coupon code from the support chatbot.

### Exploitation

This was quite an easy challenge, I just complained to the chatbot a few times saying I wanted a refund or was unhappy with my order and got a coupon! It seems that the squeaky wheel gets the oil in this case.

![75e54aa89f5898b424f839425997a35c.png](images/3ecf3c53b0864b5c8c80379c11d25de6.png)

## Confidential Document

### Description

Access a confidential document.

### Exploitation

This was actually found by my crawler / spider. The confidential document is viewable from the path &lt;url&gt;/ftp/

But this could also be found in the robots.txt

![28136a39d1a1edc57f489784c1ce4a71.png](images/e4a1d34854b547bba4c128a7232c5ac7.png)

Browsing to the FTP site gives us access to some confidential looking documents, in particular 'acquisitions.md'

![c746bc74bf1fb4b1af07f102ab87b492.png](images/7129064338a24f25be52582b683dc9e7.png)

## DOM XSS

### Description

Perform a DOM XSS attack with ``<iframe src="javascript:alert(`xss`)">``

### Exploitation

To exploit this I looked around for user input fields. The first I tried was on a product review, but this didn't work, it just displayed the code:

![22fa5ae3fb3f27ee86edbbaa018dcc87.png](images/df48fe8488e640bda025603199893ffa.png)

The next place I tried was the search bar.

When searching for a product, the search is reflected in the results as below

![30d567dc807cea50b715570ad91a1fb6.png](images/ed63593d4db2471aaffeaa008c90d141.png)

Here I've pasted the above code in the search field, we can see the alert popped up in my browser and the iframe loaded in the background; proving that it is vulnerable to XXS.

![6f917e2f57b571a46f13d76de19e15f8.png](images/20818a09835d4753b38fba2e3961474a.png)

## Error Handling

### Description

Provoke an error that is neither very gracefully nor consistently handled.

### Exploitation

Here I was looking for a server side error, so my first thought is SQL injection.

So I tried putting in a single quote in several fields starting, with the search field which didn't provide any errors.

However the login page seemed susceptible, putting a single quote in the username field generated this error:

![b7b3472b05b4db128fcee7b599cf7995.png](images/b0eee97cfc6742e1b68387cf62621deb.png)

This solved the challenged.

The error doesn't look good (for the developer), so I went to Burp to see what it had intercepted as the server response.

![5a6b4d03747ec392c60531461ceed953.png](images/37cbc649dae7488ca03b7d4736ff7f54.png)

Here we can see some SQL code, that could easily be exploited further. More on that later ;)

## Exposed Metrics

### Description

Find the endpoint that serves usage data to be scraped by a [popular monitoring system](https://github.com/prometheus/prometheus).

### Exploitation

We note that the link provided points to Prometheus. The Prometheus documentation mentions some folder paths that can be used to access the content. Such as /metrics and /graphs.

The /metrics path yielded some results for us that completed this challenge.

![1ff022f6f3ba8ced630c02bcef31fae7.png](images/a7230a4f1b9c4f4a887e08f8dc9d07f9.png)

## Missing Encoding

### Description

Retrieve the photo of Bjoern's cat in "melee combat-mode".

### Exploitation

Looking at the photo wall there is one missing picture with the symbol of a cat.

![ce13db24d82c7116ac1054da491ec6fa.png](images/0fb360365b4140d086fb2a03df97563b.png)

This one had me for a minute, until I re-read the heading 'missing encoding'.

Now going to the element inspector we can see there are hashes # in the uri.

![995155df3f0bcea0561684103b7f4687.png](images/4f5b1ddbde794ece827f923d4170186a.png)

Looking up the HTML encoding values for these which is %23 then replacing that in the link did the trick!

![928c5ccddcd10314e42f7fa194b0a031.png](images/768dd3f6822c416eab8bd9cfc022ce79.png)

Meeow!

![5a72e1fc3ce480cf050acef7ea4ab64e.png](images/0f90d67db13d45b389f1ff3c950a3eff.png)

## Outdated Allowlist

### Description

Let us redirect you to one of our crypto currency addresses which are not promoted any longer.

### Exploitation

In this one I searched through the JS code using the Firefox inspect element under the main-es2018.js

I searched for 'redirect' and 20 results showed up.

![ff6d9f58b6c2dd82e12cc21cd720eb34.png](images/cababa7336aa4d4ca48ab230ea027772.png)

Going through these didn't take long to find a redirect referencing a bitcoin wallet.

![fdd5ef58e3d71731ca32d296038c9a1e.png](images/c6277c61a43e4238b71493463c423f10.png)

Navigating to this redirect url completed the challenge.

## Privacy Policy

### Description

Read our privacy policy.

### Exploitation

This wasn't really an exploit, just ensuring that we are manually clicking around the site to check it out.

To get to this I created an account and logged in.

![6d0871a02284ef07131eb143544ae295.png](images/9ca5581e66d74bf291b0ccbb6742829a.png)

Clicking on the Privacy Policy solved this challenge.

## Repetitive Registration

### Description

Follow the DRY principle while registering a user.

### Exploitation

The DRY principle refers to 'don't repeat yourself'.

This exercise shows a coding error in the user sign-up page.

Once filling out the registration page, including both the password fields (initial and repeat) you can go back and change the initial password and it doesn't re-check if the 'repeat' password matches.

Here we can see I can submit the form with different length passwords.

![50b13b3bf96d4f5c4bf4ceb93e8e59ad.png](images/8bc0922303944415be9aa62418511461.png)

The user will then be created using the first password field, ignoring the second field entirely.

## Score Board

### Description

Find the carefully hidden 'Score Board' page.

### Exploitation

This was the first challenge I completed, when logging into a new Juice Shop instance it provides us with some notifications suggesting we look around for the score board.

My first instinct was to use type it in the url /scoreboard. This didn't work I then tried it hyphenated /score-board and got to it.

However it can also be found in the site code through the debugger looking at the main.js, by searching for 'score':

![04306c28f5946b21ee5c21a061a6774c.png](images/8107a563a99945bd9dbb64c5bbc3fa5f.png)

## Zero Stars

### Description

Give a devastating zero-star feedback to the store.

### Exploitation

There were two paths to this one, my first instinct was to use Burp Suite and intercept the post, editing the post data. So I'll go through that first.

Here we can see that I cannot submit the form until i give a rating, it must be higher that 0!

![ab353435311b5ed6d67bd5c4f3dbdb17.png](images/0b54198167244835aff62bcf7b26f68f.png)

So I gave a rating of one and submitted the form with my Burp Suite Intercept enabled.

Now I'll change the rating to 0 in Burp and forward the post.

![01ed285bb9170cd3b084f181b91384e9.png](images/7b252bad589b46a4a0f2cb06dbacdef6.png)

That solved the challenge!

The second way is to edit the javascript in the debugger.

If I remove the 'disabled=true' option replace it with enabled=""  then the button becomes enabled and allows me to submit with 0 stars!

![f69c3302e263166623ee63931f06a984.png](images/a072dacd071447a4a2c0a2dbf266de61.png)

Now the button is enabled and I can send in the form:

![cc9684f71c672984e68a2a09b7f6ac8a.png](images/1682b92b92974759b77d9eb0f29c344e.png)

That's it for this level.

Thanks for reading! :)