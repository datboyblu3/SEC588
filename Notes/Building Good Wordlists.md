### Assetnote

```
https://wordlists.assetnote.io
```

* Provides 1GB of wordlists available to download individually
* Capability of being an open Amazon S3 buckey
* You can provide Assetnote with a pull request through GitHub that adds the queries that you wish to look for

Assetnote GitHub Repo:
```
https://github.com/assetnote/wordlists
```

Follow the below link to request a Pull Request:
```
https://github.com/assetnote/wordlists/blob/master/.github/workflows/wordlists.yml
```

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



