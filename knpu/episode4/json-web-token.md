# JSON Web Token

Think about how you normally authenticate on the web. Well, usually after we send our user name and password, a cookie is sent back to us and then on every request that cookie is sent to the server and that cookie identifies who we are. So in a traditional application, the cookie on the server side has reference to our user ID so that the application can look up our user ID in the database and get information about us, but more generally authentication is usually done by sending a token. 

In an API, this is no different. Ultimately our clients will obtain a token and when they send us that token, the token is going to tell us two possible things. It might tell us who the user is. So I send a token. It identifies that I am user ID five or it may contain information about what I can do. So that token might say that I have certain scopes. This is how oh off works. The token that you get an oh off when you send it to the server, the server knows that this token is allowed to do certain things, but not allowed to do other things. Traditionally, tokens work like this. 

The client sends the token and then on the server, all of the tokens are stored in a database somewhere along with the information attached to the token, like the user ID or the scopes, the permissions, the things that that token is allowed to do, but what if we could create a simpler system? What if when we built our API, when the user sent the token, they could actually send us just their user ID, like 123? And then all we would need to do on our application is read that 123 and boom, we would know who was authenticated. 

Now, of course, we can’t do that, right? Because then anyone could just send any user ID and suddenly take over someone’s account and there wouldn’t be security. Right? Actually, wrong. Go to jwt.io, the main website for JSON web tokens which let you do exactly what I just said. Basically a JSON web token is nothing more than a big – a JSON string that is then signed so that you can send it back and forth from the client to server. It allows you to store information in it like my name is John Doe and securely exchange that between your client and your server. How does that work? 

Well, the key behind JSON web tokens is that yes, these JSON web tokens are encoded, but anyone can read them. So they’re not secret. You would never put something secret inside of a JSON web token, but anyone can read it, but no one can modify it. So if I give you a token, JSON web token, that says your user ID is 123, anyone else can steal that token and use it to become you, but nobody can take that token and change the user ID to something else and that is what makes it secure and when you use JSON web tokens, it makes implementing authentication on your server much easier and a lot more flexible. So let’s do it. 