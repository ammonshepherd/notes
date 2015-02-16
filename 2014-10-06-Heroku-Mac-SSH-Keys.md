---
layout: post
title:  "Heroku, Mac, SSH Keys"
date:   2014-10-06 10:10:55
---

# SSH issues

I followed the tutorials for getting Heroku to use the SSH keys but I always would get the following error when trying to git push to heroku:

<pre>
 $ git push heroku master
Saving password to keychain failed
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
</pre>

And Mac would pop up a window to enter the password for the SSH key. No matter what I put in there (even with a blank passphrase), it would fail.


If I changed the permissions on the .pub file, I would not get the pop up anymore, but I would get this error:

<pre>
$ git push heroku master
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/Users/aes9h/.ssh/id_rsa_heroku.pub' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
bad permissions: ignore key: /Users/aes9h/.ssh/id_rsa_heroku.pub
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
</pre>


 Nothing seemed to work. Found this help on stack exchange that fixed it:

 http://stackoverflow.com/questions/8786564/cannot-push-to-heroku-because-key-fingerprint

 the steps are thus:

     ssh-keygen -t rsa -C "firstname.lastname@gmail.com" -f  ~/.ssh/id_rsa_heroku

     ssh-add -K ~/.ssh/id_rsa_heroku

     heroku keys:add ~/.ssh/id_rsa_heroku.pub

