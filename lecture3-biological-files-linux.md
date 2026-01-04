# Lecture 3 — Working with Biological Files (FASTA, GTF) using Linux
Lecture 3 – Command Index (Linux + Biological Files)
# 1. Viewing Biological Files Safely

cat

less

head

head -n

tail

sed -n '1,50p'

# 2. Understanding FASTA Structure

grep "^>"

grep -c "^>"

grep -v "^>"

# 3. Counting and Measuring Sequences

wc

wc -l

wc -m

wc -w

# 4. Searching Patterns in Biological Files

grep

grep -n

grep -o

grep --color

grep -v

grep -c

# 5. Regex Concepts Used in Bioinformatics

^ (start of line)

[^ATGC] (negation inside character class)

. (any character)

* and + (repetition)

[ ] (character sets)

# 6. Case Normalization of Sequences

tr 'a-z' 'A-Z'

# 7. Pipelines and Data Flow

|

Command chaining with pipes

# 8. FASTA Quality Control Pipelines

grep -v "^>" file.fasta

grep -v "^>" file.fasta | wc

grep -v "^>" file.fasta | tr 'a-z' 'A-Z'

grep -v "^>" file.fasta | grep "[^ATGC]"

# 9. GTF / Annotation File Handling

Understanding tab-separated columns

Feature filtering (gene, transcript, exon)

# 10. Extracting Fields from GTF Files

awk -F'\t'

$1, $3, $4, $5, $9

Conditional filtering in awk

# 11. Transcript and Gene ID Extraction

grep 'gene_id'

grep 'transcript_id'

grep -oP 'transcript_id "\K[^"]+'

sort

uniq

# 12. Redirecting Output

>

>>

2>

Combining stdout and stderr

# 13. Creating Derived Files

Saving filtered GTF files

Creating intermediate outputs for downstream steps

# 14. Extracting Specific Sequences

Header-based extraction logic

Printing until next FASTA header using awk

# 15. Biological Reasoning with Linux Tools

Linking FASTA ↔ GTF

Understanding transcripts vs genes

Mapping sequence entries to annotations

# 16. Common Errors Encountered (Learning Moments)

Forgetting file name in grep

Misunderstanding ^ in regex

Using cat on large files

Confusing headers vs sequences

Running commands in wrong directory

# 17. Where This Appears in Real Pipelines 🧪

FASTA validation

Pre-alignment QC

Transcript-level analysis

Annotation filtering

Debugging corrupted sequence files
## Purpose

Hands-on Linux commands and concepts for inspecting and processing biological text files (FASTA, GTF).  
This serves as a concise, reusable reference for real bioinformatics pipelines.

---

## Index

## 1️⃣ FASTA format  
## 2️⃣ GTF format  
## 3️⃣ Viewing files safely  
## 4️⃣ Key commands (each command is a heading)  
## 6️⃣ Mistakes & follow-up questions  
## 7️⃣ Where this appears in real pipelines  
## at last are interview questions
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
“In bioinformatics pipelines, I use grep for fast filtering, sed for line-level extraction or cleanup, and awk when working with structured annotation files like GTFs where column logic matters.”
---

---------------

# Validate DNA alphabet
Detect invalid bases (case-insensitive).
tr 'a-z' 'A-Z'
Meaning
Converts lowercase bases to uppercase
Why this matters:
FASTA files are often mixed case
You want consistent checking (a vs A)

"I use this pipeline to strip FASTA headers, normalize sequence case, and detect non-canonical nucleotides. The key is understanding that ^ acts as a line anchor outside brackets and a negation operator inside character classes. This lets me reliably QC sequence integrity before downstream analysis.”
~~~
grep -v "^>" tb1.fasta | tr 'a-z' 'A-Z' | grep --color -n "[^ATGC]"
~~~

---

## 6️⃣ Mistakes & Follow-up Questions

### Common Mistakes
- Used `cat` on large files → terminal flooded  
- Forgot quotes in `grep 'gene_id "X"'`  
- Tried Ctrl+X instead of Ctrl+C for terminal flooding 
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

Interview Questions – Lecture 3 (Linux + Biological Files)
1. How do you quickly validate whether a FASTA file is biologically clean?

A FASTA file should contain headers starting with > and sequence lines with valid nucleotide or amino acid characters. I usually remove headers first and then scan the remaining sequence for invalid characters using pattern matching. This ensures the file won’t silently break downstream tools like aligners or assemblers.

In practice, this check helps catch corrupted downloads or mixed-format files early in the pipeline.

