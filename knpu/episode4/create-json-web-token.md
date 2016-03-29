# Create JSON Web Token

The first thing we need to do inside a TokenController is check and make sure that the username and password that are being sent via http basic authentication are correct.  To do that, we’re going to need the request object, so add that as a type [inaudible] [00:00:21] your controller and then we can search for the user like we always do.  User=$thisgetDoctrine, getRespository, AppBundle: User and then we’ll use findOneBy username.  And then to get the http basic user, there is a shortcut on request called request–>getUser.  That’ll be the string.  And of course, if not user, we will do a throw $this–>createNotFoundException.  Perfect.  And then to check the password, we can just leverage some core Symfony services to do that very easily.  

So create an isValid argument and set that $this–>get, and then get the security.password_encoder service and just call it isPasswordValid on it, pass it the user object, and the raw string, which again is sent via http basic and you can call request get password to get that string.  So now we know whether or not the username or password is correct.  And if it’s not, then throw new BadCredentialsException.  We’re going to talk a lot more about properly handling errors so that we get the exact right JSON response and status code and all that back.  But for now, you can know that bad credentials exception is going to cause an authentication failure.  It’s going to kick them out of the system.  So this is good enough for now.  Okay. We have our user, we know it’s valid.  Now we need to create our JSON web token.  And here’s how you do it.  

We’re going to token variable and set that to $this–>get and remember when we installed the lexik JWT authentication bundle, it gave us a service that’s going to help create JSON web tokens.  Get it out with lexik_jwt_authentication.encoder and then call the method encode on it.  And notice this takes an array.  I want you to pass it an array with the username key set to $user–>getUsername.  This is a really important thing with JSON web tokens.  All they really are is just an array of information.  You can put whatever you want inside of a JSON web token.  You can include roles; you can include user information; you can include a poem; whatever you want.  I’ll show you how we’re going to use this username and then we’ll talk about what else you might want to store inside of your token in other cases.  

But the point is, you’re just putting information into this token.  In our case, information that identifies who is token belongs to.  So, finally, that will be a string.  So let’s return a new JSON response and we need a token property because that’s what our test is looking for, and we’ll set it to that string.  And that, guys, is everything.  Let’s run our test.  And it passes.  And because that was so easy, let’s also make sure that if we send a bad password that we’re going to get back the 401 status code that we want.  So in TokenControllerTest, let’s copy our method.  Paste it, rename it to TestPOSTTokenInvalidCredentials.  We’ll keep everything the same, except we will now make the big lie that my password is IH8Pizza, even though we know it’s I<3Pizza.  

And then check for the 401 status code.  We’ll copy that method name.  We’ll go over and our vendor/bin/phpunit/ --filter, paste that method name and run it.  And it should pass, but it doesn’t.  Interesting.  Look at this.  It doesn’t return the token, but look at the url.  It actually goes to /login.  So, it’s we’re getting kicked out of the controller, but this isn’t Api, but our application instead is treating us like a browser and redirecting us to the /login page.  So, it’s kind of working, but it’s not what we want for our Api. So this is another thing that we’re going to fix.