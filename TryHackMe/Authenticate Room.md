# Authenticate Room

Learn how to attack authentication mechanisms used in web applications.

## Dictionary Attack

Here we will use OWASP ZAP to perform a dictionary attack on a web portal. The process is similar in Burp, Burp is just slower due to throttling.

We have a username and password field on the target host.

![7d84f66b28e43cab4a0d7abd6110fe4a.png](images/537b006c2ee04720923cbd460e1f946f.png)

I'll turn on the proxy intercept and capture a login attempt.

As you can see above I've captured a login attempt and sent it over to Fuzzer. Now I'll load a dictionary file and start the attack.

Note we have been given the username 'jack' in the room, so we'll just attack the password field.

![d17dacf29cb016ad4193d331e583d012.png](images/53d0b2bd224a419cb4882145727c3aa3.png)

I've loaded our rockyou dictionary as the payloads.

![fd59b859e37c09625fa9927974818215.png](images/f72474509f7642039326ac83f26de37b.png)

After a short while, we look for differences in the response sizes.

Here we can see a unique one, which would be our password!

![f915a50c9e276503a46a3d2e58aee800.png](images/8d5cdaf03df94d4fb07b790b5c04dac7.png)

And here we have our flag.

![bd8194de620dbc21ebb0d4627638bfe4.png](images/04c51cd042194babbbff4833b65bb2aa.png)

Now we've been told to do the same thing with the username 'mike'.

Here are the results

![c6e21b274e889770dd4c1ec07d7c7539.png](images/2d0d670629e24492b237aa01e5fdaf1d.png)

And our flag

![5557834250dd6ded03c5962fee094bf9.png](images/ec9c71e5ed634c90abd0b1931cdc035a.png)

## Re-registration

Here we will re-register an account 'darren'.

When trying to register darren we can see the account already exists

![8fe3dfe09cc76c6cba74f28c622f53a5.png](images/5534ad9938c84743855e1538334ff4be.png)

So now we will register ' darren', noting the space at the start.

![2d9a8cff9ac063ec5f9cad4b866e825c.png](images/cf6906e2051d469c8b4af90f7a9faaa3.png)

This has worked!

Logging in with our new user, has shown us darren's account information i.e. the flag!

![6f50d3e0b65d045ee5e68488f4a59b44.png](images/143c49436fee441b99165a26ef47a797.png)

Now we do the same thing with the account 'arthur', here is the flag.

![695f4b626157742fd03a5da50685c886.png](images/70460ed539ce40e9991c7f73a3592562.png)

## JSON Web Token

Here we will exploit an incorrectly configured JSON Web Token authentication.

We capture the JWT

![2a031d34029cd0260139f3d1891e4bb1.png](images/62d8251af1644944b487759defa8fe0c.png)

And send it to the decoder, decoding it from BASE64

![fc31f3f7b65b2772dfb1d29412f9de2d.png](images/355a76729c4d48e1ae048e7a8900c5a4.png)

Now we change the encryption method to none and the user ID and re-encode it

![c19478357eab92ddcbc173643dcdb641.png](images/4afab8f434714c8da97f108f9c2cc014.png)

We enter the encoded values into the Authorisation JWT token section of the request, and we bypass the security!

![665a287d9df7e87ab7970755a1b84789.png](images/cbe407518a794d349adaa233394a4a26.png)

## No Auth

Here we will look at access control issues where we can get access without any authentication necessary.

In this example, when we login we can see a user ID in the URL path.

![1a5025a953ac9c8532ae58347fecb7da.png](images/6a9f7d00a2004aa58e80fa79e8c7f6cc.png)

Changing this ID number allows us to access other user account's information.

![43d18824f2c23e2521134c21e7fb3c1c.png](images/54ba893c4dde4ed8bb3ae7e096977101.png)

Trying some different user ID accounts we discover the superadmin account and the flag!

![39d630201c5e802ae0e09cdb99925e1e.png](images/7e44ae89770f4d5eaa769b7dc665297e.png)

That's it for this lesson!