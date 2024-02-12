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
