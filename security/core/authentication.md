---
layout: page
title: Authentication
categories: security
---
## Definition
Authentication is the process by which a user or application identifies itself to another application or system. Authorization is the validation that they are allowed to perform an action on a resource within the system.
 
Passwords, cryptographic tokens, and biometrics are some of the common ways of authentication.

## Factors in Authentication
Factors in authentication are means to provide authentication to a system. They are broadly categorized as follows -

- Something you know i.e. “here is something only you could have known, so it must by you”
  * Password (memorized phrase)
  * PIN (numerical code)
  * Security Questions (“what’s your first pet’s name?”, “where did you go to school?”)
- Something you have i.e. “here is something only you could possess, so it must by you”
  * Mobile phone (automated phone call, SMS, authentication apps)
  * Smart card
  * Hardware Tokens
  * Magic Link (URL with embedded tokens)
- Something you are (with biometrics, accuracy and privacy can be a problem)
  * Fingerprint
  * Facial recognition
  * Retinal/Iris Scan
- Something you do i.e. “this is something you do that is unique to you”
  * Voice recognition
  * Handwriting/signature recognition
  * Typing patterns/speed
- Somewhere you are
  * Geolocation
  * IP based

> Multi-factor Authentication: Using more than one factor for authentication, usually 2FA or 3FA
> 
> 2FA (Two factor authentication): Using two factors for authentication
> 
> 3FA (Three factor authentication): Using two factors for authentication
>
> Passwordless: Using factor(s) other than "something you know"

## Authentication for the Web
Since HTTP is a stateless protocol, web servers need a way to avoid having to re-authenticate a user everytime they make a request. Authentication to servers can be session-based or token-based.

### Session-based Authentication
The server creates a “session” associated with a user and stores it in its database. A unique `sessionID` string corresponding to the session is sent back to the HTTP client on successful authentication. The `sessionID` attached as a header in every subsequent client request so the server can identify the requestor.

Session-based auth is usually done using HTTP Cookies, but can also be done through form fields and URLs. Scalability is a major downside with this method, since sessions need to be replicated and synchronized across multiple servers using something like a shared database, which adds complexity. It is also not an ideal scheme for authenticating APIs since those endpoints are designed for independent requests and don’t maintain sessions.

### Token-based Authentication
After authentication, the server creates a token (cryptographically signed with some “claims”) and sends it back to the client. The client attaches these tokens in their header while making subsequent requests. The server verifies the token’s signature and associated claims to authenticate the user.

Tokens allows the server to avoid storing state associated with a user’s authentication. This allows a token to be passed to a different webserver (or even application) without needing re-authentication. The tokens will usually contain an expiry time and a web server will not accept any token that has passed this time.

## Password Storage
Web applications store passwords in their database. This can be done in a couple of ways.

1. [Terrible] Store in database in plaintext. If the database is compromised, all customer logins are compromised.

2. [Less Bad] Encrypt passwords with a key. Attackers will now need to compromise the database as well as the key.

3. [Better] Store hashes of passwords. Since hashes are one-way functions, even if the database is leaked. However, they make it easy to determine if two users have the same password. Hackers can try to brute-force the password by computing hashes for every possible password.

   Hash = H(Password)

   Hackers use “rainbow tables” which are large tables of pre-computed hashes for all password possibilities. These can grow to several hundred gigabytes, depending on the length of the keys and character space. Lookups in rainbow tables are faster than trying to compute all possible password combinations.

4. [Even Better]
   Store hashes of passwords along with a “salt”, which is a random string appended to the password while generating its hash. Salts should be unique for every user.

   Hash = H(Password + Salt)

   It is okay to store salts with their corresponding hashes in the database, because the primary purpose of a salt is to make rainbow tables ineffective for a hacker and force manual hash computation. For a sufficiently long and random password string, this should take an unfeasibly large amount of time.

   You can even add a random “pepper” along with the salt, which is a constant stored in source code. This way the hacker would need to compromise the database as well as the source code to attempt cracking passwords.

   Hash = H(Password + Salt + Pepper)

