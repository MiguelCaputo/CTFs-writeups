# Seek  

We are given a C file and we need to construct the flag from characters without order

We can copy all the ```fseek``` functions into a file and 

```
cat a.txt | sort -n | uniq > b.txt
cat b.txt | awk '{print $3}' | awk -F ')' '{print $1}'
```

Then we can remove the new lines, put it in cyberchef and see:

![image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/DCTF%2022/flag.png)

## Flag

> DCTF{FOUND}