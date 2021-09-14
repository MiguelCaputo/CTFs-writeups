# Weak Password

We are given a password hash "7f4986da7d7b52fa81f98278e6ec9dcb" that we need to decrypt. 

We are told that the owner of the account "Aaron" usually uses his birthday in the format YYYYMMDD and his name in his passwords.

# Finding type of hash

In order to break the hash, we first need to know what type of hash it is, for that we can go to https://md5decrypt.net/en/HashFinder/ and check the given hash

We find that the hashing algorithm is probably MD5

# Creating a wordlist

Knowing the structure of the password we can create a wordlist of possible passwords using regex and the python module exrex.py.

``` exrex "([Aa]ron([21][90][0-9][0-9])([10][0-9])([0-3][0-9]) | ([21][90][0-9][0-9])([10][0-9])([0-3][0-9])([Aa]ron)" > passwords.txt ```

This will give us all possible passwords using Name-Date and Date-Name.

# Finding the password

After creating the wordlist we can use hashcat to decrypt the hash.

``` hashcat -m 0 hashes.txt passwords.txt -O ```

The hashcat returns "Aaron19800321" as the password.

# Flag

> flag{Aaron19800321}
