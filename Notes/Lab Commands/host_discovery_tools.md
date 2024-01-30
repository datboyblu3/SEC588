### PureDNS Bruteforce Mode
```
puredns bruteforce wordlist.txt -d targetdomains.txt
```

To resolve addresses, use the `resolve` keyword
```
puredns resolve targethosts.txt
```

### DNSx
```
dnsx -silent -l hosts.txt
```

### Piping subfinder results into DNSx

This will determine if the outputs from subfinder are still valid and still resolves.
```
subfinder -silent -d sec588.org | dnsx -silent -a -resp
```


