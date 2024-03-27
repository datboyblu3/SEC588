

**Passively Discover Assets with subfinder**
```
subfinder -dL /home/sec588/Coursefiles/workdir/target-domains.txt -o /home/sec588/Coursefiles/workdir/subfinder-hosts.txt -stats
```
>

    -dL: A list of target domains provided by the file
 
    -o: The output file in txt format to write the results out to
 
    -stats: This command will display statistics about the current running job


**Brute force domains with dnsx**
```
dnsx -d /home/sec588/Coursefiles/workdir/target-domains.txt -w /home/sec588/Coursefiles/wordlists/subdomains-5k.txt -a -aaaa -cname -txt -mx -json -o /home/sec588/Coursefiles/workdir/dnsx-awesome-sparrow-sec588.json -r 10.10.60.35 -re -stats
```
>

    -d: Domain Name
    -w: Wordlist
    -o: Output
    -r: Force the use of a specific resolver



