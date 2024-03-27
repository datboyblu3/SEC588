**Create an IP address list that excludes more generic services. The json file is from the last lab**
```python
cat /home/sec588/Coursefiles/workdir/dnsx-awesome-sparrow-sec588.json | jq -r '. | "Hostname: \(.host) A Record: \(.a[]?)"' | grep -Ev "mail|MX|heroku|office|s3-website|files|www-dev|www|images" | awk '{ print $5 }' | anew /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt
```

**Map an IP address to a Cloud Service Provider**
```python
edge -ip /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt -prefix
```

**Masscan**
```python
sudo /opt/masscan/bin/masscan --adapter tun0 --open-only --source-port 40000-41023 -p 1-1024,3000,5000,6379,27017 -oB /home/sec588/Coursefiles/workdir/masscan.bin -iL /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt
```
>
    
    --adapter tun0: This specifies our OpenVPN interface, and we need to do this to prevent Masscan from scanning from our ethernet interface. Masscan will not respect the local routing table as it sets up its TCP/IP stack for faster scanning
    --open-only: Report only open ports, not closed ports
    --source-port: This sets the source ports used for scanning
    -p: The ports to scan, in this case, are 1-1024 (server ports), 3000 (NodeJS server port), 5000 (random server ports), 6379 (Redis port), 27017 (MongoDB port)
    -oB: Output to binary mode; this is more space efficient for large scans
    -iL: List of IP addresses to scan


**Masscan - Read the scan from Masscan**
```python
/opt/masscan/bin/masscan --readscan /home/sec588/Coursefiles/workdir/masscan.bin
```

**Parse the output into a Nmap scan**

parses the output of Masscan, separating it by /, which is how the ports show up. I.E. 443/tcp. We then sort that list to remove duplicates. It then prints out the first section of it. Subsequently, the following awk command will print out the ports by themselves. Then tr will replace new lines \n with ,

```python
/opt/masscan/bin/masscan --readscan /home/sec588/Coursefiles/workdir/masscan.bin | awk -F\/ '{ print $1 }' | sort -u | awk '{ print $4 }' | tr "\n" "," > /home/sec588/Coursefiles/workdir/portlist.txt
```

**Run Nmap**

```python
sudo nmap -iL /home/sec588/Coursefiles/workdir/target-systems-by-ip-filtered.txt -e tun0 -p `echo $(cat /home/sec588/Coursefiles/workdir/portlist.txt)` -n -O -sV --script redis-info,mongodb-databases,http-git,http-methods,http-passwd --open --reason -Pn -oA /home/sec588/Coursefiles/workdir/scan
```
Let's review the Nmap options that we used in this command: - -iL: Like Masscan, this provided Nmap with a list of IP addresses. - -e tun0: This specified the VPN interface, just like Masscan Nmap sets up its TCP/IP stack that doesn't look at the routing table. - -p: This one looks odd; what it does is that it runs the cat command to print out the port list and then uses echo to put it into the -p command. It does look odd, but it works. - -n: This switch disables DNS resolution. - -O: This turns on OS Fingerprinting - -sV: This turns on banner grabbing - --script: In this case, we are specifying only the following scripts to be run: redis-info, mongodb-databases, http-git, http-methods, http-passwd. - --reason: This will explain why Nmap thinks a port is closed or open, we will not really use this feature as only open ports are displayed - -Pn: This switch will tell Nmap not to probe a host and assume it's available. - -oA: This will tell Nmap to output in all of its outputting formats. - --open: Only show open ports, do not show closed ports

### Naabu

`naabu` can gracefully take in plaintext strings from cat or echo to run portscans
```python
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
