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

Input:
```
command
```

Results:
```
Solution
```

*Explanation:*


# Question 3: Find a sequence
*Find how many lines in the data file 'test.fastq.gz' start with "TGCAG" and end with "GAG". Describe your work.*

Input:
```
command
```

Results:
```
Solution
```

*Explanation:*

# Question 4: Find a sequence chunk
*Using 'grep' and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.* 

Input:
```
command
```

Results:
```
Solution
```

*Explanation:*
