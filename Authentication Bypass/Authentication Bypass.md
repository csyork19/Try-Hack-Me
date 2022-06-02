# Authentication Bypass

## Username Enumeration
A helpful exercise to complete when trying to find authentication vulnerabilities is creating a list of valid usernames, which we'll use later in other tasks. Website error messages are great resources for collating this information to build our list of valid usernames. 

Command: ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.1.193/customers/signup -mr "username already exists"

*** Insert image of usernames being found ***

## Brute Force
Using the valid_usernames.txt file we generated in the previous task, we can now use this to attempt a brute force attack on the login page. When running this command, make sure the terminal is in the same directory as the valid_usernames.txt file.\

Command: ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.1.193/customers/login -fc 200


## Logic Flaw
Sometimes authentication processes contain logic flaws. A logic flaw is when the typical logical path of an application is either bypassed, circumvented or manipulated by a hacker. Logic flaws can exist in any area of a website, but we're going to concentrate on examples relating to authentication in this instance.

## Cookie Tampering
Examining and editing the cookies set by the web server during your online session can have multiple outcomes, such as unauthenticated access, access to another user's account, or elevated privileges.

The contents of some cookies can be in plain text, and it is obvious what they do. Take, for example, if these were the cookie set after a successful login:\

Set-Cookie: logged_in=true; Max-Age=3600; Path=/\
Set-Cookie: admin=false; Max-Age=3600; Path=/\

We see one cookie (logged_in), which appears to control whether the user is currently logged in or not, and another (admin), which controls whether the visitor has admin privileges. Using this logic, if we were to change the contents of the cookies and make a request we'll be able to change our privileges.\



*** Insert image of not logged in message ***

We can see we are returned a message of: Not Logged In. Now we'll send another request with the logged_in cookie set to true and the admin cookie set to false:\

*** Insert image of being logged in ***

We are given the message: Logged In As A User. Finally, we'll send one last request setting both the logged_in and admin cookie to true:\

*** Insert image of admin being logged in ***


## Encoding

Encoding is similar to hashing in that it creates what would seem to be a random string of text, but in fact, the encoding is reversible. So it begs the question, what is the point in encoding? Encoding allows us to convert binary data into human-readable text that can be easily and safely transmitted over mediums that only support plain text ASCII characters. Common encoding types are base32 which converts binary data to the characters A-Z and 2-7, and base64 which converts using the characters a-z, A-Z, 0-9,+, / and the equals sign for padding.\

Take the below data as an example which is set by the web server upon logging in:\

Set-Cookie: session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/\

This string base64 decoded has the value of {"id":1,"admin": false} we can then encode this back to base64 encoded again but instead setting the admin value to true, which now gives us admin access.
