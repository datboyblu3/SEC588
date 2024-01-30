### Map an IP address to a Cloud Service Provider

```
edge -ip /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt -prefix
```

### Masscan
```
sudo /opt/masscan/bin/masscan --adapter tun0 --open-only --source-port 40000-41023 -p 1-1024,3000,5000,6379,27017 -oB /home/sec588/Coursefiles/workdir/masscan.bin -iL /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt
```

### Masscan - Read the scan from Masscan
```
/opt/masscan/bin/masscan --readscan /home/sec588/Coursefiles/workdir/masscan.bin
```

### Parse the output into a Nmap scan

parses the output of Masscan, separating it by /, which is how the ports show up. I.E. 443/tcp. We then sort that list to remove duplicates. It then prints out the first section of it. Subsequently, the following awk command will print out the ports by themselves. Then tr will replace new lines \n with ,

```
/opt/masscan/bin/masscan --readscan /home/sec588/Coursefiles/workdir/masscan.bin | awk -F\/ '{ print $1 }' | sort -u | awk '{ print $4 }' | tr "\n" "," > /home/sec588/Coursefiles/workdir/portlist.txt
```

### Run Nmap

```
sudo nmap -iL /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt -e tun0 -p `echo $(cat /home/sec588/Coursefiles/workdir/portlist.txt)` -n -O -sV --script redis-info,mongodb-databases,http-git,http-methods,http-passwd --open --reason -Pn -oA /home/sec588/Coursefiles/workdir/scan
```

### Naabu

```
cat target-systems-by-host.txt | grep -Ev "www-dev|mail|files|images|www.|^awesome-sparrow" | naabu -p 22,80,443,6379,27017 -o /home/sec588/Coursefiles/workdir/naabu-portscan.txt
```

### Naabu w/ dnsx

using dnsx to query each and every IP address:

    -d: Domains to scan using DNSx
    -w: The Wordlist to use
    -resp-only: Only show (or gather) the reponses, ignore the names.
    -a A records only (IPv4)

What about naabu?

    -p: Ports to scan
    -stats: Display statistics every so often
    -o: Output to this file.


```
dnsx -d /home/sec588/Coursefiles/workdir/target-domains.txt -w /home/sec588/Coursefiles/wordlists/subdomains-5k.txt -resp-only -a -silent | naabu -p 22,80,443,6379,27017 -stats -o naabu-portscan.txt
```
