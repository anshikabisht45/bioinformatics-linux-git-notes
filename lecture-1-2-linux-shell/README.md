## 1️⃣ Why Linux is Essential in Bioinformatics

Bioinformatics data is:
- large (GBs to TBs)
- remote (HPCs, cloud, servers)
- processed in **pipelines**, not GUIs

Most tools:
- do **not** have graphical interfaces
- expect **text input**
- output **text files**

Linux is the environment where:
- data flows
- tools chain together
- analysis becomes reproducible

If you can’t use Linux:
- you can’t debug pipelines
- you can’t scale analysis
- you can’t collaborate properly
- you can’t collaborate properly

---

## 2️⃣ 🧠 Mental Model to Keep Forever

Think of Linux as **three things only**:

- a             **file system**
- commands that **read and write text**
- .             **streams of data flowing between commands**

Everything else is built on top of this.

---

## 3️⃣ What is Unix / Linux?

- Unix: a family of operating systems
- Linux: an open-source Unix-like OS

Linux is:
- stable
- scriptable
- scalable
- designed for automation

That’s why:
- servers run Linux
- HPCs run Linux     -An HPC (High-Performance Computing) server is a powerful, specialized system
  .                   designed to process massive datasets and complex calculations at extremely high speeds, often by linking many
                      processors (CPUs/GPUs) in parallel within clusters to solve problems too large or time-consuming for
                      standard computers
- bioinformatics runs on Linux

---

## 4️⃣ What is a Unix Shell?

A Unix shell is:
- a **command-line interpreter**
- that lets you talk directly to the OS
- using **text commands instead of buttons**

### Flow
You type → shell interprets → OS executes → output returns
### Common shells
- `bash` (used in this course)
- `zsh`
- `sh`

---

## 5️⃣ Why Linux Instead of UI Tools?

UI tools:
- hide what’s happening
- don’t scale
- break reproducibility
- fail on large datasets

Linux:
- is explicit
- logs everything
- can be automated
- works the same everywhere

In bioinformatics:
> reproducibility > convenience

---

## 6️⃣ File System Basics

Linux follows a **hierarchical file system**.
Important ideas:
- Everything is a **file**
- Directories are just files that hold files
- Paths can be:
  - absolute (`/home/user/file.txt`)
  - relative (`data/file.txt`)

Key commands:
```bash
pwd     # where am I?
ls      # what is here?
cd      # move between directories
```

7️⃣ Core Navigation Commands
pwd     Prints current working directory.
        Why it matters:
        pipelines break if paths are wrong

ls
ls -l     # long format
ls -a     # show hidden files like {.gitignore ones}

cd        Change directory.
         Mistake I made:
         trying to cd into files instead of directories
         cd folder_name	Enter a specific directory
cd ..	   Go back one level
cd ~   	Go to home directory
cd -  	Go to the previous directory you were in
cd /	  Go to the root directory (the top-most level)
Pro Tip: If your folder name has spaces, wrap it in quotes: cd "folder name"

touch    touch does not add content
         it only creates an empty file
         
				 touch file.txt
cat     Viewing file contents
     
				cat file.txt

9️⃣ Streams, Redirection, and Pipes

Linux commands work with streams.
Three standard streams
```
stdin → input
stdout → normal output
stderr → error output
```
In Linux, streams are pre-connected communication channels between a program and its environment. They act as a "conveyor belt" for data, typically text, allowing
programs to receive input and send output sequentially without needing to know the specific hardware source or destination. 
The Three Standard Streams
Every program started in a Linux shell is automatically assigned three standard streams, each associated with a unique File Descriptor (FD): 
Standard Input (stdin) - FD 0: The data source for a program. By default, it is connected to your keyboard.
Standard Output (stdout) - FD 1: The destination for a program's normal results. By default, it is connected to your terminal screen.
Standard Error (stderr) - FD 2: A separate channel for error messages or diagnostics. It also defaults to the terminal screen, which allows you to see errors even if normal output is moved elsewhere. 

REDIRECTION

>   # overwrite output
>>  # append output
2>  # redirect errors

ls file1 file2 > output.txt 2> error.txt

PIPES    (|)  |||||||||||||||||||||||||||||

Pipe sends output of one command as input to another.
~~~
cat file.txt | grep "gene"
command A → pipe → command B
~~~
---------------

LESS LESS LESS LESS LESS LESS LESS LESS LESS

Used for large files.
THINK OF IT LIKE TURNING DATA INTO PAGES OF A BOOK FOR EASY READABILITY
~~~
cat bigfile.txt | less
~~~
Why it matters:

prevents terminal flooding
allows scrolling
essential for FASTA / GTF files

Exit with:
~~~
q
~~~

0️⃣ Common Beginner Mistakes I Made
confusing ls with cat
expecting touch to add content
forgetting where output was redirected
typing wrong quotes in grep
not understanding why commands “did nothing”

🧪 Where This Appears in Real Pipelines
Linux fundamentals appear everywhere:
FASTQ / FASTA preprocessing
filtering reads with grep
counting entries using wc
inspecting GTF annotations
chaining tools using pipes
running jobs on HPC clusters
automating workflows with scripts
If Linux basics are weak:
pipelines become guesswork instead of reasoning

Interview Questions (Bioinformatics / Data Scientist)

           Conceptual
Why is Linux preferred over GUI tools in bioinformatics?
Explain the mental model of Linux in one minute.
What are stdin, stdout, and stderr?
Why are pipes important in data processing?
How does Linux enable reproducibility?

            Practical
Difference between ls and cat?
Difference between > and >>?
What happens when you pipe cat file | less?
How would you inspect a 10GB FASTA file safely?
How do you separate errors from output?

          Applied (company-style)
