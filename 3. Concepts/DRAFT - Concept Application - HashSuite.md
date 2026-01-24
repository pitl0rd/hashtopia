# Hash Suite
This tutorial was written using **Hash Suite 3.4 Pro** and assumes basic knowledge of password hashing and password hash cracking.

[Purchase it](https://hashsuite.openwall.net/purchase) or you can download the [free version](https://hashsuite.openwall.net/Hash_Suite_Free.zip).

When the download completes unzip the file and execute **Hash_Suite_64.exe** (this executes the 64 bits version that is faster than the 32 bits). The **Welcome** dialog (fig 1) appears at first run with some basic information. Press **Enter** or click the **OK** button to dismiss.

![Welcome Dialog](https://hashsuite.openwall.net/tt_welcome.png "Welcome Dialog")  
**Fig 1: Welcome Dialog.**

### Preparations

Let’s do some preparations before we begin to crack passwords. First we will run a benchmark to know our hardware performance. Press **alt+f+b** to run a quick benchmark (fig 2). Hash Suite uses a ribbon interface that supports hierarchical keyboard shortcuts. We will use these shortcuts heavily in the tutorial.

![Benchmark](https://hashsuite.openwall.net/tt_benchmark.png "Benchmark")  
**Fig 2: Benchmark on tutorial hardware.**

The benchmark lasts ~12 minutes (and you may stop it whenever you want) on our system composed of one CPU (Core i5-4670) with a high-end gaming GPU (GeForce GTX 970). Hash Suite automatically selects **Threads=3** (of 4), which means we dedicate 3 CPU cores to hashing and 1 CPU core to GPU communication. This is the best setting for our hardware. You may need to manually configure **Threads** by pressing **alt+h+t** (fig 3) to obtain the best performance when CPU and GPU are used concurrently.

![Hardware Tab](https://hashsuite.openwall.net/tt_hardware_tab.png "Hardware Tab")  
**Fig 3: Hardware Tab.**

  
The general rules to configure this setting are:

-   If the GPUs performance is orders of magnitude higher than the CPU's, reduce **Threads** by the number of GPUs (our case).
-   If using an integrated GPU from Intel, put **Threads** to the maximum value (Logical cores).
-   In other cases experiment with values ranging from the maximum value to 0.

Next we need to obtain some big/quality wordlist. Press **alt+d+h** to open Hash Suite Downloader and select some wordlist(s) to download (fig 4) (Tip: In Hash Suite, pressing **F1** shows a context sensitive help, so if you do not know how to do something simply press **F1** and read the documentation). We will download and use in the tutorial the wordlist **wikipedia-wordlist-sraveau-20090325.txt.bz2** that contains words taken from Wikipedia.

![Downloader Tab](https://hashsuite.openwall.net/tt_downloader.png "Downloader Tab")  
**Fig 4: Select good wordlists to download.**

### Obtain hashes

To crack hashes we first need to obtain them. Normally you obtain the hashes from a local/remote machine; however, in this tutorial we will use hashes from password cracking contest **Crack Me If You Can 2010** (available from [here](https://contest-2010.korelogic.com/defcon_contest_hashes.txt)). These are publicly available hashes of realistic yet artificial passwords (so anyone can access them without concerns), and many of the hashes are of types used on Windows systems (and thus are supported by Hash Suite). The contest lasted 48 hours, which corresponds to a reasonable effort for us to spend as well, and in the end we can compare our results with those of contest participants. First import the hashes (**alt+f+i**) (fig 5).

![Import hashes](https://hashsuite.openwall.net/tt_import_hashes.png "Import hashes")  
**Fig 5: Import hashes.**

You will import **3380 LM**, **30640 NTLM**, **326 raw SHA1**, **10582 SSHA**, **4716 MD5CRYPT**, **80 BCRYPT** hashes (fig 6), excluding possible duplicate hashes (resulting from the same passwords seen more than once). In this tutorial we will focus on **LM** and **NTLM** hashes and superficially consider **SSHA** and **MD5CRYPT**.

![Imported Hashes](https://hashsuite.openwall.net/tt_num_hashes_imported.png "Imported Hashes")  
**Fig 6: Statistics of imported hashes.**

Let's begin our 2 days cracking quest.

### Cracking LM hashes

LM hashes were introduced in earlier versions of Windows and support for them continued in later versions for backwards compatibility, even though they were recommended by Microsoft to be turned off. As of Windows Vista, the protocol is disabled by default, but continues to be used by some non-Microsoft CIFS implementations. These hashes were very weak: we can crack **ANY** valid LM hash password within hours by **brute-force** (additional information regarding LM hashes may be found [here](https://en.wikipedia.org/wiki/LM_hash)).

We start with nothing cracked yet:

00h:00min

LM: Found 0/3380 **0%**

NTLM: Found 0/30640 **0%**

We will use the **Charset** key-provider, which is the default option (fig 7), and a range of password lengths from **0** to **6**, which is also the default. (You can see parameters on the left panel or by pressing **alt+p**.) So we only need to start the attack by pressing **alt+1** or clicking the **Start** button (we can pause/stop any attack by pressing **alt+2** or clicking the **Stop** button).

![Charset Selected](https://hashsuite.openwall.net/tt_lm_charset.png "Charset Selected")  
**Fig 7: Charset Selected.**

![Charset Params](https://hashsuite.openwall.net/tt_lm_7_without_symbols.png "LM Common")  
**Fig 8: Common passwords of length 7.**

00h:01min

LM: Found 1302/3380 **38%**

NTLM: Found 0/30640 **0%**

We then increase the password length to the maximum value for LM hashes: **7** and deselect the **Symbol** characters (fig 8). This will use only **Upper** and **Digit** characters, and will find common passwords first. Note that Hash Suite is smart enough not to use lower-case characters (which the LM hash algorithm would have converted to upper-case anyway) even if selected.

00h:02min

LM: Found 1663/3380 **49%**

NTLM: Found 0/30640 **0%**

Finally, **brute-force** all the remaining LM hash passwords by selecting **Symbol** and then starting the attack with **alt+1**.

00h:29min

LM: Found 3380/3380 **100%**

NTLM: Found 0/30640 **0%**

Voilà, we found all LM hash passwords in half an hour.

### Cracking NTLM hashes

NTLM is the successor of LM. It was introduced in Windows NT and it is still in use. First, select the NTLM hashes with **alt+m+f** (fig 9). Then, infer the case of characters of our cracked LM hash passwords: select the **LM2NT** key-provider (fig 10) and start the attack (**alt+1**), which should complete instantly.

![Select NTLM](https://hashsuite.openwall.net/tt_select_ntlm.png "Select NTLM")  
**Fig 9: Select NTLM hashes.**

![LM to NTLM](https://hashsuite.openwall.net/tt_lm2nt.png "LM to NTLM")  
**Fig 10: Convert LM passwords to NTLM correcting the case.**

![Keyboard](https://hashsuite.openwall.net/tt_keyboard.png "Keyboard")  
**Fig 11: Keyboard selected.**

![DB Info](https://hashsuite.openwall.net/tt_db_info.png "DB Info")  
**Fig 12: DB Info selected.**

00h:29min

LM: Found 3380/3380 **100%**

NTLM: Found 2556/30640 **8%**

### Quick Attacks

We begin with some quick (and productive) tests. Select **Keyboard**, keeping other options at their defaults (fig 11).

00h:29min

LM: Found 3380/3380 **100%**

NTLM: Found 2635/30640 **8%**

Now use **DB Info** with default options (fig 12). Since this is a very quick check, we will use this key-provider from time to time whenever we've found some new passwords by other means.

00h:30min

LM: Found 3380/3380 **100%**

NTLM: Found 2709/30640 **9%**

Use **Wordlist** (fig 13) with the file **wikipedia-wordlist-sraveau-20090325.txt.bz2** (the file we began to download at the preparing stage) as input. Note that the attack start is delayed some seconds while Hash Suite is compiling and optimizing rules for the GPU.

![Wordlist](https://hashsuite.openwall.net/tt_wordlist.png "Wordlist")  
**Fig 13: Wordlist selected.**

![Charset Selected](https://hashsuite.openwall.net/tt_ntlm_charset.png "Charset Selected")  
**Fig 14: Charset Selected.**

00h:31min

LM: Found 3380/3380 **100%**

NTLM: Found 6900/30640 **22%**

We will use the **Charset** (fig 14) key-provider with default options, which are: password length from **0** to **6** with all printable characters. Note that our password length settings were reset when switching to the NTLM format.

00h:33min

LM: Found 3380/3380 **100%**

NTLM: Found 7707/30640 **25%**

### Phrases

The popularity of passwords based on phrases has risen lately. Hash Suite provides a phrase generator with English words. Now let's use **Phrases** (fig 15) of 2 words with the most used English words.

00h:33min

LM: Found 3380/3380 **100%**

NTLM: Found 7967/30640 **26%**

![Phrases 2 words](https://hashsuite.openwall.net/tt_phrases.png "Phrases 2 words")  
**Fig 15: Phrases of 2 words with most used English words.**

![Phrases 3 words](https://hashsuite.openwall.net/tt_phrases_3_words.png "Phrases 3 words")  
**Fig 16: Phrases of 3 words with the 8000 most used words.**

Use **Phrases** (fig 16) of 3 words with the 8000 most used English words.

00h:37min

LM: Found 3380/3380 **100%**

NTLM: Found 7977/30640 **26%**

Use **Phrases** of 4 words with the 800 most used English words.

00h:40min

LM: Found 3380/3380 **100%**

NTLM: Found 7977/30640 **26%**

In this case **Phrases** do not crack a good number of passwords, so we give up with this source of words.

### Fingerprint

Fingerprint decompiles passwords into all possible parts or patterns ordered by use. Then you recombine them with **Phrases** creating common patterns many humans will choose. This is a powerful and simple attack to try apparently complicated passwords. Hash Suite provides a file with many common patterns ready to use. Just choose the file **fingerprint_common_pro.txt** and use **Phrases** of 2 patterns and one million maximum words to load (fig 17).

![Fingerprint Common](https://hashsuite.openwall.net/tt_fingerprint_source.png "Fingerprint Common")  
**Fig 17: Combination of 2 common patterns.**

![Fingerprint generation](https://hashsuite.openwall.net/tt_fingerprint_generation.png "Fingerprint generation")  
**Fig 18: Phrases of 3 words with the 8000 most used words.**

00h:42min

LM: Found 3380/3380 **100%**

NTLM: Found 9068/30640 **30%**

Use **Phrases** of 3 patterns with the 8000 most used patterns.

00h:45min

LM: Found 3380/3380 **100%**

NTLM: Found 9676/30640 **32%**

Once you have enough found passwords you may try to find patterns in them to launch a more specific fingerprint attack. **alt+p+f** (fig 18) generate a **fingerprint.txt** file with patterns from the found passwords. Click **Yes** to begin the attack.

00h:45min

LM: Found 3380/3380 **100%**

NTLM: Found 11737/30640 **38%**

Note that you can repeat this procedure again. Given that 2061 new passwords were found, new patterns will be generated resulting in more passwords found. Let's do the fingerprint again **(alt+p+f)** to test this.

00h:45min

LM: Found 3380/3380 **100%**

NTLM: Found 12588/30640 **41%**

Given that 852 new passwords were found, let's do it again **(alt+p+f)**.

00h:46min

LM: Found 3380/3380 **100%**

NTLM: Found 12947/30640 **42%**

Given that not so many new passwords were found let's try another idea. Let's once again use **Phrases** of 3 patterns with the 8000 most used patterns.

00h:49min

LM: Found 3380/3380 **100%**

NTLM: Found 14813/30640 **48%**

Given that some new passwords were found, let's do fingerprint again **(alt+p+f)**.

00h:50min

LM: Found 3380/3380 **100%**

NTLM: Found 15122/30640 **49%**

To see just how powerful fingerprint is, note that we almost double the found passwords with it in 10 minutes. And it is an almost automatic methodology.

### Time-Consuming Attacks

We finish our quick tests and move on to more time-consuming attacks. The most productive of these is **Wordlist** with a good wordlist (large, yet with common words) and **rules** enabled. Select the **Wordlist** (fig 13) key-provider and go to the rules tab (**alt+u** or simply use the left panel) to use more intensive rules. Select the **Less Common Rules** (fig 19), which you can easily do by inverting the selection with **alt+u+i**. Note that if you try to stop this attack you may need to wait some minutes before the attack actually stops. This is caused by an optimization and happens only when using **Rules on the GPU** with **SLOW** rules enabled (like **Word+3chars** and **3chars+Word**). In any other case the attack stops almost immediately.

![Uncommon rules enabled](https://hashsuite.openwall.net/tt_wordlist_uncommon.png "Uncommon rules enabled")  
**Fig 19: Uncommon rules enabled.**

03h:56min

LM: Found 3380/3380 **100%**

NTLM: Found 18748/30640 **61%**

Let's try **Charset** (fig 14) with password length **7** and all printable characters.

05h:56min

LM: Found 3380/3380 **100%**

NTLM: Found 19684/30640 **64%**

### Patterns

It is time to move on to more intelligent cracking and try to find patterns in the found hashes. We can sort the accounts by **Cleartext** clicking twice in the header (fig 20). Then we can manually cycle through the pages trying to find patterns. There are some easily seen patterns like:

-   concatenated numerals (e.g., "onetwo") and multi-word numerals written as one word (e.g., "ninebillion")
-   the word **Number** followed by 1 to 4 digits
-   permutations of names of days of the week
-   permutations of month names
-   permutations of seasons
-   permutations of the following words: **whitehat**, **blackhat**, **lasvegas**, **vegas**, **korelogic**, **defcon**, **hello**, **facebook**

![Patterns](https://hashsuite.openwall.net/tt_one_pattern.png "Patterns")  
**Fig 20: One common pattern.**

![Patterns Params](https://hashsuite.openwall.net/tt_patterns_wordlist.png "Patterns Params")  
**Fig 21: Use the generated pattern wordlist.**

We can create a program or script to generate a wordlist with these patterns and then use it in Hash Suite. We choose to make a simple [C++ program](https://hashsuite.openwall.net/Pattern_Generation.zip). This program generates a wordlist named **patterns.txt** with a size of 1.5GB that we can load in Hash Suite with **alt+p+w** (fig 21) (or with the left side panel). Then we select the **Wordlist** key-provider (fig 13) and select all rules (**alt+u+m**) except **Word+3chars** and **3chars+Word**.

06h:06min

LM: Found 3380/3380 **100%**

NTLM: Found 20345/30640 **66%**

There is also an easy pattern of **person names** with [leetspeak](https://en.wikipedia.org/wiki/Leet) transformation. We can exploit it by downloading the wordlist **facebook-names-unique.txt.bz2** and applying leet rules. We leave this pattern for readers of this tutorial to explore on their own.

### Salted Hashes

Let's make a quick stop at **SSHA** and **MD5CRYPT** hashes and how to crack them, given that there are some differences with the hash types we tried cracking so far. These are [salted hashes](https://en.wikipedia.org/wiki/Salt_(cryptography)), meaning an expected-unique value (normally random and called **[salt](https://en.wikipedia.org/wiki/Salt_(cryptography))**) is added to the hash computation. This causes the need to test each key _for each_ different salt, effectively reducing the performance of the attack by the number of salts used. Note that performance of attack on _one_ salted hash is similar to that of attack on a non-salted hash; it's only when many hashes are attacked the use of salts strengthens the security of hashes. What this means is that we need to use more efficient/intelligent methods to attack salted hashes.

### SSHA

Let's begin with **SSHA** and a **Wordlist** key-provider (fig 13) without rules enabled. We will try all our wordlists sorted by size. Begin with **wordlist_small.lst**.

06h:06min

SSHA: Found 3/10582 **0%**

MD5CRYPT: Found 0/4716 **0%**

Then use **lower.gz**.

06h:06min

SSHA: Found 20/10582 **0%**

MD5CRYPT: Found 0/4716 **0%**

Finish with **wikipedia-wordlist-sraveau-20090325.txt.bz2**.

06h:12min

SSHA: Found 357/10582 **3%**

MD5CRYPT: Found 0/4716 **0%**

Let's try **DB Info** key-provider without rules enabled.

06h:12min

SSHA: Found 565/10582 **5%**

MD5CRYPT: Found 0/4716 **0%**

Let's finish with **Phrases** key-provider without rules enabled. Use a fingerprint attack with 2 patterns from source **fingerprint_common_pro.txt** and the 7000 most used patterns.

06h:17min

SSHA: Found 622/10582 **5%**

MD5CRYPT: Found 0/4716 **0%**

Use a fingerprint attack **(alt+p+f and select no)** with 2 patterns from source **fingerprint.txt** and the 7000 most used patterns.

06h:21min

SSHA: Found 1618/10582 **15%**

MD5CRYPT: Found 0/4716 **0%**

### MD5CRYPT

Let's move to **MD5CRYPT** and use the same strategy as we did with **SSHA**. Begin with a **Wordlist** key-provider (fig 13) without rules enabled. We will try all our wordlists sorted by size. First is **wordlist_small.lst**.

06h:21min

SSHA: Found 1618/10582 **15%**

MD5CRYPT: Found 583/4716 **12%**

Then use **lower.gz**.

06h:26min

SSHA: Found 1618/10582 **15%**

MD5CRYPT: Found 764/4716 **16%**

We don't use **wikipedia-wordlist-sraveau-20090325.txt.bz2** as it is very large for the performance of the attack with this number of MD5CRYPT hashes. Let's try **DB Info** key-provider without rules enabled.

06h:27min

SSHA: Found 1618/10582 **15%**

MD5CRYPT: Found 822/4716 **17%**

Let's finish with **Phrases** key-provider without rules enabled. Use a fingerprint attack with 2 patterns from source **fingerprint_common_pro.txt** and the 650 most used patterns.

06h:32min

SSHA: Found 1618/10582 **15%**

MD5CRYPT: Found 858/4716 **18%**

Use a fingerprint attack **(alt+p+f and select no)** with 2 patterns from source **fingerprint.txt** and the 650 most used patterns.

06h:37min

SSHA: Found 1618/10582 **15%**

MD5CRYPT: Found 924/4716 **19%**

With this we finish our quick tour of salted hashes and how to approach them. We go back to **NTLM** hashes for the rest of the tutorial.

### Custom charsets

We have enough time left that we can employ "smart" **brute-force**. We plan what we will do for password length from **8** and up. Given a speed of 9.60 billion hashes/second, we calculate the number of different characters to try assuming that we want to spend **10 hours** on each candidate password length:

Password Length

Charset Length1

Expected Coverage2

Passwords Found3

8

65

59%

6600

9

41

26%

5800

10

28

7%

2100

11

21

1%

721

12

16

0%

686

1 _Calculated as **(10 hours * 60*60 * 9.60*10****9**)**1 / (Password Length)**_  
2 _The percent of passwords that will be found if you use_ **Charset Length** _most used characters as a charset. For example with the **65** most used characters we expect (as this is based on already found passwords) to_ **cover** _**59%** of the passwords. Hash Suite calculates this metric for you (press **alt+p+a** and enter the number of characters)_.  
3 _Use the **Hashes_Found** report (**alt+r**) to obtain it_.  

It is pretty clear we _expect_ to maximize found passwords using password lengths **8** and **9**. We distribute the remaining 41 hours between these two lengths proportionally to the **Coverage**, giving us 30 hours for length 8 and 11 hours for length 9. (Hash Suite might automate this analysis and length distribution in a future version.)

**(30 hours * 60*60 * 9.60*109)1/8 = 75  
(11 hours * 60*60 * 9.60*109)1/9 = 42  
**

Add a new charset with the **75** most used characters: press **alt+p+a** and enter **75** as the **Number of characters** (fig 22). Start a **Charset** attack (fig 14) with password length **8** and the 75 most used characters as charset (fig 23).

![75 most used characters](https://hashsuite.openwall.net/tt_75_characters_added.png "75 most used characters")  
**Fig 22: 75 most used characters.**

![Params for 75 most used characters](https://hashsuite.openwall.net/tt_charset_8.png "Params for 75 most used characters")  
**Fig 23: Params for 75 most used characters.**

34h:53min

LM: Found 3380/3380 **100%**

NTLM: Found 23341/30640 **76%**

Add a new charset with the **44** most used characters (**alt+p+a** and enter **44** as the **Number of characters**). Start a **Charset** attack (fig 14) with password length **9** and the 44 most used characters as charset. Stop the attack when you approach 12 and a half hours of cracking time.

47h:23min

LM: Found 3380/3380 **100%**

NTLM: Found 24023/30640 **78%**

### Round-up

Given that some new passwords were found, let's do fingerprint again **(alt+p+f)**.

47h:24min

LM: Found 3380/3380 **100%**

NTLM: Found 24311/30640 **79%**

Finish our cracking with **DB Info** (fig 13) with all rules enables (**alt+u+m**).

47h:25min

LM: Found 3380/3380 **100%**

NTLM: Found 24403/30640 **79%**

### Conclusions

In our 2 day cracking quest we found **79%** of all passwords. In a real environment we expect more than **90%**, but this is a contest.

How good is this? We crack **2360** LM, **24576** NTLM, **1618** SSHA, and **924** MD5CRYPT hash passwords (**alt+v+s** and see **Matches**; the difference is because there are some accounts that share the same password). We score **29478** and would end up **[4th](https://contest-2010.korelogic.com/stats.html)** of the 18 teams that participated in the contest. Note that we focus on only 2 types of hashes (**LM** and **NTLM**; **SSHA** and **MD5CRYPT** were only superficially touched) out of the 8 types given by the contest organizers, and we only had one PC system, whereas high-scored teams had multiple members and used multiple machines. On the other hand, Hash Suite 3.4 and the GTX 970 graphics card were not yet available in 2010 (when the contest occurred).

### Fixing Weak Accounts

Cracking passwords may be fun, but each cracked password is a weak password that represents a security risk. Hash Suite Pro can help to mitigate this risk disabling the account or forcing the user to change the weak password, with **alt+f+a** (fig 24). This only works when you import the accounts from a local/remote machine (not from a file).

![Fixing weak accounts](https://hashsuite.openwall.net/tt_fix_accounts.png "Fixing weak accounts")  
**Fig 24: Fixing weak accounts.**

### Analyzing

Hash Suite Pro Reports (**alt+r**) help you with the analysis of the data you obtain. Below are some graphs from the reports [Attacks](https://hashsuite.openwall.net/Attacks.pdf) and [Hashes_Found](https://hashsuite.openwall.net/Hashes_Found.pdf).

![Time by Key-Provider](https://hashsuite.openwall.net/tt_graph_time_provider.png "Time by Key-Provider")  
**Fig 25: Time by Key-Provider.**

![Guesses by Key-Provider](https://hashsuite.openwall.net/tt_graph_guesses.png "Guesses by Key-Provider")  
**Fig 26: Guesses by Key-Provider.**

**Phrases** and **Rules** (with **Wordlist** and **DB Info**) are the most productive key-providers with **59%** of found password and only **8%** of time consumed.

![Passwords cracked by Time](https://hashsuite.openwall.net/tt_graph_cracked_by_time.png "Passwords cracked by Time")  
**Fig 27: Passwords cracked by Time.**

In the first hours we can expect to crack **3/4** of all passwords found.

![Passwords cracked by Length](https://hashsuite.openwall.net/tt_graph_found_by_len.png "Passwords cracked by Length")  
**Fig 28: Passwords cracked by Length.**

Users normally select passwords with less than 10 characters that are easily crackable.

Hash Suite is a very fast and simple (yet powerful) password cracker that can help keep your organization users' passwords safe. We hope that with this tutorial Hash Suite use will be simpler to a broad number of customers.


