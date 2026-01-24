
Below we apply basic rules to help explain the expected output when using rules.  

| WORD     | RULE | OUTPUT |
| -------- | ---- | ------ |
| password | $1  | passwordl |
| password | ,...,,...1     | l!password       |
| password | so0 sa@     | p@ssw0rd       |
| password | c so0 sa@ $1     | P@ssw0rdl       |
| password | u r    |  DROWSSAP      |

##### MASKPROCESSOR HASHCAT-UTIL 

https://github.com/hashcat/maskprocessor 

Maskprocessor can be used to generate a long list of rules very quickly.  
Example rule creation of prepend digit and special char to dictionary candidates  
(i.e. "1 "! , "2 "@ , ... ): 
```
mp64.bin '"?d "?s' -o rule.txt  
```

Example creating rule with custom charset appending lower,uppercase chars and  
all digits to dictionary candidates (i.e. $a $Q $1 , $e $ A $2, ... ): 
```
mp64.bin -1 aeiou -2 QAZWSX '$?1 $?2 $?d'  
```

GENERATE RANDOM RULES ATTACK (i.e. "Raking")  
```
hashcat -a 0 -m #type -g <#rules> hash.txt dict.txt 
```

GENERATE RANDOM RULES FILE USING HASHCAT-UTIL 
```
generate-rules.bin <#rules> <seed> I ./cleanup-rules.bin [l=CPU,2=GPU] > out.txt  
generate-rules.bin 1000 42 I ./cleanup-rules.bin 2 > out.txt  
```

SAVE SUCCESSFUL RULES/METRICS
```
hashcat -a 0 -m #type --debug-mode=l --debug-file=debug.txt hash.txt -r rule.txt  
```

SEND RULE OUTPUT TO STDOUT / VISUALLY VERIFY RULE OUTPUT 
```

hashcat dict.txt -r rule.txt --stdout  

john --wordlist=dict.txt --rules=example --stdout

```

| HASHCAT INCLUDED RULES  | Approx # Rules |
| ----------------------- | -------------- |
| Incisive-leetspeak.rule | 15,487         |
|InsidePro-HashManager.rule| 6,746|  
|InsidePro-PasswordsPro.rule| 3,254  |
|T0XlC-insert_00-99_1950-2050_toprules_0_F.rule| 4,019 | 
|T0XlC-insert_space_and_special_0_F.rule| 482  |
|T0XlC-insert_top_100_passwords_l_G.rule |1,603  |
|T0XlC.rule| 4,088  |
|T0XlCv1.rule |11,934  |
|best64.rule| 77  |
|combinator.rule| 59  |
|d3ad0ne.rule |34,101  |
|dive.rule |99,092  |
|generated.rule |14,733  |
|generated2.rule| 65,117  |
|leetspeak.rule |29  |
|oscommerce.rule |256  |
|rockyou-30000.rule |30,000  |
|specific.rule |211  |
|toggles1.rule |15  |
|toggles2.rule |120  |
|toggles3.rule |575  |
|toggles4.rule |1,940  |
|toggles5.rule| 4,943  |
|unix-ninja-leetspeak.rule |3,073|


| JOHN INCLUDED RULES                                | Approx # Rules  |
| -------------------------------------------------- | --------------- |
|                                                    |                 |
| All (Jumbo + KoreLogic)                            | 7,074,300       |
| Extra                                              | 17              |
| Jumbo (Wordlist + Single + Extra + NT + OldOffice) | 226             |
| KoreLogic                                          | 7,074,074       |
| Loopback                                           | (NT + Split) 15 |
| NT                                                 | 14              |
| OldOffice                                          | 1               |
| Single                                             | 169             |
| Single-Extra (Single + Extra + OldOffice)          | 187             |
| Split                                              | 1               |
| Wordlist                                           | 25              |
|                                                    |                 |
|                                                    |                 |
http://www.openwall.com/john/doc/RULES.shtml  


#rules #howto 