# Error Log (Medium)

# Flavor Text

Analyze HTTP error logs to track server behavior and identify malicious actors.

# Solving them 

We can use grep and regex to find most of the answers,

## How many "File does not exist" errors are in the log?

Just found all instances of "File does not exist"

```grep "File does not exist:" error.log | wc -l```

> 65119

### How many unique IP addresses are listed in the log?

We just need to find all IP addresses and count the unique ones

```grep -o "\[client .*\] " error.log | sort | uniq | wc -l```

> 8121

## What IP address generated the most errors?

We can just do the same thing as before but we count how many times an IP appears

```grep -o "\[client .*\] " error.log | sort | uniq -c | sort -nr | head -n 1```

> 88.80.10.1

## What IP address scanned for the most unique files?

