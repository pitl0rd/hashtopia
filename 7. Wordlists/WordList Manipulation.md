
A collection of practical command-line snippets for cleaning and preparing wordlists during processing.

---

## Remove spaces
#sed
Remove **all** space characters:

```bash
sed -i 's/ //g' file.txt
```

Remove **blank/whitespace-only** lines:

```bash
egrep -v "^[[:space:]]*$" file.txt
```

---

## Remove the last character on each line
#sed
```bash
sed -i 's/.$//' file.txt
```

---

## Remove blank lines
#sed
```bash
sed -i '/^$/d' list.txt
```

---

## Remove a specific character

Example: remove single quotes (`'`):
#sed
```bash
sed -i "s/'//g" file.txt
```

---

## Delete a substring with sed

Example: remove the literal string `foo`:

```bash
echo 'This is a foo test' | sed 's/foo//g'
```

---

## Replace characters
#sed
Replace `@` with `#` using `tr`:

```bash
tr '@' '#' < emails.txt > emails_out.txt
```

Or with `sed`:

```bash
sed 's/@/#/g' file.txt > out.txt
```

---

## Print specific columns from CSV
#awk
Using `awk` (3rd column, comma-delimited):

```bash
awk -F "," '{print $3}' infile.csv > outfile.csv
```

Using `cut`:

```bash
cut -d "," -f 3 infile.csv > outfile.csv
```

**Note:** to output all columns from column `X` onward:

```bash
cut -d "," -f X- infile.csv > outfile.csv
```

---

## Generate random candidates with `/dev/urandom`

Examples:

```bash
tr -dc 'a-zA-Z0-9._!@#$%^&*()' < /dev/urandom | fold -w 8  | head -n 500000 > wordlist.txt
tr -dc 'a-zA-Z0-9-_!@#$%^&*()_+{}|:<>?=' < /dev/urandom | fold -w 12 | head -n 4
base64 /dev/urandom | tr -cd '[:alnum:]' | cut -c1-10 | head -n 2
tr -dc 'a-zA-Z0-9' < /dev/urandom | fold -w 10 | head -n 4
tr -dc '[:print:]' < /dev/urandom | fold -w 10 | head -n 10
tr -cd '[:alnum:]' < /dev/urandom | fold -w 30 | head -n 2
```

Require at least one symbol (example filter):

```bash
tr -dc 'a-zA-Z0-9-_!@#$%^&*()_+{}|:<>?=' < /dev/urandom | fold -w 12 | head -n 1000 | grep -E '[!@#$%^&*()_+{}|:<>?=]'
```

---

## Remove parentheses

```bash
tr -d '()' < in_file > out_file
```

---

## Convert case

Upper → lower:

```bash
tr 'A-Z' 'a-z' < file.txt > lower-case.txt
```

Lower → upper:

```bash
tr 'a-z' 'A-Z' < file.txt > upper-case.txt
```

---

## Generate a wordlist from filenames

```bash
ls -A > filenames.txt
```

If you want to apply a transform, run it through `sed` afterward (example: strip an extension):

```bash
ls -A | sed 's/\.[^.]*$//' > filenames_no_ext.txt
```

---

## Clean “weird characters” when tools choke

A safer default for normalizing to printable characters:

```bash
tr -cd '[:print:]\n' < infile.txt > cleaned.txt
```

---

## Filter by length
#awk
Only lines of exactly length 10:

```bash
awk 'length($0) == 10' file.txt > 10-length.txt
```

---

## Sort a wordlist by length
#awk
```bash
awk '{print length($0), $0}' rockyou.txt | sort -n | cut -d " " -f2- > rockyou_length_list.txt
```

---

## Merge two files line-by-line

```bash
paste -d' ' file1.txt file2.txt > new-file.txt
```

---

## Faster sorting (large files)

```bash
export LC_ALL=C
sort --parallel=<number_of_cpu_cores> -S <amount_of_memory>G -u file.txt > new-file.txt
```

---

## Line ending conversion

Mac classic (CR) → Unix (LF):

```bash
tr '\015' '\012' < in_file > out_file
```

DOS → Unix:

```bash
dos2unix file.txt
```

Unix → DOS:

```bash
unix2dos file.txt
```

---

## Remove lines from one file that appear in another

Remove entries in `file1.txt` from `file2.txt`:

```bash
grep -Fvx -f file1.txt file2.txt > file3.txt
```

---

## Extract a line range
#sed
Lines 1–100:

```bash
sed -n '1,100p' test.file > file.out
```

---

## Create a wordlist from a PDF
#tools
https://github.com/spatie/pdf-to-text
```bash
pdftotext file.pdf file.txt
```

---

## Find line numbers for matching text
#awk 
```bash
awk '{print NR, $0}' file.txt | grep "string-to-grep"
```

---
[[Wordlists and Dictionaries]]
[[Home]]