# Wild Web (Easy)

## Flavor text

While doing a post mortem, some logs for an old web server were found and we think there might be evidence of some attacks. Can you find them?

## How many GET requests were made to the server?

```grep -o "GET" access.log | sort | uniq -c | sort -nr | head -1```

> 8735

## How many unique IP addresses made requests to the server?

```awk '{print $1}' access.log | sort | uniq | wc -l```

> 294

## What is the most requests made from a single IP address?

```awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -n 1```

> 745

## What service had a brute force attack conducted against it?

> phpmyadmin

## How many attempts were made in the brute force attack?

```grep "username" access.log | wc -l```

> 33

## What IP address conducted an nmap scan?

> 85.91.96.54

## What was the most popular user agent? (include the full user agent string)

```grep -o "\"-\" \"[^-]*\"" access.log | sort | uniq -c | sort -nr | head -1```

> Toata dragostea mea pentru diavola