How would you debug a broken pipeline on a remote server?
How does Linux help with large clinical datasets?
Why is shell knowledge critical for scalable ML pipelines?
How do Linux logs help in production systems?
How does understanding streams improve pipeline design?

chatgpt link for ALL the answers above
https://chatgpt.com/s/t_695a6b80d09c8191befdf357c2677b67

commands i practiced

❤️Creating Files & Directories

mkdir stcc_demo_1
mkdir demo_2
mkdir demo3
mkdir -p genomics-project/{data/sequence,script,analysis}    
``` 
 mkdir -p creates nested directories
```
touch stcc_file.csv
touch stcc_file2.tsv

❤️Writing & Appending to Files

echo "text" > file.txt
echo "text" >> file.txt

```
> → overwrite
>> → append
```

❤️Viewing File Contents
cat file.txt
less file.txt         turn data into pages view
head file.txt
head -n 2 file.txt    show top 2 only
tail -n 2 file.txt    show bottom 2 only


Why this matters
cat for small files
less for large files (safe viewing)
head/tail for sampling
🧪 Real pipelines
Inspecting FASTA, FASTQ, GTF, logs

❤️Pipes & Redirection (Core Shell Concept)
command1 | command2
command > output.txt
command >> output.txt
command 2> error.txt

❤️Copying, Moving & Deleting Files
                   COPY
# Copy a file to another file:
~~~
cp file1.txt file2.txt
~~~

# Copy a file to a directory:
~~~
cp file.txt /path/to/directory/
~~~

# Copy multiple files:
You can list several files followed by a destination directory.
~~~
cp file1.txt file2.txt /path/to/directory/
~~~

# Copy an entire directory:
~~~
cp -r source_dir/ destination_dir/
~~~
                        
												MOVE
mv
# Rename a file or directory:
~~~
mv old_name.txt new_name.txt
~~~
# Move to a specific directory:
~~~
mv file.txt /home/user/Documents/
~~~
# Move and rename simultaneously:
~~~
mv intro.txt manual/chap1.txt (Moves intro.txt into the manual folder and renames it to chap1.txt).
~~~
# Move multiple files:
You can list several files followed by a destination directory.
~~~
mv file1.txt file2.txt /destination_folder/
~~~
Use wildcards:

Move all files of a certain type.
~~~
mv *.jpg /home/user/Pictures/
~~~
                      REMOVE
# Delete a single file:
~~~
rm file.txt
~~~

# Delete multiple files:
~~~
rm file1.txt file2.txt
~~~

# Delete a directory and its contents:
~~~
rm -r directory_name
~~~

# Forcibly delete a directory:
~~~
rm -rf directory_name
~~~
(Note: This is highly dangerous and should be used with caution.)

# Delete files with confirmation:
~~~
rm -i file.txt
~~~

rm file.txt
rm -rf directory/
rmdir empty_directory

**Brace Expansion (Advanced Shell Feature)**

~~~
echo languages-{python,r,rust}
~~~
Why this was cool---
One command → multiple outputs
Used later for batch file creation

1️⃣1️⃣ Finding Files

# Find files by name:
~~~
find . -name "*.fastq"
~~~

Why this matters

Real projects contain hundreds/thousands of files  
find is essential for automation

---

1️⃣2️⃣ Command Help & Documentation

# View manual pages:
~~~
man cp
man grep
man less
~~~

Using manuals instead of Googling blindly


### Basic Linux Commands (Quick Reference)

# `ls`
Lists files & folders  
Why it matters: Basic navigation

# `ls -l`
Lists files with details  
Why it matters: Shows permissions & file sizes

# `ls -a`
Shows hidden files  
Why it matters: Reveals `.git`, `.ssh`, config files

# `cd <dir>`
Changes directory  
Why it matters: Core navigation skill

# `cd ..`
Moves one level up  
Why it matters: Prevents directory confusion

# `mkdir <dir>`
Creates a directory  
Why it matters: Organizing projects

# `mkdir -p a/b/c`
Creates nested directories  
Why it matters: Standard pipeline structure

# `touch file.txt`
Creates an empty file  
Why it matters: Initializes data/log files

# `echo "text" > file`
Writes text (overwrite)  
Why it matters: Controlled file writing

# `echo "text" >> file`
Appends text  
Why it matters: Safe logging without data loss

# `cat file`
Prints file content  
Why it matters: Quick inspection of small files

# `less file`
Scrollable file viewer  
Why it matters: Safely view large files

# `head file`
Shows first 10 lines  
Why it matters: File sanity checks

# `head -n 2 file`
Shows first N lines  
Why it matters: Metadata inspection

# `tail -n 2 file`
Shows last N lines  
Why it matters: Checking file endings/logs

# `command1 | command2`
Pipes output between commands  
Why it matters: Core Unix data-flow concept

# `command > out.txt`
Redirects output  
Why it matters: Saving results

# `command >> out.txt`
Appends output  
Why it matters: Incremental logging

# `command 2> err.txt`
Redirects errors  
Why it matters: Debugging pipelines

# `cp src dest`
Copies files  
Why it matters: Data duplication without risk

# `mv old new`
Moves or renames files  
Why it matters: Dataset organization

# `rm file`
Deletes a file  
Why it matters: Cleanup (use carefully)

# `rm -rf dir`
Force deletes a directory  
Why it matters: Dangerous but sometimes necessary

# `rmdir dir`
Deletes empty directory  
Why it matters: Safe directory cleanup

# `find . -name "*.fastq"`
Finds files by pattern  
Why it matters: Locating sequencing data

# `man <command>`
Opens manual page  
Why it matters: Self-debugging & independence

# `echo a-{b,c,d}`
Brace expansion  
Why it matters: Batch file creation



















