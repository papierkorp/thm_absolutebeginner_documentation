#tools 

**hashcat** is the world’s fastest and most advanced password recovery/cracking tool.

mode number = Parameter for specific hashes

**--force can lead to false positives/negatives** 

```
hashcat -a 0 -m 3200 hash.txt /usr/share/wordlists/rockyou.txt
```

- a = attack mode (0 = dictionary)
- -m = [hash mode](https://hashcat.net/wiki/doku.php?id=example_hashes) of hash
- hash.txt = list of hashes