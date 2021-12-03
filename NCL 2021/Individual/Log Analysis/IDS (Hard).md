# IDS (Hard)

## Flavor Text

We are trying to generate a report from our Intrusion Detection System, can you help?

## How many unique destination IP addresses are there?

```grep "dest_ip" eve_alert.json | sort | uniq | wc -l```

> 298

## How many unique signatures were recorded?

```grep "signature\"" eve_alert.json | sort | uniq | wc -l```

> 75

## What is the most common attack category?

```grep "category\"" eve_alert.json | sort | uniq -c | sort -nr | head -1```

> Potentially Bad Traffic

## What is the most common attack signature?

```grep "signature\"" eve_alert.json | sort | uniq -c | sort -nr | head -1```

> "ET POLICY SMB2 NT Create AndX Request For a DLL File - Possible Lateral Movement

## How many total bytes are sent to the IP 10.47.8.20?

```jq '.[] | select(.dest_ip=="10.47.8.20")' eve_alert.json | grep "bytes_toserver\".*" | awk '{print $2}' | rev | cut -c2- | rev | awk '{sum += $1} END {print sum}'```

> 178746

## How many seconds elapsed during this log?

```jq '.[] | select(.proto != "TCP") | .alert.category' eve_alert.json```

> Generic Protocol Command Decode