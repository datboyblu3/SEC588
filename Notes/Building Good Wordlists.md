### Commonspeak2
```
https://github.com/assetnote/commonspeak2
```

### Publicly Available Datasets
```
https://www.reddit.com/r/bigquery/wiki/datasets
```

### Building a Subdomain wordlist with Commonspeak2
```
./commonspeak2 --project sec588 --credentials ~/.config/gcloud/appcred.json subdomains -o ~/subdomains.txt
```

### Building a .jsp wordlist with Commonspeak2
```
./commonspeak2 --project sec588 --credentials ~/.config/gcloud/appcred.json ext-wordlist.txt -e jsp ~/jsp.txt
```



