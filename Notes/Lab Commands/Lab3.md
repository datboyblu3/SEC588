# External Vulnerability Scanning and Systems Information

```
cat /home/sec588/Coursefiles/workdir/target-systems-by-host.txt | naabu -i tun0 -silent | httpx -o /home/sec588/Coursefiles/workdir/http-hosts.txt
```
> NOTE: The output is not the needed format of `ip:port` but in `host:port` and also each IP is on a seperate line
> Solution: Use the `naabu` `-silent` mode switch to remove all informational messages and output just the `host:port`
> Then forward the list of hosts to `httpx`


### HTTPX

```
cat /home/sec588/Coursefiles/workdir/target-systems-by-host.txt | naabu -i tun0 -silent | httpx -o /home/sec588/Coursefiles/workdir/http-hosts.txt
```

What is HTTPX doing with the wordlist created from naabu?

* Read each line passed to httpx, typically each host and port, and first attempt an HTTPS request. If successful, write a line with the format of https://target.local. If a port outside of 443 is specified, append it to the end of the string :port.
* If HTTPS is not available, attempt HTTP with the same results.
* If neither protocol works, then do not save the host or write it to a file
