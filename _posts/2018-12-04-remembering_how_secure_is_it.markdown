---
layout: post
title:      "Remembering... how secure is it?"
date:       2018-12-05 00:38:03 +0000
permalink:  remembering_how_secure_is_it
---

Yep, yep, got it. Wait, how does that work again? ActiveRecord, Sinatra, Rails... instead of reinventing the wheel, being able to call upon these pre-written methods and frameworks make it so much easier and fun to program in Ruby. It also makes it easy to forget the logic behind the method and just remember that there's a method that will help you do something like store a password securely.

Most of the time, not knowing how the method works doesn't matter since the method name is self explanatory and it's purpose is relatively uncomplicated. When it comes to passwords though, it's a constant battle to stay ahead of hackers and find better ways to keep passwords from being stolen or "guessed at". So, it's important to understand how you are securing passwords in your application and also realize that at some point you might need to go back and update how passwords are being encrypted and stored when better methods become available.

### A PASSWORD PRIMER

***Passwords stored in plain text are bad***... 

Funny how once upon a time, I thought it was great that a website would just email me my username and password upon request. If this happens, then your password is being stored in plain text. Which means that anyone that gains access to the database can literally just read every username and password. They'll not only have access to that website, but any other website that the user used the same username and password for. A hacker can also just run a program that tests a huge list of the most common passwords (a dictionary attack). This is why users should not use common passwords and should have unique passwords for each websites.

***Hashed passwords can be hacked***... 

Hashing algorithms like `md5` and `sha1` are like secret encryptors that will turn your password into a string of random characters and numbers that can be stored instead of the plain text password. Since a particular string like "`12345`" will always produce the same hash "`827CCB0EEA8A706C4C34A16891F84E7B`", the password used to log in can be hashed and then compared to the hashed password stored in the user database. Great, right? The problem is, all I had to do was find an MD5 Hash Generator on the internet and type in "`12345`" to find the `md5` hashed version. Hackers can run a dictionary attack by creating a list of hashed common passwords or use one that someone else has posted on the internet. If any of the hashed passwords match up, the hacker knows what plain text password to type in. Sometimes you can even just google a hashed password to find the plain text.

***Salt a hash***... 

Before a password gets hashed, it can be salted - which is basically a string added to the password. So, "`12345`" would become a less common "`12345mysalt`" and that is what would get hashed and stored. The problem with this is that if a hacker has compromised a common password, they can use that to figure out the salt and then use that salt to generate another dictionary.

***Enter bcrypt***... 

`bcrypt` ensures that users with the same password will have completely different salted hashes stored for their password by creating a random, unique salt for every single user. This exponentially increases the number of dictionaries that a hacker would have to create for each unique salt. For a salt made up of 4 characters, that's almost half a million dictionaries. `bcrypt` is designed to be slow by having an adjustable cost factor built in that allows the programmer to effectively increase or decrease the amount of computation time required to hash a password. So, while `bcrypt` is not unhackable, the shear amount of disk space needed to create dictionaries for all the unique salts and the time required to run everything through `bcrypt` makes the resource cost too high.

### USING `bcrypt`

Encrypting and storing passwords using `bcrypt` is easy:

1. Install the `bcrypt` gem by including it in your application's gemfile.
2. Include a `password_digest` column in the `users` table.
3. Allow the user to create a `password` attribute when they sign up
4. Call on the `has_secure_password` method in the `user` model

Now I understand and am less fearful of why so many websites ask you if you want to sign up or login through your Google or Facebook account. Who better to outsource your password security to?



