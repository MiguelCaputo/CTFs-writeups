# Contact Us

We are given a .pcap file that we can open with Wireshark and sslkeyfile.txt

# Adding the key file 

We need to add the key file into WireShark

> EDIT => PREFERENCES => PROTOCOLS => TLS => (Pre)-Master-Secret Log filename

# Finding the flag

Know that we added the key file we can filter packages to TLSV1.2, we rick click in one and "Follow TLS Stream."

We need to find the one that looks like:

![Screenshot](image.png)

# Flag

> flag{m@r$hm3ll0w$}
