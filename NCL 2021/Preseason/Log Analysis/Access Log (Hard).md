# Access Log (Hard)

# Flavor Text

Analyze access logs from an NGINX server to find trends in the data.

# Solving them 

We can use grep and regex to find most of the answers,

## How many requests were made for "/"?

```grep "GET / " access.log | wc -l```

> 1981

## How many requests yielded a 405 status code?

We can find the column that has all the available statutes codes by using awk

```awk '{print $9}' access.log | grep "405" | wc```

> 18

## What IP address made the most requests?

We can find all IPS, get the unique ones, and find the one that was used the most

```awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -n 1```

> 107.170.42.130

## How many bytes were transferred in total?

We can use a sum with awk

```awk '{sum += $10} END {print sum}' access.log```

> 2266714959

## How many requests were "refered" by Google?

In a NGINX server the field after the package bytes are the refered information, so we can look for requests that are connected to google.

```grep -o "\" [0-9][0-9][0-9].* \".*google.*\" \"" access.log | wc -l```

> 273

## What IP address attempted to exploit the Shellshock vulnerability?

Shellsock vulnerability usually looks on the request as a bunch of shell commands, we can find it by looking for /bin/

```grep "/bin/" access.log```

```
213.251.182.107 - - [24/Apr/2015:00:20:48 +0200] "GET /?x=() { :; }; echo Content-type:text/plain;echo;echo;echo M`expr 1330 + 7`H;/bin/uname -a;echo @ HTTP/1.0" 303 245 "() { :; }; echo Content-type:text/plain;echo;echo;echo M`expr 1330 + 7`H;/bin/uname -a;echo @" "() { :; }; echo Content-type:text/plain;echo;echo;echo M`expr 1330 + 7`H;/bin/uname -a;echo @"
```

> 213.251.182.107

## How many HTTP requests were made on the busiest day?

We can find all days, count the days, and find the days with a biggest count

```awk '{print $4}' access.log | cut -c 2-12 | sort | uniq -c | awk '{print $1}' | sort -nr | head -n 1```

> 2089 (14/Apr/2015)

## How many bytes were transferred on the day where the most bytes were transferred (specify result in bytes)?

We can find each day, the number of bytes sent on that day, and group by date by adding the number of bytes 

```awk '{print $4 " " $10}' access.log | cut -c 1,13-21 --complement | awk ' {arr[$1]+=$2} END { for (key in arr) printf("%s\t%s\n", key, arr[key])}' | awk '{print $2}' | sort -nr | head -n 1```

> 163340076

