 # Apocryphon
 
 ## Flavor Text
 
 We got this suspicious email today in our inbox from someone claiming to have popped our infra and are now demanding a ransom. While we have high confidence that these claims are fake, we would still like to investigate the matter fully.

Can you help us find the true identity of the person behind this email?

The flag is in standard flag format.
 
 ## Solution 
 
 We are given a .eml file called **Iv3_g0t_yo=0u_r19ht_1n_my_7r4ck5.eml** that contains the following email
 
 ![[Dam/email.png]]
 
 If we want to check the sender we can use strings on the file
 
 ```strings Iv3_g0t_yo=0u_r19ht_1n_my_7r4ck5.eml```
 
 > From the-information-society-231645@protonmail.com  Fri Nov 05 06:22:21 2021
X-Original-To: admin@damctf.xyz
To: admin@damctf.xyz
Subject: Iv3 g0t yo=0u r19ht 1n my 7r4ck5
MIME-Version: 1.0
Content-Type: text/plain; charset="UTF-8"
Content-Transfer-Encoding: 8bit
Date: Fri, 05 Nov 2021 06:22:21 +0000 (UTC)
From the-information-society <the-information-society-231645@protonmail.com>
== ATTENTION ADMINS OF DAMCTF ==
== VERY IMPORTANT MESSAGE MUST READ ==
ive hacked into your systems and can see all your infra >:)
i can see all of your secrets >:)
you better send me one million dogecoin or im gonna send your browsing history to the entire internet :O
i want those dogs in my wallet by tomorrow or youre gonna get leaked sucker B)
ciao nerds- best hacker

We now know that the email came from: **the-information-society-231645@protonmail.com** 

We can test that email address in multiple websites, but by knowing that other OSINT challenges in this CTF were using github accounts we can check if there is a github account created with this email, to do this we can try to register a new account with that email, when we do that we get an error message saying that there is already an account created with that email.

We know need to find the user that is linked to that email.

We can try using the github API like:

```
curl -i -s https://api.github.com/search/users?q=the-information-society-231645@protonmail.com
```

But that did not returned anything useful.

After researching a bit more about how to link email to github users I found this post: https://github.community/t/api-to-get-user-check-if-exists-by-email/1305/3

If we read the comment made by "belochub" we can see that he mentions that if we set our email on git to the one we want to look for and we make a commit to any repository the commit will look like it was made by the user that is connected to that email!

So lets try that, we need to set our git email to "the-information-society-231645@protonmail.com" 

```
git config --global user.email the-information-society-231645@protonmail.com
```

Now we make a commit to any of our repositories, and just like that we can find an user:

![[commit.png]]

We can now go to his profile:

![[profile.png]]

Now we just need to find the flag.... 

(This might be a bug)

In order to find the flag we need to open the repository with a phone or in responsive design mode and just like that we can see the flag.

![[flag.png]]

## Flag

> dam{guess_im_in_your_sights_instead}