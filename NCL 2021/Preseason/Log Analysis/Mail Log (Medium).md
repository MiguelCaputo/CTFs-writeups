# Mail Log (Medium)

# Flavor Text

The mayor's office thinks that someone has been leaking classified information. Analyze some logs and see what's going on.

Note: 1 email = 1 entry where stat=Sent

# Solving them 

We can use grep and regex to find most of the answers,

## How many emails were sent to bkwconsulting.com?

We can find all emails that were send to bkwconsulting and count how many lines grep returns 

```grep "to=<.*@combo.bkwconsulting.com>" MailLog.txt | wc -l```

> 177

## How many emails were successfully sent?

```grep "stat=Sent" MailLog.txt | wc -l```

> 592

## What is the unique identifier for the only email successfully sent to multiple recipients?

First we need to find the format for multiple recipients:

We can see that it is: "to=<mail>,<mail>,<mail>,..."
	
We can find all the emails that were sent to multiple recipients like: 
	
```grep "to=<.*>,<.*>" MailLog.txt```
	
We can find the one that succeded like this:
	
```grep "to=<.*>,<.*>.* stat=Sent" MailLog.txt```

We get: 
	
```
Aug 28 07:42:33 combo sendmail[29519]: j7S4xvYX024921: to=<awdcvbhuk@yahoo.com.tw>,<awdr151@yahoo.com.tw>,<awdr78321@yahoo.com.tw>,<awdrg112329@yahoo.com.tw>,<awdrg121@yahoo.com.tw>,<awdrgyjilp1204@yahoo.com.tw>,<awds142@yahoo.com.tw>,<awdsxz@yahoo.com.tw>, delay=06:41:38, xdelay=00:48:35, mailer=esmtp, pri=421994, relay=mx7.mail.tw.yahoo.com. [203.84.194.57], dsn=2.0.0, stat=Sent (ok dirdel 8/0)
```
	
>  j7S4xvYX024921
	
## How many emails were successfully sent to non-US country TLDs?

We can find all the sent emails, and then we check the email addresses for something after .net and .com
	
```grep -o "to=<.*>, .* stat=Sent" MailLog.txt | grep -o "to=<.*>," | grep -e "\.com\." -e "\.net\." | wc```
	
> 19
	
## How many different IP addresses had an IP name lookup failure?

We just look for unique IPs that are after the string: "IP name lookup failed."

```grep -o "IP name lookup failed .*" MailLog.txt | sort | uniq | wc```
	
> 42
	
	