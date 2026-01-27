
Corpus: Collection_1 - Anti-Public Dump

[[Password Analysis Findings]]
Samples : **2,786,840,437**

#### Notes 

Samples from high profile collection leaked in 2019. The Anti-Public dump is comprised of leaked email credentials from various large scale providers:

- @gmail.com
- @yahoo.com
- @aol.com  
    Sample chunks are very large, most in excess of 10GB and appear in the following format:

```
someone@email.com:andapassword
```

Post processing with bash command line tools

```
$ cat all_files.txt* | cut -f 2 -d ":" > spliced_samples.lst
```

combines all files into one with the password portion of the samples only

```
passowrd
imagreatguy
make34coolagain
etcetcetc
...
```

**At this point data is good for compression (optional) and use with hashcat**

###### Compress into tar.gz with [pigz](https://hashtopia.net/pigz) 

You can use [pigz](http://zlib.net/pigz/) instead of gzip, which does gzip compression on multiple cores. Instead of using the -z option, you would pipe it through pigz:

```
tar cf - paths-to-archive | pigz > archive.tar.gz
```

By default, pigz uses the number of available cores, or eight if it could not query that. You can ask for more with -p n, e.g. -p 32. pigz has the same options as gzip, so you can request better compression with -9. E.g.

```
tar cf - paths-to-archive | pigz -9 -p 32 > archive.tar.gz
```

Data is still in random order from the raw dump which kind of works out. PACK doesnt do well with files over 1GB ( about 5 min to process 1 GB file) so need to break list that was just created into 1 GB chunks.

```
split --bytes=1G
```

BOOM! twenty seven 1GB chunks of passwords

#### statsgen.py 

Master PACK statsgen.py

```
     StatsGen 0.0.3   | |
      _ __   __ _  ___| | _
     | '_ \ / _` |/ __| |/ /
     | |_) | (_| | (__|   <
     | .__/ \__,_|\___|_|\_\
     | |
     |_| iphelix@thesprawl.org


[*] Analyzing passwords in [xaa]
[*] Saving advanced masks and occurrences to [antipublic_001.mask]
[+] Analyzing 99% (104659109/104753498) of passwords
[*] Statistics below is relative to the number of analyzed passwords, not total number of passwords

[*] Length:
[+]                         8: 27% (28352940)
[+]                         7: 17% (18072323)
[+]                         6: 16% (17073540)
[+]                         9: 14% (15212157)
[+]                        10: 14% (14759128)
[+]                        11: 02% (2529517)
[+]                        12: 01% (1818013)
[+]                         5: 01% (1688264)
[+]                        13: 00% (1045673)
[+]                         4: 00% (990044)
[+]                        15: 00% (893970)
[+]                        14: 00% (737475)
[+]                        16: 00% (445247)
[+]                         3: 00% (296050)
[+]                        17: 00% (153332)
[+]                        18: 00% (140916)
[+]                        20: 00% (88137)
[+]                        19: 00% (78938)
[+]                         1: 00% (60546)
[+]                         2: 00% (52192)
[+]                        21: 00% (41962)
[+]                        22: 00% (38429)
[+]                        24: 00% (26964)
[+]                        23: 00% (25199)
[+]                        25: 00% (19813)
[+]                        26: 00% (11480)
[+]                        27: 00% (6860)

[*] Character-set:
[+]             loweralphanum: 61% (64015565)
[+]                loweralpha: 21% (22519115)
[+]                   numeric: 08% (8660865)
[+]             mixedalphanum: 02% (2729406)
[+]         loweralphaspecial: 02% (2694612)
[+]      loweralphaspecialnum: 01% (1743663)
[+]                mixedalpha: 00% (845671)
[+]             upperalphanum: 00% (578711)
[+]                upperalpha: 00% (404307)
[+]                       all: 00% (189551)
[+]                specialnum: 00% (137955)
[+]         mixedalphaspecial: 00% (49772)
[+]      upperalphaspecialnum: 00% (36758)
[+]                   special: 00% (32079)
[+]         upperalphaspecial: 00% (21079)

[*] Password complexity:
[+]                     digit: min(0) max(27)
[+]                     lower: min(0) max(27)
[+]                     upper: min(0) max(27)
[+]                   special: min(0) max(27)

[*] Simple Masks:
[+]               stringdigit: 52% (54563372)
[+]                    string: 22% (23769093)
[+]                     digit: 08% (8660865)
[+]               digitstring: 05% (5946028)
[+]                 othermask: 03% (3681791)
[+]         stringdigitstring: 03% (3233402)
[+]             stringspecial: 01% (1517377)
[+]       stringspecialstring: 00% (844504)
[+]          digitstringdigit: 00% (825982)
[+]        stringspecialdigit: 00% (756676)
[+]        stringdigitspecial: 00% (340927)
[+]             specialstring: 00% (135180)
[+]      specialstringspecial: 00% (62692)
[+]        specialdigitstring: 00% (57646)
[+]        digitspecialstring: 00% (50324)
[+]              digitspecial: 00% (47127)
[+]        digitstringspecial: 00% (44315)
[+]        specialstringdigit: 00% (33569)
[+]                   special: 00% (32079)
[+]              specialdigit: 00% (28668)
[+]         digitspecialdigit: 00% (22250)
[+]       specialdigitspecial: 00% (5242)

[*] Advanced Masks:
[+]          ?l?l?l?l?l?l?l?l: 07% (8256924)
[+]          ?l?l?l?l?l?l?d?d: 04% (5089922)
[+]            ?l?l?l?l?l?l?d: 04% (4914611)
[+]              ?l?l?l?l?l?l: 03% (4145979)
[+]          ?l?l?l?l?l?l?l?d: 03% (4077862)
[+]        ?l?l?l?l?l?l?l?l?d: 03% (3180251)
[+]              ?d?d?d?d?d?d: 03% (3153495)
[+]            ?l?l?l?l?l?d?d: 02% (3082306)
[+]        ?l?l?l?l?l?l?l?d?d: 02% (2988990)
[+]            ?l?l?l?l?l?l?l: 02% (2867758)
[+]      ?l?l?l?l?l?l?l?l?d?d: 02% (2669734)
[+]              ?l?l?l?l?l?d: 02% (2663709)
[+]              ?l?l?l?l?d?d: 02% (2538172)
[+]      ?l?l?l?l?l?l?l?l?l?d: 02% (2350831)
[+]      ?l?l?l?l?l?l?l?l?l?l: 01% (2039686)
[+]          ?d?d?d?d?d?d?d?d: 01% (1796703)
[+]        ?l?l?l?l?l?l?l?l?l: 01% (1739644)
[+]          ?l?l?l?l?d?d?d?d: 01% (1678137)
[+]          ?l?l?l?l?l?d?d?d: 01% (1557706)
[+]        ?l?l?l?l?l?l?d?d?d: 01% (1548602)
[+]      ?l?l?l?l?l?l?d?d?d?d: 01% (1282363)
[+]        ?l?l?l?l?l?d?d?d?d: 01% (1253692)
[+]      ?l?l?l?l?l?l?l?d?d?d: 01% (1139775)
[+]            ?l?l?l?l?d?d?d: 01% (1128826)
[+]              ?l?l?l?d?d?d: 01% (1054340)

```

On the Windows side, Onedrive Spreadsheet compiling all statsgen results  
master list - need to link this sucker up to do the math  
pages in the book will be statsgen imported with corresponding labels.

I think that is the complete workflow.  
That raw data is received and processed for both analysis and use in the production cracking pipeline so this is the initial task.  
Initial task will break out into two different activity loops:

- running the processed data through the password cracking processes in conjuction with the [#dictionary](https://publish.obsidian.md/#dictionary) , [#combinator](https://publish.obsidian.md/#combinator) , [#hybrid](https://publish.obsidian.md/#hybrid) attacks and [#rules](https://publish.obsidian.md/#rules) applications
- breaking the processed data down into managable chunks, running through PACK and capturing the statistics
    - PACK workflow  
        Statsgen.py  
        with 1GB chunked data:  
        `statsgen target.lst --maxlength=27 -o target.masks`   
        migrate output to workbook  
        Maskgen.py  
        with .mask file that was created generate efficient masks using:  
        tune the values and options until test cases run the desired length  
        `maskgen`   
        The list below shows additional filters you can use:  
        `Individual Mask Filter Options: --minlength=8 Minimum password length --maxlength=8 Maximum password length --mintime=3600 Minimum mask runtime (seconds) --maxtime=3600 Maximum mask runtime (seconds) --mincomplexity=1 Minimum complexity --maxcomplexity=100 Maximum complexity --minoccurrence=1 Minimum occurrence --maxoccurrence=100 Maximum occurrence`   
        Policygen.py


#pitl0rd #wordlists


[Home](https://hashtopia.net/Home)****