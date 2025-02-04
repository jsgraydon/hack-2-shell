# Question 1: Get the data files
*Download the following data files from the internet using the curl command: `http://eaton-lab.org/pdsb/test.fastq.gz` and `http://eaton-lab.org/pdsb/iris-data-dirty.csv`. Use the `less` or `zless` commands to look at each file. Describe what these commands do. Finally, use the `head` command to print the first 5 lines of the file `iris-data-dirty.csv`.*

The following commands were used:

|Command|Description|
|---|---|
|`curl -L -O [URL]`|Downloads data from the indicated URL and saves the file locally. `-L` allows the command to follow redirects (necessary here, as the data appears to have been moved), and `-O` saves the file with its original name (and prevents unnecessary output into the terminal).|
|`head -n -v [file]`|Prints the first `n` lines of the indicated file. `-v` prints the name of the file or dataset at the top of the output.|
|`less [file]`|Opens up a viewer to view the indicated file.|
|`zless [file]`|Opens up a viewer to view the indicated file, allowing for viewing without unzipping a compressed file.|

Input:
```
curl -L -O http://eaton-lab.org/pdsb/iris-data-dirty.csv
head -5 -v iris-data-dirty.csv
```

Results:
```
==> iris-data-dirty.csv <==
5.1,3.5,1.4,0.2,Iris-setosa
4.9,3.0,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5.0,3.6,1.4,0.2,Iris-setosa
```
# Question 2: Clean the data
*Use 'grep', 'uniq', and 'sed' for this question. Check that all of the species names are spelled correctly in the file 'iris-data-dirty.csv'. Also check for missing values stored as 'NA'. Create a new file where mispelled names are replaced with the correct values, and lines with 'NA' are excluded, and save it as 'iris-data-clean.csv'. Use 'cut', 'sort' and 'uniq' to list the number of data values there are for each species in the new cleaned data file. Describe your work.*

### Check species names
Input:
```
cut -d',' -f5 iris-data-dirty.csv | grep -v '^$'| sort | uniq -c
```

Results:
```
     49 Iris-setosa
      1 Iris-setsa
     49 Iris-versicolor
      1 Iris-versicolour
     50 Iris-virginica
```

*Explanation:* The piped command, `cut -d',' -f5 iris-data-dirty.csv | grep -v '^$'| sort | uniq -c` first 1) extracts the 5<sup>th</sup> column, 2) removes any blank items in the column, 3) sorts the results alphabetically, and then 4) returns the counts of unique entries. From these results, we see that there are 49 or 50 entries for the correct spellings and two instances of misspelled names (Iris-setsa and Iris-versicolour).

### Check for NA
Input:
```
grep NA iris-data-dirty.csv
```

Results:
```
6.3,NA,4.9,1.5,Iris-versicolor
5.6,NA,4.1,1.3,Iris-versicolor
```

*Explanation:* The `grep` command looks for any `NA` values present in the dataset, showing there were two NA values in the second column.

### Create clean dataset
Input:
```
sed '/NA/d; s/Iris-setsa/Iris-setosa/; s/Iris-versicolour/Iris-versicolor/' iris-data-dirty.csv > iris-data-clean.csv
```

*Explanation:* The `sed` command performs a variety of functions, here removing rows with `NA` values, replacing the misspelled flower names, and writing the new dataset to a file called `iris-data-clean.csv`.

### Explore new dataset
Input:
```
cut -d',' -f5 iris-data-clean.csv | grep -v '^$'| sort | uniq -c
```

Results:
```
     50 Iris-setosa
     48 Iris-versicolor
     50 Iris-virginica
```
*Explanation:* The `cut` command again extracts the 5<sup>th</sup> column from the new clean dataset, `grep` removes any blank rows, `sort` orders the resulting names, and `uniq -c` returns the count of each unique value in the data.

# Question 3: Find a sequence
*Find how many lines in the data file 'test.fastq.gz' start with "TGCAG" and end with "GAG". Describe your work.*

Input:
```
zgrep -c '^TGCAG.*GAG$' test.fastq.gz
```

Results:
```
44
```

*Explanation:* First, the `zgrep` function must be used because the file, `test.fastq.gz`, is compressed; `grep` will not process the file properly. For both `grep` and `zgrep`, `-c` indicates that only the count of items that match the result should be returned. The regex pattern used, `^TGCAG.*GAG$`, looks for any lines that start with TGCAG and end with GAG — `^` indicates the pattern is at the start of a string, `$` indicates that pattern is at the end, and `.*` is a wildcard filler that allows for any characters between the two.

# Question 4: Find a sequence chunk
*Using 'grep' and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.* 

Input:
```
zgrep -n -A 1 -B 2 AAAACCCC test.fastq.gz
```

Results:
```
2511-GGGGHHHHHHHHHDAGGGEDFDFEGE;GAA>EEFDDGGEHHGHDGG>GGG@DEDDEDEFGGGD@>ECEBBGGGD
2512-@32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
2513:TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
2514-+32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
--
3935-IIIIIIIIIGIIIIIIHIIIEIIIIIFIIIIEIIIIIIIIHEGIIHIIHIIIIIHIIIHI?FEEFC8HBBEE@E
3936-@33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
3937:TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC
3938-+33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
```

*Explanation:* The command, `zgrep`, is used again as the file is compressed. The option `-n` prints the line number of the returned line along with the content (useful here to demonstrate that the extracted chunks are correct), `-A n` returns `n` lines above the matched line, and `-B n` returns `n` lines below it. Finally, the pattern is very simple, as it can appear anywhere in the string.
