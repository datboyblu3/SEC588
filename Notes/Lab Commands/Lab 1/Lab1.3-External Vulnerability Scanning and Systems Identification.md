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

Now, we wil take the lists of hosts and pass it to the `nuclei` vulnerability scanner

### Nuclei Vulnerability Scanner

```
cat /home/sec588/Coursefiles/workdir/http-hosts.txt | nuclei -o /home/sec588/Coursefiles/workdir/nuclei-output.txt -me /home/sec588/Coursefiles/workdir/nuclei-output-md/ -ni -duc -et miscellaneous/missing-x-frame-options.yaml,misconfiguration/http-missing-security-headers.yaml -t dns/,technologies/aws/aws-bucket-service.yaml,misconfiguration/aws-object-listing.yaml,technologies/tech-detect.yaml,technologies/s3-detect.yaml,technologies/werkzeug-debugger-detect.yaml,technologies/waf-detect.yaml,technologies/kubernetes/kubernetes-version.yaml,technologies/kubernetes/kube-api/kube-api-version.yaml,exposures/apis/openapi.yaml,http/takeovers/,ssl/,network/,exposures/configs/, -stats
```
> 
    -o: The output file in txt format to write the results out to.
    -me: Also write output in markdown format, "markdown export."
    -ni: Exclude interactsh templates.
    -duc: Disable update checks.
    -et: Exclude specific templates.
    -t: Include specific templates.
    -stats: Display statistics while scanning.
>

Review the results, search for vulnerabilities marked as high severity

### Search High Severity Results
```
cat /home/sec588/Coursefiles/workdir/nuclei-output.txt | grep -Ev "\[info\]|\[low\]"
```

You can also take screenshots with eyewitness and/or gowitness. 

### Gowitness
```
gowitness file -f /home/sec588/Coursefiles/workdir/http-hosts.txt
```

**Launch the gowitness webserver to display the results**
```
gowitness report serve --address :7171
```

Afterwards, navigate to the page 
```
http://localhost:7171
```

Or in the terminal
```
firefox http://localhost:7171
```

![image](https://github.com/datboyblu3/SEC588/assets/95729902/cf06a085-756f-4aff-997d-6bb9ed8eeacc)

![image](https://github.com/datboyblu3/SEC588/assets/95729902/c649034a-4bd7-4589-b56e-516fa4cf698a)



> Go here to find out more about these tools:
> 
> [eyewitness](https://github.com/RedSiege/EyeWitness)
> 
> [gowitness](https://github.com/sensepost/gowitness)
