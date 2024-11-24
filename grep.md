# Learn the `grep` Command



## 1. What is `grep`?

`grep` is a powerful command-line utility for searching plain-text data for lines matching a regular expression. It is commonly used in Linux and Unix-based systems.



---



## 2. Basic Syntax

```bash

grep [OPTIONS] PATTERN [FILE...]

    PATTERN: The text or regular expression to search for.
    FILE: The file(s) to search in. If no file is specified, grep reads from standard input.
```
## 3. Basic Usage Examples


- **Search for a specific word in a file:**
  ```bash
  grep "word" filename.txt
  ```


- **Search case-insensitively:**
  ```bash
  grep -i "word" filename.txt
  ```


- **Search recursively in directories:**
  ```bash
  grep -r "word" /path/to/directory
  ```

- **Show line numbers with matches:**
  ```bash
  grep -n "word" filename.txt
  ```



- **Count the number of matches:**
  ```bash
  grep -c "word" filename.txt
  ```



## 4. Advanced Usage

- **Search for multiple patterns:**
  ```bash
  grep -E "pattern1|pattern2" filename.txt
  ```
    

- **Search for lines NOT matching a pattern:**
  ```bash
  grep -v "word" filename.txt
  ```


- **Search and print only filenames with matches:**
  ```bash
  grep -l "word" *.txt
  ```


- **Print filenames without matches:**
  ```bash
  grep -L "word" *.txt

  ```

    
## 5. Contextual Searches

- **Show lines before and after a match:**
  ```bash
  grep -A 3 -B 3 "word" filename.txt
  ```


- **Show lines before the match:**
  ```bash
  grep -B 5 "word" filename.txt
  ```


- **Show lines after the match:**
  ```bash
  grep -A 5 "word" filename.txt
  ```



## 6. Regular Expressions with grep

    

- **Match lines starting with a word:**
  ```bash
  grep "^word" filename.txt
  ```

- **Match lines ending with a word:**
  ```bash
  grep "word$" filename.txt
  ```


- **Match lines containing a digit:**
  ```bash
  grep "[0-9]" filename.txt
  ```


- **Match lines with exactly 3 characters:**
  ```bash
  grep "^...$" filename.txt
  ```


    

## 7. Combining grep with Other Commands

    
- **Search command output:**
  ```bash
  ps aux | grep "process_name"
  ```

- **Filter files in a directory:**
  ```bash
  ls -l | grep ".txt"
  ```

- **Filter out lines starting with a semicolon (;), including those with leading spaces or tabs, and then remove leading spaces from the remaining lines:**
  ```bash
  grep -v '^[[:space:]]*;' filename | sed 's/^[[:space:]]*//'
  ```
    

## 8. Useful Options

- **-o: Print only the matching parts of the line.**
  ```bash
  grep -o "word" filename.txt
  ```
    

- **--color: Highlight matches in the output.**
  ```bash
  grep --color "word" filename.txt
  ```

- **-R: Recursively search directories, following symbolic links.**
  ```bash
  grep -R "word" /path/to/directory
  ```

 

## 9. Practical Examples

- **Find IP addresses in a log file:**
  ```bash
  grep -Eo "([0-9]{1,3}\\.){3}[0-9]{1,3}" logfile.txt
  ```
    

- **Find email addresses in text:**
  ```bash
  grep -Eo "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
  ```


- **Find lines from a text file that have between m and n characters:**
  ```bash
  grep -E '^.{m,n}$' file.txt
  grep -E '^.{m,n}$' file.txt | grep -v '[[:space:]]'
  grep -E '^[^[:space:]]{m,n}$' file.txt
  ```

 


- **Find failed login attempts:**
  ```bash
  grep "failed password" /var/log/auth.log
  ```
