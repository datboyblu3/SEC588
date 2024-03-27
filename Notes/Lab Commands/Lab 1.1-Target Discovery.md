

**Passively Discover Assets with subfinder**
```python
subfinder -dL /home/sec588/Coursefiles/workdir/target-domains.txt -o /home/sec588/Coursefiles/workdir/subfinder-hosts.txt -stats
```
>

    -dL: A list of target domains provided by the file
 
    -o: The output file in txt format to write the results out to
 
    -stats: This command will display statistics about the current running job


**Brute force domains with dnsx**
```python
dnsx -d /home/sec588/Coursefiles/workdir/target-domains.txt -w /home/sec588/Coursefiles/wordlists/subdomains-5k.txt -a -aaaa -cname -txt -mx -json -o /home/sec588/Coursefiles/workdir/dnsx-awesome-sparrow-sec588.json -r 10.10.60.35 -re -stats
```
>

    -d: Domain Name
    -w: Wordlist
    -o: Output
    -r: Force the use of a specific resolver

**Use jq to display only the A records that have a CNAME record from the dnsx output**
```python
cat /home/sec588/Coursefiles/workdir/dnsx-awesome-sparrow-sec588.json | jq -r '. | "Hostname: \(.host) A Record: \(.a[]?) CNAME Record: \(.cname[]?)"' 
```
>

    
    -r : Display results in "raw" mode
    '. : This command will grab all the json objects that came over the command pipe
    | : The pipe operator within jq will pass all the objects over
    \(.host) : Pull out a key that matches the name host
    \(.a[]?) : Pull out a key that matches the name of a, for a records, and will only display values if they exist (?)
    \(.cname[]?): Pull out a key that matches the name of cname, for cname records, and will only display values if they exist (?)


**From the list output above, create a target list to look for vulnerabilities**
```python
cat /home/sec588/Coursefiles/workdir/subfinder-hosts.txt | anew /home/sec588/Coursefiles/workdir/target-systems-by-host.txt
```
