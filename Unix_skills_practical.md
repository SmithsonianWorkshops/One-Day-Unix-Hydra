##Basic Unix Command Line Skill

***Note:*** Lines that start with a `$` should be entered on the command line without the  `$`

**1. Download and navigate to the workshop data**

- Open your terminal window. On a Mac, this is contained in the Utilities folder within your Applications folder. On Unix, it should be in your Applications folder as well.
- `cd` into the directory that contains the workshop `.zip` file (on a Mac, this is likely to be `~/Downloads`)
- Enter the command `unzip STRI_07_2015-master.zip`
- Now `cd` into the new `STRI_07_2015-master` directory

**2. Make a directory and copy your contents into it**

- Make a new directory called `sequences`
- Copy both sequence files into your `sequences` directory
- Change directories into your `sequences` directory
- Now print the path of the directory you are in to the screen

**3. Viewing file contents**

- Try all four of these commands to examine the file `sequences.fa`
    
    a)  `$ cat sequences.fa`
    
    b)  `$ head sequences.fa`
    
    c)  `$ tail sequences.fa`
    
    d)  `$ less sequences.fa`   (Remember, `q` is quit in less)
    
- Use `less` to *find the sequences* that contains the description EAS20\_8\_6\_1\_5_388 (Type `h` in less for help screen, look for “Search forward…” in the “SEARCHING” section.).
    
- Use `head` to display the first sequence only (first two lines). Type `$ man head` (or Google) to find the option to limit the number of lines `head` displays (man pages open in `less`, use the arrow keys to navigate and `q` to quit).
    
- Use `tail` and wildcard globbing to display the last 10 lines *of the two sequences files* (`sequences.fa` and `sequences.fq`) with one command.

**4. Edit a file with `nano`**

- Open `sequences.fa` with `nano`
- Change some bases in one of the sequences.
- Save and close the file
*Remember in* `nano` *shortcuts are at the bottom of the screen and* `^` *is the control key.*

**5. Create a new text file and delete it**

- `$ nano newfile` If the filename doesn’t exist, an empty document will be opened.
- Enter some text into the file, save it and exit.
- Use one of the text viewing commands we used in Step 5 to view your new file’s contents.
- After viewing your file delete it with the `rm` command

###Using Core utilities

> A. grep: globally search a regular expression and print

**Try these:**

- `man grep`
Everything you need to know.

- `grep 'EAS' sequences.fa`
- `grep 'Contig' sequences.fa`
- `grep 'contig' sequences.fa`

- grep -i : grep,case insensitive
`grep -i 'contig' sequences.fa`

- grep -r: recursive grep
`grep -r 'Contig' ./`

- You may also combine options, e.g.:
`grep -ir 'conTIG' ./`

- grep -A: show lines after the grep hit
`grep -i -A 1 'contig' sequences.fa` Useful if the sequences are in one line.

    - Also try:
        - `grep -B 10 '<searchterm>' <filename>` (10 lines Before)
        - `grep -C 5 '<searchterm>' <filename>` (5 lines of Context)
        - Combinations are possible: `grep -A 1 -B 2 '<searchterm>' <filename>` (1 line After, 2 lines Before the hit)

>   B. Output redirection: > (write to file)

- `grep 'Contig' sequences.fa > selection.fa`

Creates a file, if it does not exist.
Then writes the output into the file. *Caution: existing content will be overwritten. Do not write to the same file that you read from.*

- `grep 'Contig' sequences.fa >> selection.fa`

Creates a file, if it does not exist.
Appends the output to the end of the file.
Existing content is not overwritten. Rather, the file grows. (You should still not write to the same file that you read from.)

### Working with FASTA files

>   A. `wc`: count number of lines, words, characters

- `wc` → word count. `-l ` (for counting lines) is the most commonly used option. More information: `man wc`.

- **Exercise: sequence count**

    - Find a way to output the number of Sequences in a FASTA file.
You may use ‘temporary' files.
Tip: individual sequences are identified by their headers. Header lines in a FASTA file begin with ‘>'.

> B. Output redirection: | (pipe)

- `ls -1 | wc -l`

‘Pipes' the output of  `ls -1` into the input of `wc -l`. Prints out the number of items in this directory.
No temporary file necessary.

- `grep '>' sequences.fa | less`
    - 'Pipes' the output of grep into the input of less.
    - Allows to comfortably read the list of headers in a FASTA file.
    - No temporary file necessary.
    - Displayed using `less`:
Search using `/`
Quit with `q`

- **Exercise: filter;**

Output the first five headers in a FASTA file

- **Exercise: one specific line;**

Output line number 19 of a given file.

- **Exercise: collecting sequences;**

Save the first three and the last three headers from a FASTA file in a new file.
_Hint_: you may use two commands


### Managing Files

> A. tar: tape archive

`tar -cvf new_tar_archive.tar <list>`

- `tar` → tape archive
- `c`: create new archive
- `v`: verbose output
- `f`: file to use; can take files and directories as arguments

- **Exercise: create and unpack a tar archive;**

    1. Create a tar archive of your sequences.fa file
    2. Show the content of a tar archive
    3. Unpack a tar archive

> B. gzip: compress files

- **Exercise: compress and decompress fasta file using gzip;**

    Check the size of the sequences.fa file:
    - `ls -lh sequences.fa`
    
    Now compress it using gzip
    
    After it is compressed check the size of the file:
    - `ls -lh sequences.fa.gz`

    Compression reduces file size. Useful for copying over network, Mail attachments, storing on removable media, . . .

    Now decompress the fasta file



#Digging deeper (optional for more advanced users):#
1.  Want to learn the `vi` editor? Enter the command `$ vimtutor` for a tutorial. To exit type `:q`
2.  Want to learn the `emacs` editor? Do the emacs tutorial by starting `$ emacs` and then `<control+h>` and then `t`. To exit type `<control+x>` and then `<control+c>`.
3.  Using  `$ ls -l` Compare the creation date of the original file and the one you copied. Check the `cp` man page or online resources to learn how to copy the file and preserve the original create date.
4.  When you use `less` to view the fasta file, the lines automatically wrap ("fold" in `less` lingo) which can be annoying for fasta files with long sequences. What flag for less will stop this so the lines don't wrap ("chop" in `less` lingo)? Hint: this is an option when starting `less` rather than a command when `less` is already running.
5. `sed`: stream editor

    - Add taxon name to end of all header lines:

    `sed 's/>.*/& Escherichia coli/' sequences.fa > outfile_sed.fa`

6. `awk`: 'awk' stands for the names of its authors “Aho, Weinberger, and Kernighan”
    
    - Clean up a fasta file so only first column of the header is outputted:

    `awk '{print $1}' outfile_sed.fa > output_awk.fa`