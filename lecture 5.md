
# Lecture 5: Unix Data Tools for Genomics Files
## Files Used in This Lecture

* `.bed` → genomic intervals
* `.gtf` → gene annotations
* `.fasta / .fastq` → sequences
* `.txt` → gene lists, lengths, names

---

## SECTION 1: Viewing Large Genomics Files Safely

---

# `head`

Shows the first lines of a file.

Used to:

* inspect file format
* confirm columns
* avoid opening massive files fully

```
head Mus_musculus.GRCm38.75_chr1.bed
```

---

# `tail`

Shows the last lines of a file.

Used to:

* check file completeness
* verify sorting or truncation

```
tail Mus_musculus.GRCm38.75_chr1.bed
```

---

# `head` and `tail` together

View **top + bottom** without opening full file.

```
(head -n 3; tail -n 3) < Mus_musculus.GRCm38.75_chr1.bed
```

---

🧪 Where This Appears in Real Pipelines
Before running:

* `bedtools`
* `bwa`
* `featureCounts`

You *always* inspect files first.

---

## SECTION 2: Column-Based Extraction with `cut`

---

# `cut`

Extracts specific columns from tab-separated files.

GTF and BED are **tab-delimited**, not space-delimited.

```
cut -f 1,2 Mus_musculus.GRCm38.75_chr1.bed
```

Explanation:

* `-f` → fields (columns)
* default delimiter = TAB

---

# `cut` on GTF

```
cut -f 1,4,5 Mus_musculus.GRCm38.75_chr1.gtf
```

Used to:

* extract chromosome
* start and end positions

---

## SECTION 3: Ignoring Metadata Lines with `grep -v`

---

# `grep -v "^#"`

Remove comment lines from GTF.

```
grep -v "^#" Mus_musculus.GRCm38.75_chr1.gtf
```

Explanation:

* `^` → start of line
* `#` → comment marker
* `-v` → invert match (remove them)

---

🧪 Where This Appears in Real Pipelines
Required before:

* `awk`
* `cut`
* `bedtools`
  on GTF files

---

## SECTION 4: Searching Gene IDs with `grep`

---

# Basic gene search

```
grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf
```

Finds all annotation lines belonging to a gene.

---

# Count gene occurrences

```
grep 'gene_id "ENSMUSG00000025907"' Mus_musculus.GRCm38.75_chr1.gtf | wc -l
```

Used to:

* estimate number of exons / transcripts
* sanity-check annotations

---

## SECTION 5: Working with Gene Lists

---

# View gene list file

```
head Mus_musculus.GRCm38.75_chr1_genes.txt
```

Format:

```
ENSMUSG00000051951   Xkr4
ENSMUSG00000025900   Rp1
```

---

# Search by gene name

```
grep Olfr Mus_musculus.GRCm38.75_chr1_genes.txt
```

Finds olfactory receptor genes.

---

# Highlight matches

```
grep --color=always Olfr Mus_musculus.GRCm38.75_chr1_genes.txt
```

---

## SECTION 6: Regular Expressions with `grep`

---

# Character classes

```
grep "Olfr14[13]" Mus_musculus.GRCm38.75_chr1_genes.txt
```

Matches:

* Olfr141
* Olfr143

---

# OR condition with extended regex

```
grep -E "(Olfr141|Olfr143)" Mus_musculus.GRCm38.75_chr1_genes.txt
```

Explanation:

* `-E` → extended regex
* `|` → OR operator

---

🧪 Where This Appears in Real Pipelines
Used in:

* gene family analysis
* receptor / TF filtering
* QC gene lists

---

## SECTION 7: FASTQ Context Matching (`-A`, `-B`)

---

# `-A` (After context)

```
grep -A2 "AGATCGG" contam.fastq
```

Shows:

* match line
* 2 lines after (sequence + quality)

---

# `-B` (Before context)

```
grep -B1 "AGATCGG" contam.fastq
```

Shows:

* FASTQ header
* matching sequence

---

# Color + context

```
grep --color=always -A2 "AGATCGG" contam.fastq
```

---

🧪 Where This Appears in Real Pipelines
Used to:

* detect adapters
* verify contamination
* debug trimming failures

---

## SECTION 8: Sorting Genomic Coordinates (`sort -k`)

---

# Basic sort

```
sort example.bed
```

Sorts **alphabetically**, not biologically meaningful.

---

# Sort by chromosome, then start position

```
sort -k1,1 -k2,2n example.bed
```

Explanation:

* `-k1,1` → chromosome column
* `-k2,2n` → numeric sort on start position

---

# Why `-k` matters

Wrong:

```
sort example.bed
```

Correct:

```
sort -k1,1 -k2,2n example.bed
```

---

🧪 Where This Appears in Real Pipelines
Required before:

* `bedtools intersect`
* `bedtools merge`
* peak calling

---

## SECTION 9: `uniq` vs `sort | uniq`

---

# `uniq` alone

```
uniq letters.txt
```

Only removes **adjacent duplicates**.

---

# Correct deduplication

```
sort letters.txt | uniq
```

---

# Count occurrences

```
sort letters.txt | uniq -c
```

---

🧪 Where This Appears in Real Pipelines
Used for:

* unique gene lists
* deduplicating sample IDs
* QC summaries

---

https://chatgpt.com/s/t_69704dbd3c588191a25f09ab296909da





