# Lecture 3 — Working with Biological Files (FASTA, GTF) using Linux

## Purpose

Hands-on Linux commands and concepts for inspecting and processing biological text files (FASTA, GTF).  
This serves as a concise, reusable reference for real bioinformatics pipelines.

---

## Index

## 1️⃣ FASTA format  
## 2️⃣ GTF format  
## 3️⃣ Viewing files safely  
## 4️⃣ Key commands (each command is a heading)  
## 5️⃣ Examples for harder commands  
## 6️⃣ Mistakes & follow-up questions  
## 7️⃣ Where this appears in real pipelines  

---

## 1️⃣ FASTA format

FASTA stores biological sequences.
- 2-line record structure—a header starting with > followed by the sequence.
- Lines starting with `>` are **headers / identifiers**
- Following lines are         **sequence data**
- One header corresponds to   **one sequence entry**

👉 Counting headers = counting sequences.
-fasta is different from fastq as it does not have quality score
---

## FASTQ FORMAT
--   Stores both sequence data and base-call quality scores (Phred scores).
--    FASTQ: Standardized to a 4-line record structure:
--   @ followed by the Sequence ID.
--  The raw biological sequence.
--  + (separator, optionally repeating the ID).
-- Quality scores encoded in ASCII characters.

## 2️⃣ GTF format

GTF is a **tab-separated annotation format**.

- Each line represents a genomic feature (gene, transcript, exon)
- Column 9 contains attributes like:
  - `gene_id`
  - `transcript_id`
- Attribute searches **require exact quoting**

---

## 3️⃣ Viewing files safely

Never `cat` very large biological files.

Safe inspection tools:
- `less`
- `head`
- `tail`
- stream processing with pipes

---

## 4️⃣ Key Commands

# `grep ">" flank.fasta`
Show FASTA headers (identifiers only).

~~~
grep ">" flank.fasta
~~~

---

# `grep -c ">" flank.fasta`
Count number of FASTA entries (one header = one sequence).

~~~
grep -c ">" flank.fasta
~~~

---

# `grep -v ">" flank.fasta`
Print only sequence lines (exclude headers).
-v inverted matching krata h
~~~
grep -v ">" flank.fasta
~~~

---

# `grep -v ">" flank.fasta | wc`
Count lines, words, and characters in sequence-only output.

~~~
grep -v ">" flank.fasta | wc
~~~

---

# `grep -v ">" flank.fasta | wc -l`
Count only sequence lines.

~~~
grep -v ">" flank.fasta | wc -l
~~~

---

# `grep -v ">" flank.fasta | wc -m`
Count total characters (bases).

~~~
grep -v ">" flank.fasta | wc -m
~~~

---

# `grep -v ">" flank.fasta | wc -w`
Count words (useful when data is tokenized).

~~~
grep -v ">" flank.fasta | wc -w
~~~

-------------------------------------------------------------------------------------

# `grep 'gene_id "ENSMUSG00000020122"' Mus_musculus.GRCm38.75_chr1.gtf`
Extract all annotation lines for a specific gene.

~~~
grep 'gene_id "ENSMUSG00000020122"' Mus_musculus.GRCm38.75_chr1.gtf
~~~

---

# `grep -o 'gene_id "ENSMUSG00000020122"' Mus_musculus.GRCm38.75_chr1.gtf`
Print only the matched attribute (format verification).

~~~
grep -o 'gene_id "ENSMUSG00000020122"' Mus_musculus.GRCm38.75_chr1.gtf
~~~

---

# `grep -c 'gene_id "ENSMUSG00000020122"' Mus_musculus.GRCm38.75_chr1.gtf`
Count how many GTF lines reference the gene.

~~~
grep -c 'gene_id "ENSMUSG00000020122"' Mus_musculus.GRCm38.75_chr1.gtf
~~~

---

# `grep --color "[^ATGC]" tb1.fasta`
Highlight invalid DNA characters.
^ symbol is for ignore okayyyy


~~~
grep --color "[^ATGC]" tb1.fasta
~~~

---

# `head tb1.fasta`
View the first 10 lines (sanity check).

~~~
head tb1.fasta
~~~

---

# `head -n 2 Mus_musculus.GRCm38.75_chr1.gtf`
Check file format or headers. first 2 lines

~~~
head -n 2 Mus_musculus.GRCm38.75_chr1.gtf
~~~

---

# `tail -n 2 Mus_musculus.GRCm38.75_chr1.gtf`
Check file termination and trailing content.

~~~
tail -n 2 Mus_musculus.GRCm38.75_chr1.gtf
~~~

---

# `less Mus_musculus.GRCm38.75_chr1.gtf`
Safely browse large annotation files.  
Exit with `q`.

~~~
less Mus_musculus.GRCm38.75_chr1.gtf
~~~

---

# `cat small_file.fasta`
Print contents (only for small files).

~~~
cat small_file.fasta
~~~

---

