
Useful quick reference for [[Concept Application - John the Ripper]]
```
OPTIONS:  
--encoding=NAME  
--input-encoding=NAME  
input encoding (eg. UTF-8, IS0-8859-1).  
input encoding (alias for --encoding)  
--internal-encoding=NAME  
--target-encoding=NAME  
encoding used in rules/masks (see doc/ENCODING)  
output encoding (used by format)  
Example LM hashes from Western Europe, using a UTF-8 wordlist:  
john --format=lm hast.txt --encoding=utf8 --target:cp850 --wo:spanish.txt  
Example using UTF-8 wordlist with internal encoding for rules processing:  
john --format=#type hash.txt --encoding=utf8 --internal=CP1252 -  
wordlist=french.lst --rules  
Example mask mode printing all possible "Latin-1" words of length 4:  
john --stdout --encoding=utf8 --internal=8859-1 --mask:?l?l?l?l  
SUPPORTED LANGUAGE ENCODINGS  
UTF-8, 150-8859-1 (Latin), 150-8859-2 (Central/Eastern Europe), IS0-8859-7  
(Latin/Greek), !50-8859-15 (Western Europe), CP437 (Latin), CP737 (Greek), CP850  
(Western Europe), CP852 (Central Europe), CP858 (Western Europe), CP866  
(Cyrillic), CP1250 (Central Europe), CP1251 (Russian), CP1252 (Default Latinl),  
CP1253 (Greek) and KOI8-R (Cyrillic).
```


[[Home]]

#reference #john_attacks 