# The Magical Modbus

We are given a .pcap file that can be opened  with Wireshark

If we check all the packages we can see that the last bits of all connections between the source 238.0.0.6 and the destination 238.0.0.5 contain characters.

We can apply ```ip.src == 238.0.0.6``` as a filter, and check all packages in order to character by character construct the flag.

# Flag

> flag{Ms_Fr1ZZL3_W0ULD_b3_s0_Pr0UD}