# `cat tb1-protein.fasta tb1.fasta > combines.fasta 2> combines.stderr`
Concatenate files and capture errors separately.
files which show error will go to 2> iske baad jo bhi file name hoga
~~~
cat tb1-protein.fasta tb1.fasta > combines.fasta 2> combines.stderr
~~~

---

# `grep ">" flank.fasta | less`
Inspect many FASTA headers safely.

~~~
grep ">" flank.fasta | less
~~~

---

# `grep -v "^>" tb1.fasta | grep --color "[^ATGC]"`
Validate DNA alphabet after removing headers.
checking whether any characters other than A, T, G, or C are present in the sequence lines
^> matches lines starting with >
-v means “invert match”
Result: only sequence lines remain

^ inside brackets means “NOT” [^ATGC]
[^ATGC] matches any character that is not A, T, G, or C


WHY ^ SYMBOL MEANING KEEPS ON CHANGING
Interview-level explanation (say this confidently)

“In regular expressions, the caret has context-dependent meaning.
Outside character classes it anchors the start of a line, while inside character classes it negates the character set.”

| Pattern  | Meaning                         |
| -------- | ------------------------------- |
| `^>`     | Line starts with `>`            |
| `[^>]`   | Any character except `>`        |
| `^ATG`   | Line starts with `ATG`          |
| `[^ATG]` | Any character except A, T, or G |

~~~
grep -v "^>" tb1.fasta | grep --color "[^ATGC]"
~~~

---

# `grep -c "^>" tb1.fasta`
Count FASTA entries using strict header matching.

~~~
grep -c "^>" tb1.fasta
~~~

---

# `wc -l flank.fasta`
Count total lines in a file.

~~~
wc -l flank.fasta
~~~

---

# `find . -name "*.fastq"`
Locate FASTQ files in the project tree.

~~~
find . -name "*.fastq"
~~~

---
✨✨✨✨✨GOOD ONE DO SEE HARD 
# `grep -oP 'transcript_id "\K[^"]+' Mus_musculus.GRCm38.75_chr1.gtf | sort | uniq`
Extract unique transcript IDs.
EXPLAINATION⬇️⬇️⬇️
https://chatgpt.com/s/t_695a91c0dcd4819190ec77bfc5dbec8c


~~~
grep -oP 'transcript_id "\K[^"]+' Mus_musculus.GRCm38.75_chr1.gtf | sort | uniq
~~~

---
✨✨✨✨✨
# `awk -F'\t' '$3=="exon" {print $1 ":" $4 "-" $5 "\t" $9}' Mus_musculus.GRCm38.75_chr1.gtf | head`
Extract exon coordinates using column-aware parsing.
EXPLAINATION⬇️⬇️⬇️
https://chatgpt.com/s/t_695a94e457688191b473138aaba155d0

~~~
awk -F'\t' '$3=="exon" {print $1 ":" $4 "-" $5 "\t" $9}' Mus_musculus.GRCm38.75_chr1.gtf | head
~~~

---

# `sed -n '1,50p' file.fasta`
Print a specific line range without opening the whole file.
https://chatgpt.com/s/t_695a9963578c819188a2b26db0c712c0

sed provides precise range control of viewing which head does not provide and sed is good for large files preview

~~~
sed -n '1,50p' file.fasta
~~~

---

## 5️⃣ Examples for Harder Commands

# Count sequences and average sequence length
Explanation: count headers, compute total bases, divide.

~~~
num_seq=$(grep -c "^>" flank.fasta)
total_bases=$(grep -v "^>" flank.fasta | tr -d '\n' | wc -m)
echo "num_seq=$num_seq total_bases=$total_bases"
awk "BEGIN{print total_bases/num_seq}"
~~~

---

# Extract sequences for a single transcript
Print header + sequence until the next header.

~~~
awk '/^>ENSMUSG00000020122\|ENSMUST00000138518/{print;flag=1;next} /^>/{flag=0} flag{print}' flank.fasta
~~~

---

# Validate DNA alphabet
Detect invalid bases (case-insensitive).

~~~
grep -v "^>" tb1.fasta | tr 'a-z' 'A-Z' | grep --color -n "[^ATGC]"
~~~

---

## 6️⃣ Mistakes & Follow-up Questions

### Common Mistakes
- Used `cat` on large files → terminal flooded  
- Forgot quotes in `grep 'gene_id "X"'`  
- Tried Ctrl+X instead of Ctrl+C  
- Confused `q` (less) with stopping a command  

---

### Why did `grep` fail without quotes?
Shell splits tokens; quoting preserves the exact pattern.

### Why count headers for FASTA entries?
Each `>` defines one biological record.

### How to inspect very large files safely?
Use `less`, `head`, `tail`, streaming filters.

---

## 7️⃣ Where This Appears in Real Pipelines

- FASTA / FASTQ QC before alignment
- Extracting gene/transcript annotations
- Streaming filters to avoid large intermediates
- Preprocessing steps in Nextflow / Snakemake
- Capturing errors with `2>` during automation

---

End of Lecture 3 notes.