2. Why do you always remove FASTA headers before sequence-level analysis?

Headers are metadata, not biological sequence. If headers are included while counting bases or validating alphabets, results become meaningless.
Separating metadata from biological signal is essential to maintain correctness in analysis.

This mirrors real pipelines where metadata and data are processed in different stages.

3. Explain the biological meaning of this pattern: [^ATGC]

This pattern identifies any character that is NOT A, T, G, or C.
Biologically, it highlights ambiguous bases (like N) or sequencing/artifact errors.

From a QC perspective, it flags positions that may affect alignment accuracy or variant calling reliability.

4. Why is case normalization important in sequence analysis?

Many real FASTA files mix lowercase and uppercase letters depending on masking or source.
Normalizing case ensures downstream tools interpret bases consistently.

This is especially important before regex-based validation or statistics calculation.

5. What is the advantage of using pipes (|) in biological data processing?

Pipes allow data to flow between tools without writing intermediate files, which improves speed, memory efficiency, and reproducibility.

Most modern bioinformatics pipelines (RNA-seq, ChIP-seq, WGS) are essentially chains of piped transformations.

6. How would you count the number of sequences in a FASTA file?

Each sequence in FASTA begins with a header line (>).
Counting headers gives the number of sequences.

This approach is scalable and works for files of any size.

7. Why should cat be avoided for large biological files?

cat loads the entire file into the terminal, which is slow and unsafe for large datasets.
Tools like less, head, or sed allow controlled inspection without overwhelming memory or the terminal.

This matters when working with multi-GB sequencing files.

8. What problem does less solve in real bioinformatics workflows?

less allows interactive exploration of large files without loading them entirely into memory.
It is essential for inspecting FASTA, GTF, BAM headers, or log files safely.

This becomes critical on shared compute servers.

9. How do you extract biological meaning from a GTF file using Linux tools?

A GTF file is structured and tab-delimited.
By filtering specific feature types (like exons) and extracting coordinates and attributes, we can reconstruct transcript structure and genomic context.

This bridges raw annotation files with downstream sequence extraction.

10. Why is column-based filtering important in annotation files?

Each column in GTF has semantic meaning (chromosome, feature type, coordinates).
Filtering by column ensures biologically correct selection instead of unreliable string matching.

This prevents subtle but serious annotation errors.

11. How would you extract all transcript IDs for a gene?

Transcript IDs are embedded in the attributes column.
Using pattern-based extraction allows clean retrieval of identifiers without parsing the entire file manually.

This is commonly required for transcript-level quantification and isoform analysis.

12. What is the difference between filtering and extracting in grep?

Filtering decides which lines to keep, while extracting pulls out specific parts of those lines.
Both are useful, but extraction is required when converting raw annotations into structured identifiers.

Understanding this distinction avoids incorrect data summaries.

13. Why is sorting and de-duplication necessary after extraction?

Annotation files often contain repeated entries for the same transcript or gene across multiple features.
Sorting and removing duplicates ensures clean, non-redundant identifier lists.

This directly affects downstream counts and joins.

14. How do you connect FASTA files with GTF annotations conceptually?

FASTA contains sequences; GTF describes where those sequences originate in the genome.
By linking IDs and coordinates, we map biological meaning onto raw sequences.

This relationship underpins transcriptomics and genome annotation.

15. What kind of bugs does sequence validation prevent?

It prevents:

alignment failures

incorrect GC content calculations

silent truncation

downstream tool crashes

Early validation saves hours of debugging later.

16. How does Linux text processing enable reproducible bioinformatics?

Every command is explicit, scriptable, and auditable.
This makes analyses reproducible, reviewable, and shareable — unlike GUI-only workflows.

This is crucial in regulated or collaborative environments.

17. How would you explain your Linux skills to a data science interviewer?

I treat biological files as structured text streams.
Linux tools let me validate, transform, and audit data before modeling or statistical analysis.

This mindset ensures data quality before any downstream inference.

18. How is this relevant to companies like Elucidata?

Elucidata works with large, heterogeneous biological datasets.
Linux-based validation and preprocessing ensure data is consistent, traceable, and AI-ready.

Strong fundamentals reduce downstream model noise.

19. What signals senior-level thinking in Linux usage?

Understanding why a command is used, not just how.
Knowing what biological or computational assumption each command enforces.

This prevents accidental misuse of tools.

20. If you forgot everything, what one principle would you keep?

Separate metadata from biological signal and validate early.
Everything else builds on that.
End of Lecture 3 notes.
